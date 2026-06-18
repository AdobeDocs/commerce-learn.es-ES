---
title: 'POC de pagos fraccionados: decisiones de arquitectura y diseño'
description: Descubra cómo el POC de pagos divididos sincroniza el cierre de compra con Commerce y los pasos impulsados por I/O con App Builder, con atributos de extensión, REST y cinco casos extremos de complementos.
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# POC de pagos divididos: decisiones de arquitectura y diseño

Esta página explica las opciones arquitectónicas detrás de la prueba de concepto de pago dividido. Léalo antes de usar los indicadores de generación de esta serie, para que entienda cómo está estructurado cada componente y cómo adaptar los patrones en su propio proyecto.

## El principio básico

La prueba de concepto no es acerca de la implementación de pago fraccionado más elegante. Se trata de mostrar **cómo empezar a mover la lógica de Commerce a App Builder sin una reescritura de big bang**.

La regla que se aplica en es la siguiente:

> **Si algo debe ejecutarse sincrónicamente en el ciclo de petición de Commerce o debe invocar las API internas de Commerce que no tienen una superficie externa limpia, permanece en PHP. Todo lo demás se mueve a App Builder.**

## Qué vive en Commerce (PHP) y por qué

### &#x200B;1. Solicitud de crédito de tienda: `PlaceOrderPlugin`

El crédito de tienda se ha aplicado al carro de compras usando `Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`. Este método solo funciona en un carro de compras **activo**. El carro de compras quedará inactivo en el momento en que se realice el pedido. App Builder recibe el evento de E/S *después de* de realizar el pedido, por lo que no es posible aplicar crédito de tienda desde App Builder.

**La lección:** Cualquier cosa que deba cambiar el estado del carro de compras antes de realizar el pedido debe ejecutarse en Commerce. No hay solución.

### &#x200B;2. Guardia de umbral sincrónica: `CheckoutPlugin`

La comprobación del umbral de pedido de 100 $ debe bloquear al cliente en el paso de pago antes de que seleccione **[!UICONTROL Place Order]**. La respuesta debe ser sincrónica en el ciclo de solicitudes de Commerce. App Builder es controlado por eventos y asincrónico, por lo que no puede devolver un error inmediato en ese momento.

App Builder *also* valida el umbral (como una auditoría), pero la experiencia del cliente depende de que la comprobación de Commerce se ejecute primero.

### &#x200B;3. Extremos REST personalizados: `webapi.xml` y `SplitPaymentManagement`

Los siguientes extremos deben:

* Invocar `SplitInvoiceService` (facturas que utilizan el servicio de factura interna de Commerce)
* Invocar `ShipOrder::execute()` (servicio de envío interno de Commerce)
* Actualizar el estado y el estado del pedido con el equipo de estado del pedido de Commerce

`/V1/split-payment/orders/:id/cash-received` y `/V1/split-payment/orders/:id/cash-decline`

No hay ninguna capa REST pública limpia para ese comportamiento, por lo que Commerce expone los extremos. App Builder los llama.

### &#x200B;4. Dividir cantidades en presupuesto y pedido: observadores y complementos

Commerce necesita las cantidades divididas (`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`) en el pedido, tanto para las respuestas de REST que App Builder lee como para la vista de pedidos **[!UICONTROL Admin]**. Las cantidades se adjuntan con atributos de extensión y se copian de la oferta al pedido en Observer de `sales_model_service_quote_submit_before`.

## Qué vive en App Builder y por qué

### &#x200B;1. Procesamiento de pedidos impulsado por eventos: `payment-orchestrator`

Después de que `sales_order_place_before` se active, App Builder recibe el evento. Vuelve a validar el umbral (como una auditoría), registra un comentario de *efectivo pendiente* en el pedido y actualiza el estado del pedido. Nada de eso requiere PHP nuevo, solo REST de nuevo en Commerce.

### &#x200B;2. Aceptación de efectivo: `payment-accept`

Cuando un ERP (o un operador del panel) confirma que se recibió efectivo, `payment-accept` llama a `POST /V1/split-payment/orders/:id/cash-received`. La factura, el envío y el estado del pedido se gestionan en Commerce. App Builder es el déclencheur.

### &#x200B;3. Declive de efectivo: `payment-decline`

`payment-decline` llama a `POST /V1/split-payment/orders/:id/cash-decline` y Commerce cancela el pedido. Mismo patrón que la aceptación de efectivo.

### &#x200B;4. Panel del operador: `demo-dashboard`

Un tablero de HTML independiente servido desde una acción web de App Builder. Obtiene pedidos que están esperando el efectivo de Commerce REST y proporciona **[!UICONTROL Accept]** / **[!UICONTROL Decline]** acciones que llaman a las acciones de App Builder anteriores. No se requiere Commerce **[!UICONTROL Admin]**.

## El umbral: se aplica dos veces a propósito

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerce es propietario del protector de cara al usuario; App Builder es propietario de la auditoría posterior a la ubicación.** Eso es intencional.

## El crédito de la tienda: por qué permanece en PHP

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

El archivo `store-credit.js` de la orquestadora documenta esto. Es un código auxiliar que no funciona y contiene comentarios que explican por qué no se utiliza.

## Atributos de extensión: el pegamento

Los importes divididos se mueven a través del sistema en atributos de extensión:

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## Modelo de datos

**`sales_order`columnas planas que este módulo agrega**

| Columna | Tipo | Finalidad |
| --- | --- | --- |
| `split_store_credit_amount` | float | Crédito de almacén aplicado |
| `split_cash_amount` | float | Importe en efectivo adeudado |
| `split_cash_status` | varchar | `pending`, `received` o `declined` |
| `split_sc_invoice_id` | int | ID de entidad de la factura de crédito de tienda |
| `split_cash_invoice_id` | int | ID de entidad de la factura de tesorería |

**Atributos de extensión** (en `CartInterface`, `OrderInterface` y `OrderPaymentInterface`)

* `split_store_credit_amount` (flotante)
* `split_cash_amount` (flotante)
* `split_cash_status` (cadena)

## Campos de carga útil de evento de E/S

`observer.sales_order_place_before` está configurado en `io_events.xml` para incluir lo siguiente en el evento:

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder usa `entity_id` como identificador de pedido y `split_store_credit_amount` y `split_cash_amount` para la validación del umbral.

## Los cinco casos de borde que cubre la prueba de concepto

### 1. `CapCustomerBalanceCollectPlugin`

El recolector total **[!UICONTROL Customer balance]** nativo de Commerce puede aplicar en exceso (puede ver el saldo disponible completo, no la cantidad dividida declarada por la sesión). Este complemento limita la cantidad al valor declarado en la sesión.

### 2. `FixSplitPaymentGrandTotalPlugin`

Una vez aplicado el crédito de la tienda, la oferta **[!UICONTROL Grand Total]** puede caer al importe de solo efectivo. El JavaScript de cierre de compra debe calcular el total del pedido para la validación dividida *antes* de ese cambio. El complemento se ejecuta después de la colección de totales y corrige la presentación, mientras que JavaScript no confía solo en `grand_total` y reconstruye el valor de los segmentos de subtotales.

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

Cuando se recopilan los totales de facturas, el crédito de tienda se puede aplicar dos veces. Este complemento corrige `customer_balance_amount` en las facturas.

### 4. `SplitPaymentZeroTotalPlugin`

Una vez aplicado el crédito de la tienda, el carro de compras **[!UICONTROL Grand Total]** puede ser $0 (pedido de crédito de la tienda completo). La comprobación **[!UICONTROL Zero subtotal checkout]** de Commerce puede bloquear el CÓDIGO en ese caso. Este complemento permite el pago contra reembolso cuando el importe en efectivo de la sesión es mayor que 0.

### &#x200B;5. Recuento de presupuesto antes de `BalanceManagementInterface::apply()`

`apply()` comprueba la cantidad con el **[!UICONTROL Grand Total]** actual. Si el total ya es solo la parte de efectivo, `apply()` puede fallar o limitar. `PlaceOrderPlugin` suspende temporalmente la corrección del total general mientras se aplica el saldo, mediante un marcador de sesión (`beginBalanceApply` / `endBalanceApply`).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

---
title: 'POC de pago dividido: petición de API del módulo Commerce'
description: 'Aprenda a utilizar este símbolo del sistema para generar Client_SplitPayment: REST, complementos, JavaScript de cierre de compra y eventos de E/S, así como para habilitar, compilar e implementar comandos.'
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# POC de pago dividido: petición de API del módulo Commerce

Utilice esta página para copiar el aviso completo que genera el módulo en proceso `Client_SplitPayment`: REST, administración de sesiones, **[!UICONTROL Checkout]** y **[!UICONTROL Admin]** para mostrar la prueba de concepto de pago dividido. El flujo de trabajo del operador permanece en App Builder.

## Cómo utilizar este indicador

Copie todo desde **INICIO DEL MENSAJE** hasta **Final del mensaje** en el cursor (con Claude) o directamente en Claude. Ejecútelo desde la raíz del proyecto de Commerce o desde un directorio en el que la API pueda crear archivos.

## Personalice antes de ejecutar

* Reemplace `Client` con su nombre de proveedor real.
* Cambie `SplitPayment` si desea un nombre de módulo diferente.
* Si el sitio utiliza un tema personalizado, las rutas XML de diseño y RequireJS pueden necesitar cambios.
* Si el método **[!UICONTROL Cash on delivery]** usa un código diferente de `cashondelivery`, actualice `payment-method-helper.js`.


## El indicador

**INICIO DEL MENSAJE**

Está generando un módulo Adobe Commerce 2.4.5+ en proceso completo y listo para la producción para una función de pago dividido. Este módulo es el adaptador PHP delgado que expone la superficie REST correcta y adjunta los datos correctos en los momentos correctos del ciclo de vida de Commerce. Toda la lógica del flujo de trabajo del operador reside en Adobe App Builder (no en este módulo).

**Identidad del módulo:**
* Proveedor: `Client`
* Módulo: `SplitPayment`
* Nombre completo: `Client_SplitPayment`
* Área de nombres: `Client\SplitPayment`
* Ubicación: `app/code/Client/SplitPayment/`
* Dependencias: `Magento_Checkout`, `Magento_CustomerBalance`, `Magento_Sales`, `Magento_Quote`, `Magento_WebApi`, `Magento_AdobeCommerceEventsClient`

Genere todos los archivos enumerados en la siguiente estructura de archivos. No omita ningún archivo. Usar `declare(strict_types=1)` en todos los archivos PHP.


### Estructura de archivo para generar

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### Especificaciones de comportamiento

#### &#x200B;1. Esquema de base de datos (`etc/db_schema.xml`)

Agregar estas columnas a `sales_order` (recurso: `sales`):

| Columna | Tipo | Nullable | Comentario |
|---|---|---|---|
| `split_store_credit_amount` | decimal(12,4) | yes | Almacenar parte de crédito |
| `split_cash_amount` | decimal(12,4) | yes | Parte en efectivo vencida |
| `split_cash_status` | varchar(32) | yes | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | int sin firmar | yes | ID de entidad de factura de crédito de tienda |
| `split_cash_invoice_id` | int sin firmar | yes | Identificador de entidad de factura de efectivo |

Genere también `db_schema_whitelist.json` para estas columnas.

#### &#x200B;2. Atributos de extensión (`etc/extension_attributes.xml`)

Agregar `split_store_credit_amount` (flotante), `split_cash_amount` (flotante), `split_cash_status` (cadena) a:
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. Puntos finales REST (`etc/webapi.xml`)

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

Los tres se asignan a `Client\SplitPayment\Api\SplitPaymentManagementInterface`.

#### &#x200B;4. Eventos de E/S (`etc/io_events.xml`)

Suscribirse a `observer.sales_order_place_before` e incluir campos:
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — Contenedor de sesión

Almacena los importes divididos declarados en la sesión de cierre de compra. Debe exponer:
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — devuelve `['store_credit' => float, 'cash' => float]`
* `clear(): void`
* `beginBalanceApply(): void`: establece un indicador que suprime el complemento de correcciones totales generales durante la solicitud de crédito de tienda
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` — Controlador REST

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* Valida si el carro de compras pertenece a la sesión actual (comparando ID de comillas numéricas y enmascaradas)
* Almacena cantidades en `SplitPaymentSession`
* Devuelve `true` si se realiza correctamente; emite `LocalizedException` con un mensaje genérico si se produce un error

**`markCashReceived(int $orderId): bool`**
* Carga el pedido por `entity_id`
* Valida `split_cash_status === 'pending'`
* Establece estado en `received`, estado en `processing`
* Agrega un comentario de historial: `"Cash payment of $X.XX received."`
* Llamadas `SplitInvoiceService::createCashPortionInvoice($orderId)`
* Agrega un comentario con el identificador de incremento de factura de efectivo
* Llamadas `createShipmentIfPossible($orderId)`: crea un envío con `ShipOrder::execute()` si hay artículos de envío permitido
* Devuelve `true`; lanza `LocalizedException` genérico ante cualquier error

**`markCashDeclined(int $orderId): bool`**
* Orden de carga
* Valida `split_cash_status === 'pending'`
* Valida `$order->canCancel()`
* Establece el estado en `declined`, agrega el comentario: `"Cash payment declined (simulated fraud check)."`
* Guarda el pedido
* Llamadas `OrderManagement::cancel($orderId)`
* Devuelve `true`; lanza `LocalizedException` genérico en caso de error

#### &#x200B;7. `PlaceOrderPlugin` — roundPlaceOrder en `QuoteManagement`

Este es el complemento más importante. Ejecuta `aroundPlaceOrder`:

1. Cargue el presupuesto; si no está activo, llame a `$proceed()` inmediatamente
2. Leer `SplitPaymentSession::getAmounts()`
3. Si `customer_balance_amount_used > 0` aparece en la cita, llame a `BalanceManagementInterface::remove($cartId)` (identificador `LocalizedException`: registrar y volver a generar como mensaje genérico)
4. Llamada `recollectQuoteForBalanceOperation()`: carga el presupuesto, llama a `collectTotals()`, guarda (en `beginBalanceApply()` / `endBalanceApply()` para eliminar el complemento de correcciones totales generales)
5. Si `$storeCredit > 0`, llame a `BalanceManagementInterface::apply($cartId, $storeCredit)` (error de identificador)
6. Volver a cargar el presupuesto; establecer atributos de extensión: `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'`
7. Guardar `payment->additional_information['client_split_payment']` como `['store_credit' => x, 'cash' => y]`
8. Guardar presupuesto
9. Llamar a `$proceed()`, capturar ID de pedido
10. Llamar a `SplitPaymentSession::clear()`
11. Identificador de pedido de devolución

#### &#x200B;8. `CopySplitPaymentToOrder` — Observador en `sales_model_service_quote_submit_before`

Lee los importes divididos de los atributos de extensión additional_information → de pago de sesión → oferta (en ese orden de prioridad). Escribe `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'` en el objeto order.

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — Observador en `sales_order_place_after`

Después de realizar el pedido, si el pedido tiene un importe de crédito de tienda (`split_store_credit_amount > 0`), llame a `SplitInvoiceService::createStoreCreditPortionInvoice($orderId)` para facturar inmediatamente la parte de crédito de tienda.

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
Crea una factura sólo para la parte de crédito del almacén. Establece `customer_balance_amount` en la factura como el importe de crédito de tienda. Registra y guarda la factura. Devuelve el ID de entidad de factura o nulo en caso de error (registro; no volver a generar).

**`createCashPortionInvoice(int $orderId): ?int`**
Crea una factura para la parte de efectivo (los artículos o importes de pedido restantes no cubiertos por la factura SC). Registra y guarda. Devuelve ID de entidad o nulo en caso de error.

#### &#x200B;11. `CheckoutPlugin` — el `PaymentInformationManagement`

Complemento en `Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`. Antes de continuar:
* Cargar el presupuesto de la sesión de cierre de compra
* Obtener el umbral configurado: `Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`, predeterminado `100`
* Si `$quote->getGrandTotal() > $threshold`, arrojar `LocalizedException('Payment could not be processed. Please try again or contact support.')`

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — en `Customerbalance` total

Una vez que se ejecute la recopilación total del saldo de clientes nativos, limite `customer_balance_amount_used` a `SplitPaymentSession::getAmounts()['store_credit']`. Esto evita que Commerce sobreaplique el saldo completo del cliente cuando el cliente ha declarado una parte de crédito de tienda más pequeña.

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — en `Quote\Address\Total\Grand`

Después de cobrar el total general: si existe una sesión de pago dividido y `isBalanceApplyInProgress()` es falso, establezca el total general de la oferta en el importe de efectivo de la sesión. Esto hace que la interfaz de usuario de cierre de compra muestre solo lo que se debe en efectivo.

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — en `Sales\Model\Order\Invoice`

Una vez recopilados los totales de factura, si el pedido asociado de la factura tiene un `split_sc_invoice_id`, corrija el `customer_balance_amount` de la factura para evitar la doble aplicación del crédito de tienda.

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — en `Payment\Model\Checks\ZeroTotal`

Permitir el pago contra reembolso al `SplitPaymentSession::getAmounts()['cash'] > 0`, incluso si el total general de la cotización es de 0 $ (porque el crédito de la tienda cubre todo el pedido).

#### &#x200B;16. `LayoutProcessorPlugin` — en `Checkout\Block\Checkout\LayoutProcessor`

Una vez procesado el diseño:
* Inject the `Client_SplitPayment/js/view/payment/split-payment` component into the `additional` children of the `cashondelivery` payment method component at `components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`
* Remove the native store credit UI component (`customerBalance`, `useStoreCredit`) from the payment step — the split payment component owns store credit display/application

#### 17. `OrderRepositoryPlugin` — on `OrderRepositoryInterface`

After `get()` and `getList()`, hydrate the order&#39;s extension attributes from the flat `sales_order` columns (`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`).

#### 18. `AdminSplitPaymentTitlePlugin` — on `Payment\Block\Info`

After `getTitle()` returns, if the payment method is `cashondelivery` and the order has a split payment, append `" (Split: Cash $X.XX + Store Credit $Y.YY)"` to the title.

#### 19. `OrderPaymentPlugin` — on `Sales\Block\Adminhtml\Order\Payment`

In `_beforeToHtml` or via afterToHtml, append split payment detail (cash amount, store credit amount, status) to the payment block HTML in the Commerce Admin order view.

#### 20. Storefront Store Credit Balance Controller

`Controller/Checkout/StoreCreditBalance.php` — route: `GET /splitpayment/checkout/storecreditbalance`

Returns JSON: `{"balance": float, "logged_in": bool}`. If customer is not logged in, returns `{"balance": 0, "logged_in": false}`. Reads balance from `Magento\CustomerBalance\Model\Balance`.

Register the frontend route in `etc/frontend/routes.xml` with `frontName="splitpayment"`.

#### 21. Checkout KnockoutJS Component — `split-payment.js`

A `uiComponent` that:
* Detects when the `cashondelivery` payment method is selected (`quote.paymentMethod.subscribe`)
* Loads the customer&#39;s store credit balance via `GET /splitpayment/checkout/storecreditbalance`
* Pre-fills the cash amount field with the full order total (calculated from `total_segments` excluding `grand_total` and `customerbalance` — never uses `grand_total` directly since it may be set to the cash remainder)
* As the customer changes the cash amount input: computes store credit needed = order total − cash; validates; posts to `POST /V1/split-payment/set`
* Shows validation messages for: cash > order total, insufficient store credit, not logged in
* Shows a success message when a valid split is entered: `"The remaining $X.XX will automatically be applied from your store credit."`
* Resets when another payment method is selected (posts `{storeCreditAmount: 0, cashAmount: 0}` to clear session)

#### 22. `cashondelivery-method.js`

Extends `Magento_OfflinePayments/js/view/payment/offline-payments`. Uses `payment-method-helper.js` to detect the cash method code. Registra el componente `split-payment` en su región `additional`.

#### 23. `payment-method-helper.js`

La utilidad que devuelve `getCashMethodCode()` — comprueba `window.checkoutConfig.paymentMethods` para `cashondelivery`; vuelve a `checkmo` si es necesario.

#### &#x200B;24. `cashondelivery.html` Plantilla

Plantilla de pago contra reembolso estándar, pero incluye la región `<!-- ko foreach: getRegion('additional') -->` para que se pueda procesar el componente secundario de pago dividido.

#### &#x200B;25. `split-payment.html` Plantilla

Plantilla KnockoutJS para los campos de pago dividido:
* Visualización del saldo de crédito de tienda disponible
* Entrada de importe de efectivo (número, paso 0,01)
* Visualización de la parte de crédito del almacén (solo lectura)
* Mensaje de crédito de almacén de aplicación automática (se muestra cuando la división es válida y el crédito de almacén > 0)
* Mensaje de error de validación

#### 26. `requirejs-config.js`

Mapas:
* `Client_SplitPayment/js/view/payment/split-payment` → el componente
* `Client_SplitPayment/js/view/payment/cashondelivery-method` → la anulación de COD
* `Client_SplitPayment/js/model/payment-method-helper` → el asistente

#### 27. `etc/config.xml`

Valores predeterminados de configuración del sistema:

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### Notas de implementación críticas

**La aplicación de crédito de almacenamiento debe usar `BalanceManagementInterface`, no la manipulación directa del modelo.** `BalanceManagementInterface::apply()` administra automáticamente la sesión, la validación y el cálculo del carro de compras.

**`PlaceOrderPlugin`debe usar `aroundPlaceOrder` (no `beforePlaceOrder`).** El crédito de la tienda debe aplicarse mientras el carro de compras esté activo y debe garantizarse antes de llamar a `$proceed()`.

**El patrón de indicador de sesión para `beginBalanceApply` / `endBalanceApply` es crítico.** Sin él, `FixSplitPaymentGrandTotalPlugin` se ejecuta durante `collectTotals()` dentro de la operación de saldo y establece el total general en el resto de efectivo, lo que provoca que `BalanceManagementInterface::apply()` falle o limite el crédito.

**No exponer nunca los detalles internos del error al cliente.** Los `catch` bloques que aparecen en las respuestas REST deben arrojar `LocalizedException('Payment could not be processed. Please try again or contact support.')`.

**`entity_id`es el identificador numérico de la base de datos.** Las llamadas REST de App Builder siempre usan `entity_id`, no `increment_id`.

**`SplitInvoiceService`debería capturar y registrar errores en lugar de propagarlos.** El error de creación de factura no debe cancelar un pedido ya realizado: registre el error y deje que el administrador lo gestione manualmente.


### Después de generar archivos

Ejecute estos comandos en la raíz del proyecto de Commerce:

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### Final del mensaje


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

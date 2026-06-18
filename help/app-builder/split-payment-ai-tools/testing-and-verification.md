---
title: POC de pagos fraccionados — Guía de prueba y verificación
description: Aprenda a verificar el POC de pagos divididos. Instalación de Commerce, REST, cierre de compra, umbral, aceptación y rechazo de simulación, tablero de demostración y registros de App Builder.
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# POC de pago fraccionado: guía de prueba y verificación

Esta guía le explica cómo verificar que cada componente funciona correctamente, en el orden en que se deben probar. Comience desde abajo (módulo Commerce) y trabaje hacia arriba (App Builder).


## Paso 1: Verificación de la instalación del módulo Commerce

Después de ejecutar `bin/magento setup:upgrade` y `bin/magento setup:di:compile`:

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Resultado esperado: cuatro columnas que comienzan con `split_` visibles en `sales_order`.


## Paso 2: Verificación de la configuración del administrador de Commerce

En Commerce Admin:
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — confirmar que **[!UICONTROL Cash On Delivery]** está habilitado con el título `Cash`
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** — confirmación habilitada
3. Confirme que el cliente de prueba tiene crédito de almacén: **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[cliente]* > **[!UICONTROL Store Credit]**


## Paso 3: Verificar que los puntos finales de REST sean accesibles

Utilice curl para confirmar que los puntos de conexión responden (rechazarán solicitudes no autorizadas, pero un error 401 confirma que se han enrutado correctamente):

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## Paso 4: Prueba de la IU de cierre de compra

1. Inicie sesión en la tienda como cliente de prueba (que tiene crédito de tienda)
2. Añadir un producto al carro de compras (total menos de 100 $ después del envío + impuestos)
3. Pasar al cierre de compra
4. En el paso de pago, selecciona **Pago contra reembolso** (o Pago contra reembolso)
5. Compruebe que aparecen los campos de pago dividido:
   * Se muestra el saldo de crédito de la tienda
   * Campo de importe en efectivo rellenado previamente con el total del pedido
   * Campo &quot;Crédito de tienda para este pedido&quot; que muestra 0,00 $ (ya que efectivo = total del pedido completo)
6. Reduzca la cantidad en efectivo (por ejemplo, introduzca 10 $ para un pedido de 50 $)
7. Verificar las actualizaciones de la parte de crédito de la tienda a 40,00 $
8. Compruebe que aparece el mensaje: `"The remaining $40.00 will automatically be applied from your store credit."`

**Validación de prueba:**
* Introduzca un importe en efectivo mayor que el total del pedido → mensaje de error
* Introduzca un importe en efectivo que requiera más crédito de tienda que el disponible → mensaje de error
* Introducir efectivo = 0 error de → (o el crédito de tienda cubre el pedido completo)


## Paso 5: Probar la protección del umbral

1. Agregar productos por un total de más de $100 (subtotal + envío + impuesto > $100)
2. Continuar con el cierre de compra, seleccionar **Efectivo**
3. Intento de realizar el pedido
4. Compruebe que aparece el mensaje de error: `"Payment could not be processed. Please try again or contact support."`
5. Compruebe que el carro de compras se ha conservado (el cliente aún puede ajustarlo e inténtelo de nuevo)


## Paso 6: Realizar una orden de pago dividida de prueba

1. Crear un carro de compras por debajo de 100 $ (cliente conectado con crédito de la tienda)
2. Seleccionar Efectivo al pagar
3. Introduzca un importe en efectivo menor que el total del pedido (por ejemplo, 10 $ de un pedido de 45 $)
4. Confirme que aparece el mensaje de crédito de la tienda
5. Haga clic en **Realizar pedido**

Después de realizar el pedido, compruebe en el Administrador de Commerce:
* El pedido está en estado `pending_payment`
* El pedido tiene dos comentarios del historial:
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` (de App Builder `payment-orchestrator`)
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` (de App Builder)
* Los importes de los pagos fraccionados se pueden ver en el bloque de pagos del pedido

> **Si no aparecen comentarios de App Builder:** Compruebe los registros de acciones de App Builder con `aio app logs`. Es posible que el evento no se haya activado o que la acción tenga un error.


## Paso 7: Prueba de aceptación mediante script de simulación

El script de simulación es la forma más rápida de probar el flujo de aceptación/rechazo sin la interfaz de usuario del operador completa.

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

Después de aceptar, verifique en la vista de orden de administración de Commerce:
* El estado del pedido es `processing`
* Comentario de historial: `"Cash payment of $X.XX received."`
* Factura de efectivo creada (visible en la pestaña Facturas)
* Envío creado (visible en la pestaña Envíos, si corresponde)
* Comentario de historial: `"Split payment: cash portion invoiced #XXXXXXXX."`
* Comentario de historial: `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## Paso 8: Rechazo de prueba mediante script de simulación

Realice otro pedido de prueba (la misma configuración que en el paso 6) y, a continuación:

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

Después del rechazo, compruebe en Administración de Commerce:
* El estado del pedido es `canceled`
* Comentario de historial: `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## Paso 9: Prueba del tablero de demostración

Después de implementar `split-payment-orchestrator`, `aio app deploy` imprime las direcciones URL de acción.

Abra la URL `demo-dashboard` en un explorador:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

Si `DEMO_UI_SECRET` está establecido:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

Con un pedido pendiente:
1. El panel debe mostrar el orden en la lista de pendientes
2. Haga clic en **Aceptar** → orden debe pasar a `processing` en Commerce
3. Realice otro pedido; haga clic en **Rechazar** → el pedido debe ser `canceled` en Commerce


## Paso 10: Prueba de registros de acciones de App Builder

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

Para `payment-orchestrator`, busque:

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

Para `payment-accept` o `payment-decline`:

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## Problemas y correcciones comunes

### &quot;La firma no es válida&quot; desde Commerce OAuth

**Causa:** Diferencia de reloj entre el tiempo de ejecución de App Builder y Commerce o un error de firma de OAuth.

**Corrección:**
* Confirmar que `COMMERCE_BASE_URL` no tiene barra diagonal
* Confirme que las cuatro credenciales de OAuth son para una integración de _activated_
* Pruebe primero con el script de simulación: si el script funciona pero App Builder no, es probable que no se cargue una variable env (compruebe las variables env que faltan en la salida `aio app deploy`)

### Los campos de pago divididos no aparecen en el cierre de compra

**Causa:** las rutas de inyección de LayoutProcessorPlugin no coinciden con el diseño de cierre de compra.

**Corrección:**
* Compruebe en la consola del explorador si hay errores de RequireJS cargando `Client_SplitPayment/js/view/payment/split-payment`
* Comprobación `bin/magento setup:static-content:deploy` completada correctamente
* Vaciar caché: `bin/magento cache:flush`
* Si la temática tiene una desprotección personalizada, es posible que se necesite ajustar la ruta `LayoutProcessorPlugin` para insertar el componente

### Crédito de tienda no aplicable / &quot;No se pudo procesar el pago&quot; en el pedido del lugar

**Causa:** Normalmente, uno de los complementos de casos extremos no funciona correctamente.

**Comprobación:**
1. ¿Devuelve `SplitPaymentSession` las cantidades correctas? Agregar registro de depuración temporal en `PlaceOrderPlugin`
2. ¿Se está ejecutando `FixSplitPaymentGrandTotalPlugin` y afecta el total de la cotización antes de llamar a `BalanceManagementInterface::apply()`? El marcador `beginBalanceApply()` debe suprimirlo.
3. ¿Está activado el módulo de saldo del cliente en Commerce?

### La acción de App Builder recibe el evento pero falta orderId

**Causa:** La lista de campos de `io_events.xml` no incluye `entity_id` o la forma de carga útil de evento cambió.

**Corrección:**
* Confirmar `io_events.xml` incluye `entity_id` en la lista de campos
* En la acción, registre `JSON.stringify(params)` temporalmente para ver la forma de carga útil completa
* Compruebe que la función `extractValue()` encuentre el nivel de anidamiento correcto

### Los pedidos no se muestran en el panel de demostración

**Causa:** los criterios de búsqueda de Commerce REST `orders` no devuelven pedidos o el campo `split_cash_status` no está en la respuesta REST.

**Corrección:**
* Confirme que `OrderRepositoryPlugin` está cargando correctamente los atributos de la extensión
* Probar directamente: `GET /rest/V1/orders?searchCriteria[pageSize]=5` y comprobar si `extension_attributes.split_cash_status` aparece en la respuesta
* Compruebe que `extension_attributes.xml` declara correctamente el atributo `split_cash_status` en `OrderInterface`


## Lista de comprobación de verificación

* [ ] `split_*` columnas visibles en la tabla `sales_order`
* [ Los extremos REST de ] devuelven 401 (no 404) cuando se les llama sin autenticación
* [ ] La interfaz de usuario de pago dividido se procesa al pagar cuando se selecciona Efectivo
* [ ] Los mensajes de validación funcionan (sobrepago, crédito insuficiente)
* [ ] La protección de umbral bloquea pedidos > $100
* [ ] El pedido realizado tiene `pending_payment` estado y comentarios de App Builder
* [ ] `simulate-split-payment.mjs list` muestra el orden de prueba con importes divididos
* [ ] `simulate-split-payment.mjs accept <id>` mueve el pedido a `processing` con factura y envío
* [ ] `simulate-split-payment.mjs decline <id>` cancela el pedido
* [ ] El panel de demostración enumera las solicitudes pendientes y acepta/rechaza el trabajo de la interfaz de usuario


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

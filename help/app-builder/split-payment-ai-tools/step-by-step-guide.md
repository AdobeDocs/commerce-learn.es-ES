---
title: Guía de implementación paso a paso de POC de pagos divididos
description: Aprenda a implementar el POC de pagos divididos en orden una vez completada la configuración, que abarca el módulo, los entornos, el orquestador, la verificación y la IU opcional.
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# Guía de implementación paso a paso de POC de pagos divididos

Esta página es su lista de comprobación de la implementación. Se supone que ya ha visto la [descripción general](./overview.md) y la [demostración completa](./full-demo.md), y que su sitio de Commerce y su proyecto de App Builder están listos. Las páginas del tutorial individuales contienen detalles completos para cada tema a continuación: utilice esta página para saber qué hacer y en qué orden.

## Antes de comenzar

Confirme todo lo siguiente antes de ejecutar cualquier petición de datos o comando. La página [requisitos previos y configuración](./prerequisites-and-setup.md) cubre cada elemento en detalle.

**Lado de Commerce**

* [ ] Adobe Commerce 2.4.5 o posterior
* [ ] eventos de E/S habilitados e implementados (solo Commerce Cloud; requiere `ENABLE_EVENTING: true` en `.magento.env.yaml`)
* [ ] **Pago contra reembolso** habilitado en el administrador con su título establecido exactamente en `Cash`
* [ ] Crédito de tienda habilitado en administración
* [ ] cliente de prueba tiene un crédito de tienda de al menos 50 dólares
* [ ]: integración de Commerce creada, activada y los cuatro valores de OAuth guardados de forma segura

**Lado de App Builder**

* [ ] El proyecto de App Builder existe en Adobe Developer Console
* [ Se agregó el servicio Adobe I/O Events ] al área de trabajo
* [ ] Commerce se conectó como proveedor de eventos
* [ ] `aio login` completado; área de trabajo correcta seleccionada con `aio app use`
* [ ] Node.js 18 o posterior instalado; `aio` CLI instalado (`npm install -g @adobe/aio-cli`)

Si falta algo anterior, deténgase aquí y complételo primero.

## Fase 1: Creación del módulo Commerce

**Lo que esto hace:** Genera el módulo PHP `Client_SplitPayment` que administra la interfaz de usuario de cierre de compra, la aplicación de crédito de almacenamiento, los extremos REST y la suscripción de evento de E/S. Este es el adaptador delgado de Commerce: todo el flujo de trabajo del operador permanece en App Builder.

### Paso 1.1: Personalización de la solicitud del proyecto

Abra la página [petición de datos de IA del módulo Commerce](./commerce-module-prompt.md) y tenga en cuenta lo siguiente antes de copiar la petición de datos:

* Reemplazar `Client` con su nombre de proveedor real en el mensaje
* Reemplazar `SplitPayment` si desea un nombre de módulo diferente
* Si el tema no es Luma, es posible que sea necesario ajustar la ruta de inyección de `LayoutProcessorPlugin` y las asignaciones de RequireJS
* Si el método de Pago contra reembolso usa un código distinto de `cashondelivery`, actualice `payment-method-helper.js`

### Paso 1.2: Ejecución de la solicitud

Copie la solicitud completa de **INICIO DEL MENSAJE** a **Fin del mensaje** y péguela en el cursor (con Claude) o directamente en Claude. Ejecútelo desde la raíz del proyecto de Commerce para que la API pueda crear archivos en `app/code/`.

### Paso 1.3: Activación e instalación del módulo

Ejecute estos comandos desde la raíz del proyecto de Commerce:

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### Paso 1.4: Verificar que el módulo esté instalado correctamente

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Debería ver cinco columnas: `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`, `split_sc_invoice_id` y `split_cash_invoice_id`.

A continuación, confirme en un explorador que los campos de pago dividido aparecen al cerrar la compra cuando un cliente que ha iniciado sesión con crédito de tienda selecciona **Efectivo** como método de pago.

## Fase 2: Configuración de variables de entorno

**Lo que esto hace:** Prepara los archivos de credenciales utilizados por App Builder Orchestrator, el script de simulación y (opcionalmente) la extensión de la interfaz de usuario de Experience Cloud. En cada archivo de `.env` se usan los mismos cuatro valores de OAuth de la integración con Commerce.

Consulte [variables de entorno reference](./env-reference.md) para obtener la lista completa de variables por componente.

### Paso 2.1: Creación del orquestador `.env`

En su directorio `split-payment-orchestrator/`:

```bash
cp .env.example .env
```

Rellene todos los valores:

| Variable | ¿Dónde se consigue? |
|---|---|
| `COMMERCE_BASE_URL` | La URL de la tienda: sin barra diagonal |
| `COMMERCE_CONSUMER_KEY` | Administración de Commerce > Sistema > Integraciones |
| `COMMERCE_CONSUMER_SECRET` | Igual: solo se muestra durante la activación |
| `COMMERCE_ACCESS_TOKEN` | Igual |
| `COMMERCE_ACCESS_TOKEN_SECRET` | Igual |
| `PAYMENT_THRESHOLD` | Debe coincidir con `split_payment/general/threshold` en Commerce (predeterminado: `100`) |
| `DEMO_UI_SECRET` | Opcional: si se establece, obligatorio como `?secret=` en la dirección URL del panel |

### Paso 2.2: Creación del script de simulación `.env`

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

Complete `COMMERCE_BASE_URL` y los cuatro valores de OAuth (las mismas credenciales que arriba).

## Fase 3: Creación de App Builder Orchestrator

**Lo que esto hace:** genera el proyecto de App Builder `split-payment-orchestrator`: el consumidor de eventos de E/S `payment-orchestrator`, las acciones web `payment-accept` y `payment-decline`, y la interfaz de usuario del operador `demo-dashboard`.

### Paso 3.1: Ejecución del indicador de Orchestrator

Abra la página [petición de datos de App Builder orchestrator AI](./orchestrator-prompt.md). Copie la solicitud completa y ejecútela desde el directorio `split-payment-orchestrator/`.

### Paso 3.2: Instalación de dependencias e implementación

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

Después de la implementación, `aio app deploy` imprime las direcciones URL de acción. Tenga en cuenta la dirección URL `demo-dashboard`: la necesita en la fase 4.

### Paso 3.3: Confirmar el registro del evento

En Adobe Developer Console, abra el espacio de trabajo del proyecto de App Builder. En **Eventos**, confirme que el registro de `Split payment — sales order place before` existe y está enlazado a la acción `payment-orchestrator`.

## Fase 4 — Prueba del flujo completo

Siga los pasos de verificación en orden. La [guía de prueba y verificación](./testing-and-verification.md) tiene todos los comandos curl, el uso de scripts de simulación y la referencia de solución de problemas para cada paso de abajo.

### Paso 4.1: Verificar que los puntos finales REST se pueden enrutar

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### Paso 4.2: Prueba de la IU de cierre de compra

1. Inicie sesión en la tienda como cliente de prueba (el que tiene crédito de tienda)
2. Añadir un producto al carro de compras con un total inferior a 100 $ después del envío y los impuestos
3. En el paso de pago, selecciona **Efectivo**
4. Confirme los campos de pago dividido que aparecen: saldo mostrado, efectivo rellenado previamente, crédito de tienda en 0 $
5. Reduzca el importe en efectivo y confirme correctamente las actualizaciones de la parte de crédito de la tienda

### Paso 4.3 — Prueba de la protección de umbral

Añada productos por un total de más de 100 $, continúe con el cierre de compra, seleccione Efectivo e intente realizar el pedido. Debe aparecer el error `"Payment could not be processed. Please try again or contact support."`.

### Etapa 4.4 — Ejecución de una orden de pago fraccionado de prueba

Cree un carro de compras por debajo de 100 $, introduzca una división de crédito en efectivo/tienda y realice el pedido. En Administración de Commerce, confirme lo siguiente:

* El estado del pedido es `pending_payment`
* Dos comentarios de App Builder están visibles en el pedido:
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

Si no aparece ningún comentario de App Builder, compruebe los registros con `aio app logs` antes de continuar.

### Paso 4.5: Prueba de aceptación mediante script de simulación

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

En Administración de Commerce, confirme lo siguiente:

* El estado del pedido cambió a `processing`
* Comentario de historial: `"Cash payment of $X.XX received."`
* Factura de caja visible en la pestaña Facturas
* Envío visible en la pestaña Envíos (si corresponde)

### Paso 4.6: Rechazo de prueba mediante script de simulación

Realice otro orden de prueba y, a continuación:

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

Confirmar el estado del pedido es `canceled` y `split_cash_status` es `declined`.

### Paso 4.7: Prueba del tablero de demostración

Abra la URL del panel desde el paso 3.2. Con un pedido pendiente en el sistema:

1. Confirme que el pedido aparece en la lista pendiente
2. Haga clic en **Aceptar** y confirme que el pedido se mueve a `processing` en Commerce
3. Realice un nuevo pedido, vuelva al panel y haga clic en **Rechazar**; confirme la cancelación

## Fase 5 (opcional): Añadir la extensión de la IU de Experience Cloud

Esta fase incrusta el tablero del operador en el shell de administración de Commerce en lugar de usar el `demo-dashboard` independiente. Omita esta fase si el tablero independiente satisface sus necesidades.

### Paso 5.1: Revisión de los requisitos previos

La extensión de la interfaz de usuario de Experience Cloud requiere credenciales de IMS además de los valores de integración de OAuth. Lea la página [Solicitud de inteligencia artificial aplicada a la interfaz de usuario de Experience Cloud](./experience-cloud-ui-prompt.md) antes de ejecutar la solicitud, prestando atención a `OAUTH_CLIENT_ID` y a las variables de IMS relacionadas.

### Paso 5.2: Ejecución del indicador de extensión de la IU

Copie la solicitud completa de **INICIO DEL MENSAJE** a **Fin del mensaje** y ejecútela desde el directorio `commerce-checkout-starter-kit/`.

### Paso 5.3: Implementación

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## Referencia de solución rápida de problemas

| Síntoma | Lo primero que hay que comprobar |
|---|---|
| Los campos de pago dividido no aparecen al cerrar la compra | Requerir errores de JS en la consola del explorador; ejecutar también `bin/magento cache:flush` |
| `"Payment could not be processed"` en el pedido | `PlaceOrderPlugin` o `BalanceManagementInterface` — agregar registro de depuración temporal en `PlaceOrderPlugin` |
| No hay comentarios de App Builder en el pedido | Ejecute `aio app logs` para ver si el evento se activó y qué acción se devolvió |
| `403` de Commerce REST en App Builder | Fastly IP inclusión en la lista de permitidos: pruebe primero con el script de simulación; si esto funciona, el problema es el bloqueo de IP de salida |
| `"The signature is invalid"` de Commerce | Confirmar que `COMMERCE_BASE_URL` no tiene barra diagonal; confirmar que la integración está activada |
| Los pedidos no aparecen en el panel de demostración | Compruebe que `extension_attributes.split_cash_status` aparece en una respuesta directa de `GET /rest/V1/orders` |

Para obtener información detallada sobre la solución de problemas, consulte la [guía de pruebas y verificación](./testing-and-verification.md#common-issues-and-fixes).

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

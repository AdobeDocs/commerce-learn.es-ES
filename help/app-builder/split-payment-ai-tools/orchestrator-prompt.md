---
title: 'POC de pagos divididos: petición de App Builder Orchestrator AI'
description: Aprenda a utilizar este mensaje para crear la aplicación de pago dividido de Orchestrator. Eventos de I/O, Payment-Orchestrator, acciones web, tablero de demostración e implementación de aplicación de aio.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# POC de pago dividido: petición de App Builder orchestrator AI

Utilice esta página para copiar el aviso completo que genera el proyecto **split-payment-orchestrator**: el consumidor de eventos de E/S de **payment-orchestrator**, **payment-accept** y **payment-declina** acciones web, el **tablero de demostración** y el cliente REST de Commerce.

## Cómo utilizar este indicador

Copie todo desde **INICIO DEL MENSAJE** hasta **Final del mensaje** en el cursor (con Claude) o directamente en Claude. Ejecute la solicitud desde el directorio `split-payment-orchestrator/` (la raíz del proyecto de App Builder).

## Antes de ejecutar

* Finalizar [POC de pago dividido: requisitos previos y configuración del entorno](./prerequisites-and-setup.md).
* Tenga preparados su POC de pagos divididos [de: variables de entorno &#x200B;](./env-reference.md) y el archivo `.env` en el proyecto.


## El indicador

**INICIO DEL MENSAJE**


Está generando una aplicación Adobe App Builder completa para organizar pagos divididos en Adobe Commerce. Esta aplicación recibe eventos de E/S de Commerce, procesa las decisiones de pago divididas y vuelve a llamar a Commerce a través de REST.

**Proyecto:** `split-payment-orchestrator`
**Tiempo de ejecución:** Node.js 18
**Dependencias de clave:** `@adobe/aio-sdk ^6.0.0`, `got ^11.8.6`, `oauth-1.0a ^2.2.6`

Genere todos los archivos enumerados a continuación. La aplicación debe funcionar con Adobe I/O Runtime (`aio app deploy`).


### Estructura de archivo para generar

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

Defina cuatro acciones en el paquete `split_payment_orchestrator`:

**`payment-orchestrator`**
* `web: "no"` (solo déclencheur de eventos de E/S; no se puede llamar directamente a través de HTTP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Entradas del entorno: `LOG_LEVEL`, `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET`, `PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` (acción web HTTP — invocable por panel o ERP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Mismas entradas de credenciales de Commerce (sin `PAYMENT_THRESHOLD`)

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Mismas entradas de credenciales de Commerce

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` El panel de ← es de acceso público (protegido solamente por `DEMO_UI_SECRET` si está configurado)
* Entradas: todas las credenciales de Commerce + `DEMO_UI_SECRET`, `DEMO_UI_BASE_URL`

**Registro de eventos** (en `events.registrations`):

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

Cliente REST de OAuth 1.0a compartido para Adobe Commerce. Implementa:

**`createCommerceClient(params, logger)`** — devuelve `{ request, baseUrl }`

* Lee `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET` de `params`
* Se inicia si falta alguna credencial
* Utiliza `oauth-1.0a` con `HMAC-SHA256` (Node.js `crypto.createHmac`)
* Usa `got@11` (no `got@12+` — el proyecto usa CJS) con `prefixUrl = ${baseUrl}/rest/V1/`
* Agrega el encabezado `Authorization` a través del vínculo `beforeRequest`
* `request(method, path, options)`: normaliza la ruta de acceso (elimina las líneas que preceden a `/`), devuelve `{ statusCode, body }`
* `throwHttpErrors: false`: nunca activa 4xx/5xx; siempre devuelve el código de estado


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — devuelve `{ pass: boolean, reason: string }`

Lógica:
1. Leer `params.PAYMENT_THRESHOLD`; analizar como flotante; predeterminado: `100` si falta, NaN o ≤ 0
2. Si `orderTotal > threshold`: devuelve `{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`
3. Si `Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`: devuelve `{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`
4. De lo contrario: devuelve `{ pass: true, reason: '' }`


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — devuelve `{ ok: boolean, error? }`

* Llamadas `POST orders/${orderId}/comments` con:

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* Devuelve `{ ok: true }` en 2xx; devuelve `{ ok: false, error: { code, message } }` en caso contrario
* Se ajusta en try/catch; devuelve un objeto de error, nunca lo arroja


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — devuelve `{ ok: boolean, error? }`

* Si `success`: publica un comentario de historial `"Split payment orchestration completed. Order awaiting cash confirmation."`
* Si `!success`: publica `"Payment could not be processed. Please try again or contact support."` — nunca incluir `detail` en el comentario visible para el cliente; registre `detail` solo internamente
* Devuelve `{ ok: boolean, error? }` — nunca lanza


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`**: implementación obsoleta sin operación

Incluya este archivo con un aviso de JSDoc `@deprecated` que explique:
> El orquestador ya no aplica crédito de almacén a través de REST. El crédito de tienda se aplica al cierre de compra en el módulo Commerce PHP (`PlaceOrderPlugin`) usando `BalanceManagementInterface::apply()`, que requiere un carro de compras activo. Para el momento en que App Builder reciba el evento de E/S, el carro de compras estará inactivo. Este archivo se conserva como referencia o para flujos personalizados en los que el crédito de tienda se aplica después del pedido.

Mantenga una implementación en funcionamiento (la misma forma que otros módulos) para que los desarrolladores puedan estudiar el patrón, pero márquelo claramente como no utilizado en el flujo actual.


### `actions/payment-orchestrator/index.js`

Punto de entrada de evento de E/S. Implementa `async function main(params)`.

**Extracción de carga útil:**

Los eventos de Adobe Commerce I/O pueden entregar la carga útil de varias formas. Extraiga el objeto de valor del pedido comprobando estas rutas en orden:
1. `params.__ow_body` (analizar cadena JSON si es necesario) → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. Volver a `params`

**Extracción de campo del valor:**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* Dividir importes de `value.extension_attributes` (comprobar tanto `extension_attributes` como `extensionAttributes`):
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**Flujo:**
1. Extraer valor; si falta `orderId`, registrar error y devolver `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`
2. Llamar a `evaluateThreshold(...)`: si falla, registrar y devolver las mismas 200 respuestas
3. Llamar a `createCommerceClient(params, logger)`: si falla, devuelve el error 200
4. Si `storeCredit > 0`, registra que el crédito de almacén se aplicó al finalizar la compra (no se necesita llamada REST)
5. Llamar a `recordCashPending(...)`: si falla, llame a `updateOrderAfterOrchestration(..., success: false)` y devuelva el error 200
6. Llamar a `updateOrderAfterOrchestration(..., success: true)`
7. Devolver `{ statusCode: 200, body: { ok: true, message: 'processed' } }`

**Importante:** Devolver siempre `statusCode: 200`: I/O Runtime reintentará las respuestas que no sean 200, lo que causaría el procesamiento de pedidos duplicados. Los errores se registran en el cuerpo del mensaje.

Constante **`PUBLIC_ERROR`:** `"Payment could not be processed. Please try again or contact support."` — se usa para todos los mensajes de error externos.


### `actions/payment-accept/index.js`

Acción web HTTP. Llamadas `POST /V1/split-payment/orders/:orderId/cash-received`.

**Resolución de ID de pedido:** Compruebe `params.orderId`, después `params.payload.orderId` y después `params.__ow_body` (JSON analizado). Devuelve 400 si falta.

**Flujo:**
1. Resolver `orderId`; devolver 400 si falta
2. Cliente de comercio de inicio; devolución 500 si falla
3. Llamar a `POST split-payment/orders/${orderId}/cash-received` con cuerpo JSON vacío
4. If 2xx: return `{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }`
5. Si error: registrar y devolver `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`


### `actions/payment-decline/index.js`

Acción web HTTP. El mismo patrón que `payment-accept` pero llama a `POST /V1/split-payment/orders/:orderId/cash-decline`.

Devolver `{ ok: true, orderId, message: 'declined' }` si se realizó correctamente.


### `actions/demo-dashboard/index.js`

Panel del operador de demostración independiente. Sirve como panel de control de HTML para listar pedidos de efectivo pendientes y activar acciones de aceptación/rechazo. Se trata de una acción web única que sirve tanto a la interfaz de usuario de HTML como a una API de JSON.

**Seguridad:**
* Comprobación `DEMO_UI_SECRET` opcional: si se establece, se requiere el parámetro de consulta `?secret=<value>` o el encabezado `x-demo-secret` en todas las solicitudes. Devuelve 401 si falta/es incorrecto.
* Registrar una advertencia si `DEMO_UI_SECRET` no está establecido (el panel no está protegido)

**Enrutamiento (basado en método HTTP + ruta/cuerpo):**

La acción recibe todas las solicitudes. Determinar la intención desde:
* `params.__ow_method` (GET/POST) y `params.__ow_path` o parámetros de acción
* `GET` sin acción → servir el panel de HTML
* `GET` con el parámetro `action=list` → devolver una lista JSON de pedidos pendientes
* `POST` con `action=accept` y `orderId` → la lógica `payment-accept` de la llamada (o en línea de la llamada REST de Commerce)
* `POST` con `action=decline` y `orderId` → la lógica `payment-decline`

**Recuperando pedidos pendientes:**
* Llamar a `GET orders?searchCriteria[pageSize]=50` desde Commerce REST
* Filtre a pedidos en los que `extension_attributes.split_cash_status === 'pending'` AND order no está en estado terminal (`complete`, `closed`, `canceled`, `cancelled`)
* Ordenar más reciente-primero en la memoria
* Devolver hasta 20 (configurables mediante el parámetro `limit`)

**Panel de HTML:**
El tablero es una cadena de HTML devuelta con `Content-Type: text/html`. Debería:
* Enumerar pedidos de efectivo pendientes en una tabla: Número de pedido (increment_id), entity_id, nombre del cliente, importe de efectivo, importe de crédito de almacén, fecha
* Proporcione los botones **Accept** y **Decline** para cada pedido que llame al punto de conexión de API de la acción
* Mostrar respuestas de éxito/error en línea
* Incluir un botón de actualización
* Tener el estilo suficiente para poder utilizarla (CSS mínimo está bien; no hay dependencias de CDN externas para evitar problemas de CORS en tiempo de ejecución)
* Mostrar un banner de advertencia si `DEMO_UI_SECRET` no está establecido

**Error al mostrar en la IU:**
Cuando falle una llamada de REST de Commerce, incluya el estado HTTP y una breve descripción del cuerpo del error de Commerce (sanear: eliminar las etiquetas de HTML, truncar a 500 caracteres). No exponga las credenciales.

**Ayudantes de respuesta:**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### Comando Deploy

Después de generar todos los archivos, en el directorio `split-payment-orchestrator/`:

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

Después de la implementación, anote las direcciones URL de acción impresas por `aio app deploy`. La dirección URL `demo-dashboard` es donde se accede al panel del operador.


### Final del mensaje


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

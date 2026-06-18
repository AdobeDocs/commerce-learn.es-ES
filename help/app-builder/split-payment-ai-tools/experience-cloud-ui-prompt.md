---
title: 'POC de pagos divididos: petición de API de la extensión de IU de Experience Cloud'
description: 'Aprenda a utilizar este mensaje opcional para incrustar el pago dividido en Commerce Admin: IU de administración de SDK, IMS, OAuth, aceptar y rechazar y el script de simulación.'
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# POC de pagos divididos: petición de API de la extensión de IU de Experience Cloud

Este es el paso opcional que incrusta un panel de órdenes de pago divididas en el shell de administración de **[!UICONTROL Adobe Commerce]** (Experience Cloud) mediante el patrón `commerce-checkout-starter-kit` y `commerce-backend-ui-1`. El [tablero de demostración](./orchestrator-prompt.md) independiente de App Builder Orchestrator cubre el mismo flujo de aceptación y rechazo sin integración con Admin Shell.

## Cómo utilizar este indicador

Copie todo desde **INICIO DEL MENSAJE** hasta **Final del mensaje** en el cursor o en Claude. Ejecute la solicitud desde el directorio `commerce-checkout-starter-kit/`.

## Antes de ejecutar

* Esta ruta necesita **credenciales de IMS** además de los valores de OAuth (consulte [POC de pago dividido: referencia de variables de entorno](./env-reference.md) para las variables `commerce-checkout-starter-kit`).
* Complete [Split payment POC: el aviso de IA de App Builder orchestrator](./orchestrator-prompt.md) primero si desea comparar el mismo comportamiento de `payment-accept` y `payment-decline`; la extensión de la interfaz de usuario vuelve a utilizar esa lógica con `COMMERCE_INTEGRATION_*` nombres env.


## El indicador

**INICIO DEL MENSAJE**


Está generando la extensión SDK de la IU de administración de `commerce-backend-ui-1` para el PoC de pago dividido. Esta extensión incrusta un panel de órdenes de pago divididas en Adobe Commerce Admin Shell utilizando los patrones `@adobe/aio-app-dev-toolkit` y `@adobe/commerce-backend-ui-1` del Starter Kit de cierre de compra de Commerce.

**Proyecto base:** `commerce-checkout-starter-kit/`
**Directorio de extensiones:** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### Qué proporciona esta extensión

1. **Panel de cuadrícula de pedidos de administración**: un elemento de menú personalizado en el Admin Shell de Commerce que enumera los pedidos de pago divididos con `split_cash_status = 'pending'`
2. **Vista de detalles del pedido**: muestra el desglose de pagos divididos (importe en efectivo, importe de crédito de tienda, estado) junto con el pedido
3. **Aceptar / Rechazar acciones** — botones que llaman a las acciones `payment-accept` y `payment-decline` de App Builder a través de OAuth 1.0a


### Estructura de archivo para generar

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### Acciones de servidor

**`actions/commerce/index.js`** — REST de Commerce autenticado por IMS
* Utiliza el token de IMS proporcionado por el contexto de SDK de la IU de administración para llamar a Commerce REST
* Obtiene la lista de pedidos con el filtro `split_cash_status`
* Devuelve la lista de pedidos como JSON

**`actions/payment-accept/commerce-client.js`** — Cliente de OAuth 1.0a
* Misma implementación que `split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`
* Utiliza variables env con prefijo `COMMERCE_INTEGRATION_*` (para distinguir las credenciales de IMS)

**`actions/payment-accept/index.js`** — Aceptar acción
* La misma lógica que `split-payment-orchestrator/actions/payment-accept/index.js`
* Llamadas `POST /V1/split-payment/orders/:orderId/cash-received` a través de OAuth 1.0a

**`actions/payment-decline/index.js`** — Rechazar acción
* Llamadas `POST /V1/split-payment/orders/:orderId/cash-decline`

**`actions/registration/index.js`** — Registro de SDK en la IU de administración
* Registra la extensión con el shell de administración de Commerce
* Agrega un elemento de menú en Pedidos para el panel de pagos divididos


### Componentes de front-end de React

**`SplitPaymentDashboard.jsx`**
* Enumera las órdenes de pago dividido pendientes en una tabla de tipo espectro
* Columnas: Número de pedido (increment_id), Fecha, Cliente, Vencimiento de efectivo, Crédito de tienda, Estado
* Botones Aceptar y Rechazar por fila
* Llamadas a acciones de back-end a través de `web-src/src/utils.js` ayudantes de recuperación
* Muestra estados de carga/error; se actualiza al finalizar la acción

**`SplitPaymentOrderDetail.jsx`**
* Muestra los detalles de pago dividido de un único pedido
* Muestra: importe de efectivo, importe de crédito de almacén, split_cash_status actual

**`useSplitPaymentOrders.js`** — Reaccionar gancho
* Recupera órdenes de pago divididas de `actions/commerce/index.js`
* Devuelve `{ orders, loading, error, refresh }`


### Script de simulación

**`scripts/simulate-split-payment.mjs`**

Un script ESM de Node.js para probar llamadas Commerce REST directamente (sin pasar por App Builder). Utiliza la misma firma OAuth 1.0a que las acciones de App Builder.

Comandos:
* `node simulate-split-payment.mjs help` — mostrar uso
* `node simulate-split-payment.mjs list` — enumerar pedidos recientes con datos de pago dividido
* `node simulate-split-payment.mjs show <orderId>` — mostrar campos de pago dividido para un pedido específico (entity_id)
* `node simulate-split-payment.mjs accept <orderId>` — extremo de llamada `cash-received`
* `node simulate-split-payment.mjs decline <orderId>` — extremo de llamada `cash-decline`

Lee credenciales de `commerce-backend-ui-1/.env.simulation` (copia de `.env.simulation.example`).

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

Configure la extensión con:
* El tipo de extensión `commerce-backend-ui-1`
* Las cuatro acciones de servidor (`commerce`, `payment-accept`, `payment-decline`, `registration`)
* `require-adobe-auth: true` para todas las acciones excepto `registration`
* Enlaces de entrada de env para credenciales de `COMMERCE_INTEGRATION_*`


### Después de generar archivos

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### Final del mensaje


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

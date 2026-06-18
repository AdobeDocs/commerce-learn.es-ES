---
title: 'POC de pagos divididos: referencia de variables de entorno'
description: Obtenga información sobre cómo asignar Commerce OAuth, la URL base, el umbral de pago y la configuración de demostración opcional a los archivos de entorno de orquestación, extensión de interfaz de usuario y simulación.
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# POC de pagos divididos: referencia de variables de entorno

En cada componente se utilizan las mismas cuatro credenciales de OAuth de Commerce. En **[!UICONTROL Commerce Admin]**, cree un **[!UICONTROL Integration]** y luego vuelva a usar los cuatro valores en cada archivo de `.env` a continuación. (Consulte [POC de pago dividido: requisitos previos y configuración del entorno](./prerequisites-and-setup.md) para ver los pasos de activación).

## Las cuatro credenciales de OAuth (utilizadas en todas partes)

| Variable | ¿Dónde se consigue? |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[su integración]* |
| `COMMERCE_CONSUMER_SECRET` | Igual que arriba: los valores solo se muestran durante la activación |
| `COMMERCE_ACCESS_TOKEN` | Igual que arriba |
| `COMMERCE_ACCESS_TOKEN_SECRET` | Igual que arriba |


## App Builder orchestrator

### `split-payment-orchestrator/.env`

Copie de `.env.example` en el directorio de orquestador. No confirme este archivo.

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Extensión de la interfaz de usuario de Experience Cloud (commerce-checkout-starter-kit)

### `commerce-checkout-starter-kit/.env`

Este componente utiliza dos conjuntos de credenciales: IMS para la lista de pedidos con la interfaz de usuario **[!UICONTROL Admin]** de SDK y OAuth 1.0a para las acciones de aceptar y rechazar.

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## Script de simulación

### `commerce-backend-ui-1/.env.simulation`

Copie de `.env.simulation.example` en el mismo directorio.

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## Notas

**`PAYMENT_THRESHOLD`** — Debe coincidir con `split_payment/general/threshold` en la configuración del sistema de **[!UICONTROL Commerce]**. Ambos lados tienen el valor predeterminado `100` si falta el valor, no es numérico o es menor o igual que `0`. Si cambia el umbral en **[!UICONTROL Commerce]**, actualice App Builder `.env` para que coincida.

**`DEMO_UI_SECRET`**: opcional pero recomendado para cualquier implementación que no sea localhost. Cualquier persona con la dirección URL del panel puede enumerar pedidos y ejecutar Aceptar y rechazar si está vacío. Para un entorno de ensayo real, establezca un secreto compartido.

**`COMMERCE_BASE_URL`** — Nunca incluir una barra diagonal. El cliente REST de Commerce anexa `/rest/V1/` automáticamente.

**`AIO_CLI_ENV`** — Se establece en `stage` para el área de trabajo **[!UICONTROL Stage]**. Cambie a `prod` al implementar en **[!UICONTROL Production]**.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

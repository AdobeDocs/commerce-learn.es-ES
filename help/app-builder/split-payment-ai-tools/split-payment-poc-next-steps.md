---
title: 'POC de pagos divididos: pasos siguientes después de la prueba de concepto'
description: 'Aprenda a mover el POC de pago dividido a la producción: IU de Experience Cloud, ganchos de ERP, malla de API, ámbito de PHP, flujos de trabajo de App Builder e implementación de una lista de comprobación.'
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# POC de pagos divididos: pasos siguientes después de la prueba de concepto

El tablero de demostración y el script de simulación que ha creado en este tutorial son intencionadamente aproximados. Existen para probar el patrón. En esta página se describe una ruta práctica que abarca desde la prueba de concepto hasta el desarrollo de App Builder de estilo de producción.


## Paso 1: Reemplazo del panel de demostración con una extensión de la interfaz de usuario de Experience Cloud

La acción web `demo-dashboard` proporciona HTML desde una cadena dentro de una función Node.js. Funciona, pero no es el patrón de producción.

El reemplazo adecuado es la extensión `commerce-backend-ui-1` en `commerce-checkout-starter-kit`, una aplicación de React incrustada en Commerce Admin Shell a través de Adobe Admin UI SDK. Ver [POC de pago dividido: petición de API de la extensión de la interfaz de usuario de Experience Cloud](split-payment-poc-experience-cloud-ui-prompt.md) para la petición de datos de generación.

**Qué cambios:**
* Los operadores inician sesión a través de Commerce Admin Shell (autenticación IMS en lugar de un secreto compartido)
* La lista de pedidos utiliza el contexto de token de IMS, no es necesario un secreto de demostración
* Las acciones de aceptar/rechazar tienen un ámbito de la identidad IMS del operador
* La IU está incrustada en Commerce Admin en la URL que los administradores de Commerce ya conocen

**Lo que permanece igual:**
* `payment-accept` y `payment-decline` acciones de App Builder no se han modificado
* Los extremos REST de Commerce no cambian
* El módulo PHP no cambia


## Paso 2: Conexión de un ERP real

El flujo de aceptación/rechazo de este PoC está impulsado por un ser humano que hace clic en un botón. En producción, esto es impulsado por su ERP, CRM o procesador de pagos.

**Patrón de integración:**
1. Su sistema ERP captura efectivo y llama a `POST /payment-accept` (la URL de acción web de App Builder) con `{ orderId: <entity_id> }`
2. App Builder valida la llamada (token de portador o clave de API: agregar middleware de autenticación a `payment-accept`)
3. App Builder llama a Commerce REST `cash-received`
4. Commerce factura y envía el pedido

No se requieren cambios de PHP. La superficie REST ya está allí. La acción de App Builder solo necesita un llamador seguro en lugar de un botón del panel.

**Opciones de autenticación para el llamador de ERP:**
* Adobe I/O Runtime admite `require-adobe-auth: true` para tokens de IMS (si su ERP puede obtener un token de IMS)
* Para sistemas que no son de Adobe: agregue una simple comprobación de clave de API en la acción `payment-accept` (compare un encabezado con un secreto almacenado en el entorno de la acción)


## Paso 3: Adición de la malla de API como capa de agente

Actualmente, App Builder llama a Commerce REST directamente con las credenciales de OAuth 1.0a. Para implementaciones más grandes, la malla de la API de Adobe proporciona una capa de puerta de enlace administrada entre App Builder y Commerce.

**Beneficios:**
* Administración centralizada de credenciales: la malla de API contiene las credenciales de Commerce; App Builder llama a la malla de API
* Solicitar transformación: asigne cargas útiles de App Builder a formas REST de Commerce sin cambiar la acción de App Builder
* Limitación de velocidad y almacenamiento en caché: proteja Commerce del tráfico de eventos de gran volumen

**Patrón:**
* Reemplazar llamadas de `createCommerceClient(params, logger)` con llamadas a su extremo de Mesh de API
* API Mesh gestiona la firma de OAuth en Commerce


## Paso 4: Reducir la huella de PHP

El módulo PHP actual maneja cinco cosas que deben permanecer en proceso (ver [POC de pagos divididos: decisiones de arquitectura y diseño](split-payment-poc-architecture-and-decisions.md)). A medida que la superficie de la API de Adobe Commerce madura, algunas de estas pueden volverse móviles:

**Posiblemente movible en el futuro:**
* La API de REST de crédito de tienda está evolucionando: las versiones futuras pueden admitir la aplicación de crédito después del pedido o a carros de compras inactivos
* A medida que más operaciones de Commerce se vuelven asíncronas seguras, el protector de umbral puede moverse a una resolución de malla de API de pedido previo

**No movible (restricciones arquitectónicas):**
* La manipulación de presupuestos antes de `placeOrder()` siempre requerirá código en proceso a menos que Commerce exponga un vínculo limpio a través de un punto de extensión con prioridad de API
* Los extremos REST (`/V1/split-payment/*`) son específicos de esta característica; se encuentran en Commerce porque llaman a servicios internos de Commerce


## Paso 5: Añadir más flujo de trabajo a App Builder

El PoC actual hace una cosa: escuchar la colocación del pedido y, a continuación, activar aceptar/rechazar. Extensiones naturales:

**Puntuación de fraude antes de aceptar:**
En `payment-orchestrator`, después de registrar el efectivo pendiente, llame a una API de puntuación de fraude antes de que el resultado de la orquestación se considere final. Si la puntuación está por encima de un umbral, se rechaza automáticamente en lugar de esperar la acción del operador.

**Correos electrónicos de notificación:**
Cuando `payment-accept` se realice correctamente, déclencheur un mensaje de correo electrónico (a través de Adobe Campaign, SendGrid o cualquier API HTTPS) notificando al cliente que el pago en efectivo se ha confirmado.

**Premios de puntos de fidelización:**
Una vez confirmado el efectivo, llame a una API de fidelidad para obtener puntos. Esto es App Builder puro — no se requiere PHP.

**Control de tiempo de espera:**
Agrega una acción programada de App Builder (que usa `cron` en `app.config.yaml`) que analiza los pedidos con `split_cash_status = 'pending'` más de X días y los rechaza automáticamente.


## Paso 6: Implementación en producción

El PoC está configurado para el espacio de trabajo `Stage`. Cambio a producción:

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**Lista de comprobación de preparación para la producción:**
* [ ] `DEMO_UI_SECRET` conjunto (o tablero de demostración reemplazado con la interfaz de usuario de Experience Cloud)
* [ ] `LOG_LEVEL=warn` o `error` en producción (no `debug`)
* [ ] `PAYMENT_THRESHOLD` coincide con la configuración de producción de Commerce
* [ ] Las credenciales de integración de Commerce en `.env` son para una integración de producción dedicada (no de ensayo)
* [ ]: la lista de permitidos de IP se actualizó rápidamente con las direcciones IP de salida de producción de App Builder (Commerce Cloud)
* [ ] registro de evento de E/S confirmado en el espacio de trabajo de producción


## Diagrama de evolución de arquitectura

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## Referencias clave

* [Documentación de Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Adobe I/O Events para Commerce](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Starter Kit de Commerce Checkout](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [IU de administración de Adobe SDK](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Mesh de API de Adobe](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime (OpenWhisk)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

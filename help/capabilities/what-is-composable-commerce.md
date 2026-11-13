---
title: Commerce Componible Con Adobe
description: Obtenga información sobre el comercio componible con las opciones de plataforma de Adobe, las tiendas Edge Delivery Services, la malla de API, App Builder y las herramientas MCP de desarrollo de Commerce.
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# Comercio componible

El comercio componible es un enfoque arquitectónico que combina las capacidades de comercio a través de las API. Puede seleccionar e integrar componentes para experiencias de tienda, administración de catálogos, descubrimiento de productos, cierre de compra y otras necesidades comerciales en lugar de requerir que todas las capacidades provengan de una aplicación estrechamente emparejada.

El comercio sin encabezado separa la capa de presentación de la tienda del backend comercial. El comercio componible se basa en esa separación al permitir que los equipos elijan qué capacidades mantener juntas y cuáles integrar de forma independiente. Una tienda sin encabezado no hace que cada componente back-end sea reemplazable de forma independiente.

Adobe admite este enfoque a través de las opciones de plataforma de comercio, la tecnología de tienda y las herramientas de integración y desarrollo. Los MCP de desarrollo de Adobe Commerce as a Cloud Service (ACCS), Adobe Commerce Optimizer (ACO), Edge Delivery Services, API Mesh, App Builder y Commerce tienen diferentes propósitos. No son productos intercambiables ni una pila combinada requerida.

## Ventajas y compensaciones

Un enfoque componible puede ayudarle a lo siguiente:

* **Adaptar la experiencia del cliente:** Elija componentes de tienda e integre servicios que cumplan con los requisitos de su empresa.
* **Modernizar de forma incremental:** Reemplazar las capacidades seleccionadas mientras se retienen otros sistemas y sus datos.
* **Escale y evolucione de forma independiente:** Diseñe integraciones y extensiones para que sus cambios no requieran la publicación de toda la aplicación de comercio.
* **Reutilice las capacidades de comercio:** Use las características de plataforma y las API compatibles en lugar de reconstruir cada función comercial.

Estos beneficios dependen de la implementación de. Evalúe la madurez técnica, los costes de envío, las licencias, la seguridad y la privacidad de los datos. Defina la propiedad para los contratos de API, la sincronización de datos, la autenticación, el rendimiento y los errores en todos los servicios. Una arquitectura más modular también puede aumentar la integración y la complejidad operativa.

## Opciones de arquitectura de Adobe Commerce

Comience por decidir si necesita una plataforma de comercio administrada o si desea modernizar las experiencias de los clientes en torno a un back-end transaccional existente.

Para ver un vídeo comparativo, consulte [Opciones de comercio disponibles de Adobe](./adobes-available-commerce-options.md).

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Service es una plataforma de comercio SaaS administrada. Adobe administra la aplicación, la infraestructura y las actualizaciones principales. Su arquitectura incluye Commerce Foundation, funciones de tienda sin encabezado, servicios de comercialización y herramientas para desarrolladores.

Utilice esta opción al evaluar una base de comercio administrado con puntos de extensión admitidos. Para la lógica empresarial personalizada, planifique la extensibilidad fuera de proceso admitida en lugar de suponer que las extensiones de PHP existentes se pueden implementar sin cambios. ACCS no admite tiendas Luma; incluya la migración de tiendas en su plan de implementación cuando corresponda.

Consulte la [descripción general de Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/es/docs/commerce/cloud-service/overview).

### Adobe Commerce Optimizer

Adobe Commerce Optimizer admite la administración de catálogos, la detección de productos, la búsqueda, las recomendaciones y las experiencias de tienda, a la vez que le permite conservar un servidor comercial existente. Los datos de catálogo pueden provenir de fuentes externas y un sistema de terceros puede seguir siendo responsable del ciclo de vida del carro de compras y del cierre de compra.

Utilice esta opción cuando el objetivo sea la modernización orientada al cliente y no sea necesario reemplazar el back-end transaccional. Planifique la ingesta del catálogo y las integraciones del carro de compras y cierre de compra explícitamente. ACCS y ACO son opciones de arquitectura independientes, no una pila combinada necesaria. Compruebe la compatibilidad con la integración actual antes de seleccionar los servicios para la implementación.

Consulte la [descripción general de Adobe Commerce Optimizer](https://experienceleague.adobe.com/es/docs/commerce/optimizer/overview).

### Implementaciones de Adobe Commerce existentes

Adobe Commerce en implementaciones locales y de infraestructura en la nube también puede adoptar de forma gradual tiendas sin encabezado e integraciones basadas en API compatibles. Según la implementación, puede conservar una tienda existente o adoptar una tienda sin encabezado compatible. Mantener unidas las capacidades de comercio existentes puede reducir el trabajo de integración, al tiempo que permite que los servicios seleccionados evolucionen por separado.

La disponibilidad de las funciones, la compatibilidad de las extensiones, las licencias y los requisitos de configuración varían según la oferta y la versión. Compare las [plataformas y servicios de Adobe Commerce](https://experienceleague.adobe.com/es/docs/commerce/information-center/explore-platforms-and-services/platform-and-services) antes de elegir una arquitectura.

## Tienda Commerce con tecnología de Edge Delivery Services

Commerce Storefront con tecnología Edge Delivery Services proporciona la experiencia de tienda y el nivel de entrega de contenido. Su arquitectura sin encabezado permite a los equipos desarrollar el front-end de forma independiente de la base comercial. La creación basada en documentos y la edición visual admiten actualizaciones de contenido, mientras que los componentes de tienda utilizan las API de comercio para las funciones de compra.

Distingue la interfaz de la tienda de los sistemas responsables de los datos del catálogo, los precios, las operaciones del carro de compras, el cierre de compra y los pedidos. Edge Delivery Services no reemplaza el backend del comercio transaccional. Identifique los servicios compatibles y la ruta de integración para el back-end elegido antes de implementar la tienda.

Consulte la [documentación de Commerce Storefront](https://experienceleague.adobe.com/en/tools/commerce-storefront/).

## Integración y extensibilidad

API Mesh y App Builder ayudan a conectar y ampliar las capacidades de comercio sin necesidad de que cada personalización esté incrustada en la aplicación principal de comercio.

### API Mesh

Mesh de API para Adobe Developer App Builder combina varias fuentes de API privadas, de terceros y de Adobe en un extremo de GraphQL consultable. Utilícelo para componer datos para experiencias de front-end y coordinar el acceso a los servicios existentes.

La composición de la API no elimina la necesidad de definir los contratos de API, la autenticación, la gestión de errores o la propiedad de la lógica empresarial transaccional. Mantenga esas responsabilidades explícitas en el diseño de la integración.

Consulte la [descripción general de API Mesh](https://developer.adobe.com/graphql-mesh-gateway/).

### Adobe App Builder

Adobe App Builder proporciona una plataforma para aplicaciones que amplían las soluciones de Adobe y se ejecutan en infraestructuras de Adobe, incluido Adobe I/O Runtime. Puede utilizarlo para personalizaciones e integraciones fuera de proceso con sistemas como CRM, ERP y PIM.

Elija los puntos de extensión, los eventos y los webhooks compatibles con la oferta de Commerce que utilice. Diseñe y pruebe la aplicación de forma independiente del ciclo de vida de la versión de comercio principal.

Consulte la [descripción general de App Builder](https://developer.adobe.com/app-builder/docs/intro_and_overview/).

## MCP y habilidades de desarrollo de Commerce

Los MCP y las habilidades de desarrollo de Commerce admiten el desarrollo asistido por IA de aplicaciones de App Builder y personalizaciones de tiendas. También pueden ayudar a migrar las extensiones de Adobe Commerce PHP existentes a aplicaciones de App Builder al pasar a ACCS.

Los servidores de Protocolo de contexto de modelo (MCP, Model Context Protocol ) conectan clientes de codificación de IA compatibles con herramientas de desarrollo. Las herramientas de Adobe incluyen Commerce Developer MCP Server, App Builder MCP Server y Dropins AI Tools, junto con habilidades específicas de Commerce.

Los MCP de desarrollo de Commerce son herramientas de desarrollo, no de envío de tienda, carro de compras, cierre de compra ni servicios de procesamiento de pedidos. No hacen que una aplicación sea compatible con todas las ofertas de Commerce automáticamente. Revise los cambios generados y pruébelos en el entorno de destino.

Consulte [MCP y habilidades de desarrollo de Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools).

## Planificar una implementación maquetable

1. Defina los objetivos empresariales, las experiencias del cliente, los requisitos de seguridad y las habilidades disponibles para su equipo.
2. Elija la plataforma de comercio e identifique qué sistemas existentes se van a conservar o reemplazar.
3. Seleccione una tienda compatible y defina los límites entre los servicios de presentación, comercialización y transaccionales.
4. Establezca contratos de propiedad de datos y API para información de catálogo, precios, inventario, clientes, carros de compras, cierres de compra y pedidos.
5. Planifique la autenticación, la sincronización de datos, el almacenamiento en caché, la monitorización y la administración de errores en todas las integraciones.
6. Implemente de forma incremental y valide la funcionalidad, el rendimiento y la seguridad antes de cada versión.

Una implementación componible tiene éxito cuando sus componentes cumplen los requisitos empresariales y trabajan juntos de forma fiable. Seleccione las funciones que necesita en lugar de añadir servicios únicamente para hacer que la arquitectura sea más modular.

## Recursos relacionados

Explore estos tutoriales, vídeos, guías de implementación y la lista de reproducción para planificar su arquitectura y crear integraciones de comercio.

### Plataformas y tiendas Commerce

* [Arquitectura de Adobe Commerce as a Cloud Service](../commerce-as-a-cloud-service/architecture.md)
* [Escaparates composibles y arquitectura modular](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Información general de Adobe Commerce Optimizer](../adobe-commerce-optimizer/overview.md)
* [Por qué existe el modelo de datos de catálogo maquetable](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [Experiencias del comprador con Edge Delivery Services](../edge-delivery-services/shopper-experience.md)

### Integración y desarrollo

* [Extensibilidad fuera de proceso con App Builder](../app-builder/foundations/introduction-to-app-builder.md)
* [Introducción a API Mesh](../api-mesh/getting-started-api-mesh.md)
* [Creación de una malla de API con varias fuentes](../api-mesh/graphql-multiple-source.md)
* [Genere tarifas de envío personalizadas con las herramientas AI y MCP](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### Documentación de arquitectura y desarrollo

* [Migrar a Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/es/docs/commerce/cloud-service/migration/overview)
* [Integraciones de Adobe Commerce Optimizer](https://experienceleague.adobe.com/es/docs/commerce/optimizer/integrations-overview)
* [Introducción al desarrollo de Tiendas Commerce](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Prácticas recomendadas para desarrollar MCP y habilidades de Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### Lista de reproducción

* [Introducción a API Mesh con Adobe App Builder y Adobe Commerce](https://experienceleague.adobe.com/es/playlists/commerce-get-started-app-builder-and-api-mesh)

---
title: Introducción a GraphQL
description: Descubra cómo utilizar GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Utilice llamadas de GET y POST de GraphQL para Adobe Commerce y [!DNL Magento Open Source].
short-description: Aprenda a utilizar las llamadas de GET y POST de GraphQL para Adobe Commerce y [!DNL Magento Open Source].
kt: 11524
doc-type: video
duration: 286
audience: all
last-substantial-update: 2023-10-12T00:00:00.000Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
subfeature_v2:
  - id: e396cff5-f586-484c-89f0-7f1da3308f92
    internal-label: GraphQL
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 6%
---
# Introducción a GraphQL

Esta es la primera parte de la serie para GraphQL y Adobe Commerce. GraphQL se ha convertido rápidamente en el estándar del sector por lo potentes que son las aplicaciones del lado del cliente con respecto al servidor. Es un tema cada vez más relevante para los desarrolladores de Adobe Commerce, ya que la plataforma sigue ampliando sus capacidades en el ámbito de las implementaciones sin encabezado.

Si es nuevo en GraphQL, esta sección le guía por los conceptos y el uso básicos.

>[!VIDEO](https://video.tv.adobe.com/v/3424117?learn=on)

## Vídeos y tutoriales relacionados sobre GraphQL en esta serie

* [Parte 2 GraphQL: Consultas](../graphql-rest/graphql-queries.md)
* [Parte 3 GraphQL - Mutaciones](../graphql-rest/graphql-mutations.md)
* [Parte 4 GraphQL - Esquema](../graphql-rest/graphql-schema.md)

## ¿Qué es GraphQL?

GraphQL es una especificación para un lenguaje de consulta de API único y el tiempo de ejecución que proporciona datos en respuesta a ese lenguaje de consulta.

Las API web tradicionales, como REST, han servido bien para sistemas dispares que pasan datos de un lado a otro, pero han proporcionado menos que el rendimiento máximo para experiencias modernas de vínculo de aplicación como aplicaciones web progresivas. En aplicaciones como esta, las capas del front-end y del back-end de la aplicación _same_ se comunican a través de la API web. El enfoque reglamentado de esquemas como REST a menudo no proporciona la flexibilidad adecuada en este contexto, donde muchos tipos de datos deben recuperarse rápidamente.

GraphQL permite que un cliente describa de forma expresiva _exactamente_ los datos que necesita. En lugar de requerir varias solicitudes de red para recuperar varios tipos de datos, una sola solicitud puede consultar varios tipos. Además, las respuestas se mantienen limpias al incluir (en un formato que refleja intuitivamente la consulta) solo los tipos y campos que se solicitan.

El motor en tiempo de ejecución que implementa la especificación de GraphQL se puede generar en cualquier idioma. Adobe Commerce y [!DNL Magento Open Source] utilizan
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} implementación PHP y construye sus propias capas sobre ella.

[Ver la documentación completa de GraphQL](https://graphql.org/learn){target="_blank"}

## Uso de un cliente de GraphQL

Necesita un cliente GUI de GraphQL para probar ejemplos de código y tutoriales. Hay varias opciones:

* [Altair](https://altairgraphql.dev/){target="_blank"} es un cliente excelente con todas las funciones, creado específicamente para GraphQL. Adobe utiliza Altair en vídeos explicativos.
* Si no desea instalar la aplicación de escritorio, también hay extensiones de Altair que se ejecutan directamente en su
  explorador [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox o [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"}.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} es una implementación del IDE de GraphQL desde GraphQL Foundation. No es una herramienta instalable, sino un paquete que puede utilizar para crear la interfaz usted mismo.
* Si ya estás familiarizado con [Postman](https://www.postman.com/){target="_blank"}, tiene un soporte decente para las consultas de GraphQL, aunque no está tan completo como un cliente de GraphQL dedicado.

En su cliente de GraphQL, debe enviar las solicitudes a la ruta de URL `/graphql` de su instancia de Adobe Commerce o [!DNL Magento Open Source]. Si prefiere usar una instancia existente para sus pruebas, puede usar la demostración del tema de Venia (la implementación de ejemplo de PWA Studio): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}

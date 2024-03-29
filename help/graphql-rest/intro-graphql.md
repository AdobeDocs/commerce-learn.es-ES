---
title: Introducción a GraphQL
description: Aprenda a utilizar GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Utilice llamadas de GET y POST de GraphQL para Adobe Commerce y [!DNL Magento Open Source].
short-description: Aprenda a utilizar las llamadas de GET y POST de GraphQL para Adobe Commerce y [!DNL Magento Open Source].
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b8b1e40a2f4d38954f0d21bc6f1a91b7ec0bd8c9
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

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

Las API web tradicionales, como REST, han servido bien para sistemas dispares que pasan datos de un lado a otro, pero han proporcionado menos que el rendimiento máximo para experiencias modernas de vínculo de aplicación como Progressive Web Application. En aplicaciones como esta, las capas front-end y back-end del _igual_ La aplicación se comunica mediante la API web. El enfoque reglamentado de esquemas como REST a menudo no proporciona la flexibilidad adecuada en este contexto, donde muchos tipos de datos deben recuperarse rápidamente.

GraphQL permite a un cliente describir de forma expresiva _exacto_ los datos que necesita. En lugar de requerir varias solicitudes de red para recuperar varios tipos de datos, una sola solicitud puede consultar varios tipos. Además, las respuestas se mantienen limpias al incluir (en un formato que refleja intuitivamente la consulta) solo los tipos y campos que se solicitan.

El motor en tiempo de ejecución que implementa la especificación de GraphQL se puede generar en cualquier idioma. ADOBE COMMERCE y [!DNL Magento Open Source] use el
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} Implementación de PHP y construye sus propias capas sobre ella.

[Ver la documentación completa de GraphQL](https://graphql.org/learn){target="_blank"}

## Uso de un cliente de GraphQL

Necesita un cliente GUI de GraphQL para probar ejemplos de código y tutoriales. Hay varias opciones:

* [Altair](https://altairgraphql.dev/){target="_blank"} es un cliente excelente y con todas las funciones, creado específicamente para GraphQL. El Adobe utiliza Altair en vídeos explicativos.
* Si no desea instalar la aplicación de escritorio, también hay extensiones de Altair que se ejecutan directamente en su
  [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} explorador.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} es una implementación del IDE de GraphQL desde GraphQL Foundation. No es una herramienta instalable, sino un paquete que puede utilizar para crear la interfaz usted mismo.
* Si ya está familiarizado con [Postman](https://www.postman.com/){target="_blank"}Sin embargo, tiene un soporte decente para las consultas de GraphQL, aunque no está tan completo como un cliente de GraphQL dedicado.

En el cliente de GraphQL, debe enviar las solicitudes a la ruta URL `/graphql` en su Adobe Commerce o [!DNL Magento Open Source] ejemplo. Si prefiere utilizar una instancia existente para sus pruebas, puede utilizar la demostración de la temática de Venia (la implementación de ejemplo de PWA Studio): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}

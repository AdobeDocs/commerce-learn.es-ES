---
title: Introducción a GraphQL
description: Obtenga información sobre cómo utilizar GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Uso de llamadas de GET y POST de GraphQL para Adobe Commerce y [!DNL Magento Open Source].
landing-page-description: Obtenga información sobre cómo utilizar GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Uso de llamadas de GET y POST de GraphQL para Adobe Commerce y [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: ef3dd7aaa409d9c1bc30d3d9c225966d8c1ace9e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Introducción a GraphQL

GraphQL se ha convertido rápidamente en el estándar del sector para saber cuán poderosas aplicaciones del lado del cliente hablan con un servidor. Es un tema cada vez más relevante para los desarrolladores de Adobe Commerce, ya que la plataforma continúa ampliando sus capacidades en el ámbito de las implementaciones sin objetivos.

Si es nuevo en GraphQL, esta sección le guiará por los conceptos básicos y el uso.

## ¿Qué es GraphQL?

GraphQL es una especificación para un lenguaje de consulta API único y el motor de ejecución que proporciona datos en respuesta a ese idioma de consulta.

Las API web tradicionales, como REST, han funcionado bien para sistemas dispares que pasan datos de un lado a otro, pero han proporcionado un rendimiento inferior al máximo para experiencias modernas de vínculos de aplicaciones, como los Progressive Web Application. En aplicaciones como esta, las capas front-end y back-end del _same_ comunicación de la aplicación a través de la API web. El enfoque reglamentado de esquemas como REST no suele proporcionar la flexibilidad adecuada en este contexto, donde es necesario recuperar rápidamente muchos tipos de datos.

GraphQL permite que un cliente describa de forma expresiva _Exactamente_ los datos que necesita. En lugar de requerir varias solicitudes de red para recuperar varios tipos de datos, una sola solicitud puede realizar consultas para varios tipos. Además, las respuestas se mantienen delgadas al incluir (en un formato que refleja la consulta de forma intuitiva) solo los tipos y campos solicitados.

El tiempo de ejecución que implementa la especificación GraphQL se puede crear en cualquier idioma. Adobe Commerce y [!DNL Magento Open Source] use el
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} Implementación de PHP y crea sus propias capas sobre ella.

[Ver la documentación completa de GraphQL](https://graphql.org/learn){target="_blank"}

## Uso de un cliente de GraphQL

Necesita un cliente de GUI GraphQL para probar ejemplos de código y tutoriales. Hay varias opciones:

* [Altair](https://altairgraphql.dev/){target="_blank"} es un cliente excelente y con todas las funciones creado específicamente para GraphQL. Adobe utiliza Altair en vídeos de introducción.
* Si no desea instalar la aplicación de escritorio, también hay extensiones de Altair que se ejecutan directamente en el
   [Chrome](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} explorador.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} es una implementación del IDE de GraphQL de GraphQL Foundation. Esta no es una herramienta instalable, sino un paquete que puede usar para crear la interfaz usted mismo.
* Si ya está familiarizado con [Postman](https://www.postman.com/){target="_blank"}, ofrece una compatibilidad adecuada con las consultas de GraphQL, aunque no se incluye tan completamente como un cliente de GraphQL dedicado.

En su cliente de GraphQL, debe enviar las solicitudes a la ruta de URL `/graphql` en su Adobe Commerce o [!DNL Magento Open Source] instancia. Si prefiere usar una instancia existente para las pruebas, puede utilizar la demostración del tema de Venia (la implementación de ejemplo de PWA Studio): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}

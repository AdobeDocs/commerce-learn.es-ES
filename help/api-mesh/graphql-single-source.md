---
title: Crear una red de origen única de GraphQL en red de API
description: Descubra cómo usar la red de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre la creación de una red que tenga un origen.
landing-page-description: Descubra cómo usar la red de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre la creación de una red que tenga un origen.
short-description: Discover how to use API Mesh on Adobe Commerce and [!DNL Adobe App Builder]. Learn about creating a mesh that has one source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Crear una red con un único origen

Este vídeo ayuda a los desarrolladores a comprender cómo crear una red con una única fuente en Mesh de API para Adobe Developer App Builder. Para que este ejemplo básico funcione según lo esperado, necesita una API o un punto final de GraphQL de acceso público. El vídeo también explica cómo crear una `mesh.json` para usar con la instancia de Commerce. Para obtener más información y ejemplos de código, visite [Crear una red](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## ¿Para quién es este vídeo?

* Cualquier persona nueva en la red de API
* Desarrolladores interesados en combinar varias fuentes de GraphQL y API
* Cualquier persona que necesite saber cómo filtrar la pestaña de red y filtrar por GraphQL.

## Contenido del vídeo

* Uso de la red de API como proxy inverso
* Creación de una red a partir de un archivo de configuración JSON
* Acceso al extremo de GraphQL recién creado

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Creación del archivo de configuración json

La red de API utiliza un archivo de configuración JSON para definir los controladores de origen. El archivo JSON contiene un `sources` matriz que contiene los orígenes de la red. A continuación se muestra un ejemplo de red con una única fuente.

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}

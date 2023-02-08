---
title: Crear una solicitud de fuente única de GraphQL para utilizarla en la red de API
description: Descubra cómo usar la red de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre la creación de una solicitud que tenga un origen.
landing-page-description: Descubra cómo usar la red de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre la creación de una solicitud que tenga un origen.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Crear una única red de la API de GraphQL de origen

El vídeo ayuda a los desarrolladores a comprender cómo se crea un proxy inverso de GraphQL y tiene un único origen. Recuerde, para que GraphQL Mesh funcione según lo esperado, se requiere una URL de acceso público con un esquema de GraphQL válido. El vídeo también explica cómo configurar el json inicial para utilizarlo con el sitio web de comercio. Para obtener ejemplos de código básicos utilizados en el vídeo, visite [Crear una red](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## ¿Para quién es este vídeo?

* Cualquiera que sea nuevo en la red de API
* Desarrolladores interesados en usar varias fuentes de graphql
* Cualquiera que necesite saber cómo filtrar la ficha de red y filtrar por graphql

## Contenido del vídeo

* Uso de API como proxy inverso
* Configuración JSON con la interfaz de línea de comandos de Adobe Developer
* Acceso al extremo de GraphQL recién creado

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Creación del archivo de configuración json

Para que Adobe App Builder conozca todas sus fuentes, las define en una configuración JSON. Cada origen es un elemento de una matriz y puede tener uno o más. A continuación se muestra un ejemplo de una única fuente

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

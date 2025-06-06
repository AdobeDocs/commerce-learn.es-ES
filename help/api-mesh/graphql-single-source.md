---
title: Creación de una malla de fuente única de GraphQL en API Mesh
description: Descubra cómo usar API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Aprenda a crear una malla que tenga una fuente.
landing-page-description: Descubra cómo usar API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Aprenda a crear una malla que tenga una fuente.
short-description: Descubra cómo usar API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Aprenda a crear una malla que tenga una fuente.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Creación de una malla con un solo origen

Este vídeo ayuda a los desarrolladores a comprender cómo crear una malla con una sola fuente en API Mesh para Adobe Developer App Builder. Para que este ejemplo básico funcione según lo esperado, necesita un punto final de GraphQL o API accesible públicamente. En el vídeo también se explica cómo crear un archivo `mesh.json` simple para utilizarlo con la instancia de Commerce. Para obtener más detalles y ejemplos de código, visite [Crear una malla](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## ¿Para quién es este vídeo?

* Cualquier persona nueva en API mesh
* Desarrolladores interesados en combinar varias fuentes de GraphQL y API
* Cualquiera que necesite saber cómo filtrar la pestaña de red y filtrar por GraphQL

## Contenido de vídeo

* Uso de API Mesh como proxy inverso
* Creación de una malla a partir de un archivo de configuración JSON
* Acceder al punto de conexión de GraphQL recién creado

>[!VIDEO](https://video.tv.adobe.com/v/3419718?quality=12&learn=on&captions=spa)

## Cree el archivo de configuración json.

API Mesh utiliza un archivo de configuración JSON para definir los controladores de origen. El archivo JSON contiene una matriz `sources` que contiene los orígenes de la malla. Este es un ejemplo de una malla con una sola fuente.

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

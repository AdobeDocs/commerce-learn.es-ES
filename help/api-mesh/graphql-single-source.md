---
title: Creación de una malla de fuente única de GraphQL en API Mesh
description: Aprenda a utilizar API Mesh en Adobe Commerce y Adobe App Builder. Descubra cómo crear una malla con una sola fuente de GraphQL y acceder al nuevo punto de conexión.
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Creación de una malla con un solo origen

Este vídeo ayuda a los desarrolladores a comprender cómo crear una malla con una sola fuente en API Mesh para Adobe Developer App Builder. Para que este ejemplo básico funcione, necesita un punto final de GraphQL o API accesible públicamente. En el vídeo también se explica cómo crear un archivo `mesh.json` simple para utilizarlo con la instancia de Commerce. Para obtener más detalles y ejemplos de código, visite [Crear una malla](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}.

## ¿Para quién es este vídeo?

* Cualquier persona nueva en API mesh
* Desarrolladores interesados en combinar varias fuentes de GraphQL y API
* Cualquiera que necesite saber cómo filtrar la pestaña de red y filtrar por GraphQL

## Contenido de vídeo

* Uso de API Mesh como proxy inverso
* Creación de una malla a partir de un archivo de configuración JSON
* Acceder al punto de conexión de GraphQL recién creado

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## Cree el archivo de configuración JSON.

API Mesh utiliza un archivo de configuración JSON para definir los controladores de origen. El archivo JSON contiene una matriz `sources` que contiene los orígenes de la malla. El siguiente es un ejemplo de una malla con una sola fuente.

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

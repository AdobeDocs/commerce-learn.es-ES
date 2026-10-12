---
title: Creación de una malla de fuente única de GraphQL en API Mesh
description: Aprenda a utilizar API Mesh en Adobe Commerce y Adobe App Builder. Descubra cómo crear una malla con una sola fuente de GraphQL y acceder al nuevo punto de conexión.
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
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

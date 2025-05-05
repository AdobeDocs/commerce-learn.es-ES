---
title: Crear un GraphQL de varias fuentes para utilizarlo en la malla de API
description: Descubra cómo usar varias fuentes para API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Obtenga información sobre algunos errores comunes y cómo resolverlos.
landing-page-description: Descubra cómo usar API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Obtenga información sobre la creación de una malla que tiene varias fuentes y cómo resolver algunos errores comunes.
short-description: Descubra cómo usar API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Obtenga información sobre la creación de una malla que tiene varias fuentes y cómo resolver algunos errores comunes.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Creación de una malla con varias fuentes

Este vídeo ayuda a los desarrolladores a comprender cómo crear una malla con varias fuentes en API Mesh para Adobe Developer App Builder. Este vídeo muestra cómo crear una malla con varias fuentes e identificar errores. Para obtener más detalles y ejemplos de código, visite [Crear una malla](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## ¿Para quién es este vídeo?

* Cualquiera que sea nuevo en API mesh
* Desarrolladores interesados en combinar varias fuentes de API y GraphQL.

## Contenido de vídeo

* Utilizar [transforms](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} para modificar el esquema de origen predeterminado
* Solucionar errores, como conflictos de nombres, disponibilidad de esquemas y otros problemas de sintaxis de esquemas
* Actualización de la malla con una configuración modificada

>[!VIDEO](https://video.tv.adobe.com/v/3419786?quality=12&learn=on&captions=spa)

## Cree el archivo de configuración json.

API Mesh utiliza un archivo de configuración JSON para definir los controladores de origen. El archivo JSON contiene una matriz `sources` que contiene los orígenes de la malla. Este es un ejemplo de una malla con varias fuentes.

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
      },
      {
        "name": "Example",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}

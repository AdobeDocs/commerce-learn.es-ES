---
title: Crear un GraphQL de origen múltiple para utilizarlo en la red de API
description: Descubra cómo usar varias fuentes para Mesh de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre algunos errores comunes y cómo resolverlos.
landing-page-description: Descubra cómo usar la red de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre la creación de una red que tenga varias fuentes y cómo resolver algunos errores comunes.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b3d5b22a597b342df6bf9846346d656dd4ce1383
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# Crear una red con varias fuentes

Este vídeo ayuda a los desarrolladores a comprender cómo crear una red con varias fuentes en Mesh de API para Adobe Developer App Builder. Este vídeo muestra cómo crear una red con varias fuentes e identificar errores. Para obtener más información y ejemplos de código, visite [Crear una red](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## ¿Para quién es este vídeo?

* Cualquiera que sea nuevo en la red de API
* Desarrolladores interesados en combinar varias fuentes de API y GraphQL

## Contenido del vídeo

* Cómo usar [transforma](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/) para modificar el esquema de origen predeterminado
* Solución de problemas de errores, como conflictos de nombres, disponibilidad del esquema y otros problemas de sintaxis del esquema
* Actualización de la red con una configuración modificada

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## Creación del archivo de configuración json

La red de API utiliza un archivo de configuración JSON para definir los controladores de origen. El archivo JSON contiene un `sources` matriz que contiene los orígenes de la red. Este es un ejemplo de red con múltiples fuentes.

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

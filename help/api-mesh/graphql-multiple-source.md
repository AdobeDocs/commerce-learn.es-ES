---
title: Crear un GraphQL de origen múltiple para utilizarlo en la red de API
description: Descubra cómo usar varias fuentes para Mesh de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre algunos errores comunes y cómo resolverlos.
landing-page-description: Descubra cómo usar la red de API en Adobe Commerce y [!DNL Adobe App Builder]. Obtenga información sobre la creación de una solicitud que tenga varias fuentes y cómo resolver algunos errores comunes.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Crear una red de API de GraphQL de origen múltiple

El vídeo ayuda a los desarrolladores a comprender cómo crear un proxy inverso de GraphQL con varias fuentes. Este vídeo muestra cómo unir diferentes fuentes, identificar errores y guardar cambios en git. Para obtener ejemplos de código básicos utilizados en el vídeo, visite [Crear una red](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## ¿Para quién es este vídeo?

* Cualquiera que sea nuevo en la red de API
* Desarrolladores interesados en usar varias fuentes de graphql
* Cualquiera que necesite saber cómo filtrar la ficha de red y filtrar por graphql

## Contenido del vídeo

* Cómo el esquema de API de atributos personalizados complejos de un segundo origen puede anular el esquema de origen predeterminado
* Modificación de la configuración de la red de la api para tener en cuenta el segundo esquema de anulación
* Solucionar los errores que podrían producirse en el proceso, como conflictos de nombres, disponibilidad de esquemas y otra sintaxis de SDL
* Ejemplo de errores comunes después de intentar unir esquemas
* Reconstrucción de la red de la api después de las ediciones
* Guardar cambios en git después de modificar la configuración de Mesh de API

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## Creación del archivo de configuración json

Para que Adobe App Builder conozca todas sus fuentes, las define en una configuración JSON. Cada origen es un elemento de una matriz y puede tener uno o más. Este es un ejemplo de una solicitud de origen múltiple que se mezcla para formar una sola respuesta.

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
        "name": "ERP",
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

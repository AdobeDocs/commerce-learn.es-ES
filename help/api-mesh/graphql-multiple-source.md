---
title: Crear un GraphQL de varias fuentes para utilizarlo en la malla de API
description: Descubra cómo usar varias fuentes para API Mesh en Adobe Commerce y  [!DNL Adobe App Builder]. Obtenga información sobre algunos errores comunes y cómo resolverlos.
jira: KT-21677
doc-type: Tutorial
duration: 381
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
TQID: https://experienceleague.adobe.com/O6ONn4NzMP-VqN0nsCoD-OPkZGMBelLWB-KNP1fZqmA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: 198
ht-degree: 0%

---

# Creación de una malla con varias fuentes

Este vídeo ayuda a los desarrolladores a comprender cómo crear una malla con varias fuentes en API Mesh para Adobe Developer App Builder. Este vídeo muestra cómo crear una malla con varias fuentes e identificar errores. Para obtener más detalles y ejemplos de código, visite [Crear una malla](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}.

## ¿Para quién es este vídeo?

* Cualquiera que sea nuevo en API mesh
* Desarrolladores interesados en combinar varias fuentes de API y GraphQL.

## Contenido de vídeo

* Cómo usar [transforms](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/transforms/){target="_blank"} para modificar el esquema de origen predeterminado
* Solucionar errores, como conflictos de nombres, disponibilidad de esquemas y otros problemas de sintaxis de esquemas
* Actualización de la malla con una configuración modificada

>[!VIDEO](https://video.tv.adobe.com/v/3414125?learn=on)

## Cree el archivo de configuración JSON.

API Mesh utiliza un archivo de configuración JSON para definir los controladores de origen. El archivo JSON contiene una matriz `sources` que contiene los orígenes de la malla. A continuación se muestra un ejemplo de una malla con varias fuentes.

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

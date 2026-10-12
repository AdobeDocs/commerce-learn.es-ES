---
title: Introducción a API Mesh
description: Aprenda a utilizar la API Mesh en Adobe Commerce y Adobe App Builder, incluida la instalación de App Builder, el trabajo con proyectos y la creación de un proxy inverso.
jira: KT-11802
doc-type: Tutorial
duration: 422
last-substantial-update: 2023-08-27T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
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
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%
---
# Introducción a API Mesh

Si es nuevo en API Mesh para Adobe Developer App Builder, Adobe recomienda empezar con este tutorial introductorio antes de pasar a los demás vídeos y tutoriales.

## ¿Qué es API Mesh?

API Mesh combina varias fuentes de datos para obtener una única respuesta para que la aplicación la consuma.

[Consulte la documentación completa de API Mesh](https://developer.adobe.com/graphql-mesh-gateway/mesh/){target="_blank"}

## ¿Para quién es este vídeo?

* Cualquier desarrollador nuevo en API Mesh o [!DNL Adobe Commerce] con experiencia limitada usando [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} y API Mesh.

## Contenido de vídeo

* Información general sobre API Mesh
* Vínculos a documentación complementaria
* Caso de uso para realizar una comprobación de inventario en tiempo real al cerrar la compra
* Alejar los esfuerzos de desarrollo y el uso de recursos de su aplicación de comercio

>[!VIDEO](https://video.tv.adobe.com/v/3421885?captions=spa&learn=on)

## Casos de uso de ejemplo

La aplicación de Commerce tiene una API de REST y un extremo de GraphQL. Por ejemplo, utilice la API de REST para aplicar precios especiales o el punto final de GraphQL para gestionar el estado de inventario. Con API Mesh, puede definir ambos extremos, recuperar la información y devolverla a la aplicación solicitante como una respuesta.

## Qué es un proxy inverso

Como desarrollador que utiliza Adobe App Builder y API Mesh, no es necesario comprender la definición de un proxy inverso. Sin embargo, si le interesa la funcionalidad general en lo que respecta a Adobe App Builder, utilice los siguientes recursos:

* [Qué es un proxy inverso](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}


{{$include /help/_includes/api-mesh-related-links.md}}

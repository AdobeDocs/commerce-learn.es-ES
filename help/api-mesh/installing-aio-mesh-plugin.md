---
title: Instalación de la interfaz de línea de comandos de Adobe Developer IO y del complemento de red de API
description: Descubra cómo instalar la interfaz de línea de comandos de Adobe Developer IO y el complemento de red de API
landing-page-description: Descubra cómo usar Adobe App Builder e instalar Adobe Developer IO con el complemento de red de API.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---


# Instalación del complemento Adobe Developer IO y Mesh

Antes de empezar, hay que configurar algunas cosas. En primer lugar, configure la interfaz de línea de comandos de Adobe Developer IO. A continuación, asegúrese de que el complemento de red de API esté configurado en cada entorno.
Para obtener instrucciones sobre la configuración del entorno local para ejecutar Node, nvm e instalar Adobe Developer IO, asegúrese de visitar [Introducción a GraphQL Mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe App Builder o [!DNL Magento Open Source] con experiencia limitada con Adobe Developer IO y Mesh de API.

## Contenido del vídeo

* Introducción a la red de API
* Instalación de la interfaz de línea de comandos de Adobe Developer IO
* Añadir el complemento de red de API a la línea de comandos de AIO

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## Comandos de ejemplo que utilizan NPM y AIO

La instalación de la interfaz de línea de comandos de Adobe Developer es bastante sencilla. Después de tener instalado Nodo, ejecute este comando `npm install -g @adobe/aio-cli`
Una vez instalado el cli de Adobe Developer, es posible instalar el complemento de red. Para ello, ejecute este comando `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}

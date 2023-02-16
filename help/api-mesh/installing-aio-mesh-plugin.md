---
title: Instalación de la interfaz de línea de comandos de Adobe I/O Runtime y del complemento de red de API
description: Descubra cómo instalar la interfaz de línea de comandos de Adobe I/O Runtime y el complemento de red de API
landing-page-description: Descubra cómo usar Adobe App Builder e instalar Adobe I/O Runtime con el complemento de red de API.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: a6fb3810f34246df73ae5557240eaaa0f4407eb1
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Instalación de Adobe I/O Runtime CLI y del complemento de red

Antes de empezar a usar la red de API para Adobe Developer App Builder, debe instalar la variable `aio` CLI y el complemento de red de API.
Para obtener instrucciones y requisitos previos de instalación, visite la red de API [Introducción](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/) página.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en API Mesh o [!DNL Adobe Commerce] con experiencia limitada mediante [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/) y Mesh de API.

## Contenido del vídeo

* Introducción a la red de API
* Instalación de la CLI de Adobe I/O Runtime (interfaz de línea de comandos)
* Instalación del complemento de red de API

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## La instalación del `aio` CLI y complemento de red de API

Después de instalar `node` y `npm`, ejecute el siguiente comando para instalar el `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Una vez instalada la CLI de Adobe I/O Runtime, utilice el siguiente comando para instalar el complemento de red de API:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

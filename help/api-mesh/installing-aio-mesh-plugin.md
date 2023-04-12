---
title: Instalación de la interfaz de línea de comandos de Adobe I/O Runtime y del complemento de red de API
description: Descubra cómo instalar la interfaz de línea de comandos de Adobe I/O Runtime y el complemento de red de API
landing-page-description: Descubra cómo usar Adobe App Builder e instalar Adobe I/O Runtime con el complemento de red de API.
short-description: Descubra cómo usar Adobe App Builder e instalar Adobe I/O Runtime con el complemento de red de API.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Instalación de Adobe I/O Runtime CLI y del complemento de red

Antes de empezar a usar la red de API para Adobe Developer App Builder, debe instalar la variable `aio` CLI y el complemento de red de API.
Para obtener instrucciones y requisitos previos de instalación, visite la red de API [Introducción](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} página.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en API Mesh o [!DNL Adobe Commerce] con experiencia limitada mediante [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} y Mesh de API.

## Contenido del vídeo

* Introducción a la red de API
* Instalación de la CLI de Adobe I/O Runtime (interfaz de línea de comandos)
* Instalación del complemento de red de API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

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

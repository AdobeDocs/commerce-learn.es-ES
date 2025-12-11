---
title: Instalación de la interfaz de línea de comandos de Adobe I/O Runtime y del complemento API Mesh
description: Descubra cómo instalar la interfaz de línea de comandos de Adobe I/O Runtime y el complemento API Mesh
landing-page-description: Descubra cómo utilizar Adobe App Builder e instalar el complemento Adobe I/O Runtime con API Mesh.
short-description: Descubra cómo utilizar Adobe App Builder e instalar el complemento Adobe I/O Runtime con API Mesh.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Instalación del complemento CLI de Adobe I/O Runtime y Mesh

Antes de empezar a usar API Mesh para Adobe Developer App Builder, debe instalar `aio` CLI y el complemento API Mesh.
Para obtener instrucciones de instalación y requisitos previos, visite la página de API Mesh [Introducción](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en API Mesh o [!DNL Adobe Commerce] con experiencia limitada usando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} y API Mesh.

## Contenido de vídeo

* Introducción a API Mesh
* Instalación de Adobe I/O Runtime CLI (interfaz de línea de comandos)
* Instalación del complemento API Mesh

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## Instalando el complemento `aio` CLI y API Mesh

Después de instalar `node` y `npm`, ejecute el siguiente comando para instalar la CLI `aio`:

```bash
npm install -g @adobe/aio-cli
```

Una vez instalada la CLI de Adobe I/O Runtime, utilice el siguiente comando para instalar el complemento Mesh de la API:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

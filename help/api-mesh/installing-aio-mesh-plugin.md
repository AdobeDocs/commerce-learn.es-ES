---
title: Instalación del complemento CLI de Adobe I/O Runtime y API Mesh
description: Aprenda a instalar la interfaz de línea de comandos de Adobe I/O Runtime y el complemento Mesh de la API para empezar a utilizar Mesh de la API para Adobe Developer App Builder.
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Instalación del complemento CLI de Adobe I/O Runtime y Mesh

Antes de empezar a usar API Mesh para Adobe Developer App Builder, debe instalar `aio` CLI y el complemento API Mesh.
Para obtener instrucciones de instalación y requisitos previos, visite la página de Mesh de API [Introducción](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en API Mesh o [!DNL Adobe Commerce] con experiencia limitada usando [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} y API Mesh.

## Contenido de vídeo

* Introducción a API Mesh
* Instalación de Adobe I/O Runtime CLI (interfaz de línea de comandos)
* Instalación del complemento API Mesh

>[!VIDEO](https://video.tv.adobe.com/v/3419792?captions=spa&learn=on)

## Instalando el complemento `aio` CLI y API Mesh

Para instalar la CLI `aio`, ejecute el siguiente comando después de instalar `node` y `npm`:

```bash
npm install -g @adobe/aio-cli
```

Una vez instalada la CLI de Adobe I/O Runtime, utilice el siguiente comando para instalar el complemento Mesh de la API:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}

---
title: Configuración de Adobe Commerce
description: Obtenga información sobre cómo configurar Adobe Commerce para que permita el uso de eventos en el Generador de aplicaciones de Adobe Developer.
landing-page-description: Aprenda a configurar Adobe Commerce para que utilice el mecanismo de eventos para que lo consuma App Builder de Adobe Developer.
short-description: Aprenda a configurar Adobe Commerce para que utilice el mecanismo de eventos para que lo consuma App Builder de Adobe Developer.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Architect, Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Configuración de Adobe Commerce

Obtenga información sobre cómo configurar Adobe Commerce para que exponga los eventos. Encontrará documentación adicional en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en el Generador de aplicaciones de Adobe Commerce y Adobe Developer que utilizan eventos de E/S y que necesitan crear un proyecto de Adobe del Generador de aplicaciones.

## Contenido de vídeo {#video-content}

* Configuración de los eventos de Adobe I/O en el administrador de Commerce
* Guardar una clave privada en el administrador de Commerce
* Guardar el identificador único en el administrador de Commerce
* Creación de un proveedor de eventos

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## Comandos útiles {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

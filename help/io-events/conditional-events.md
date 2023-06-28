---
title: Aprenda a utilizar eventos condicionales en Adobe Commerce
description: Aprenda a utilizar eventos condicionales para utilizarlos en el Generador de aplicaciones de Adobe Developer.
landing-page-description: Aprenda a utilizar los eventos condicionales de Adobe Commerce.
short-description: Aprenda a utilizar los eventos condicionales de Adobe Commerce.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Eventos condicionales de Adobe Commerce

Obtenga información sobre los eventos condicionales en Adobe Commerce que se pueden utilizar en el Generador de aplicaciones de Adobe Developer. Encontrará documentación adicional en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en el Generador de aplicaciones de Adobe Commerce y Adobe Developer que utilizan eventos de E/S y que necesitan crear un proyecto de Adobe del Generador de aplicaciones.

## Contenido de vídeo {#video-content}

* Más información sobre los eventos condicionales
* Aprenda el uso adecuado del nuevo archivo XML io_events.xml
* Obtenga información sobre cómo configurar eventos condicionales
* Definición de reglas para su uso en eventos condicionales
* Obtenga información sobre cómo registrar eventos en las instancias de Commerce `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806?quality=12&learn=on)

## Comandos útiles {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}

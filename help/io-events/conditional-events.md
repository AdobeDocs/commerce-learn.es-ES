---
title: Aprenda a utilizar eventos condicionales en Adobe Commerce
description: Aprenda a utilizar eventos condicionales para utilizarlos en Adobe Developer App Builder.
landing-page-description: Aprenda a utilizar los eventos condicionales de Adobe Commerce.
short-description: Aprenda a utilizar los eventos condicionales de Adobe Commerce.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Eventos condicionales de Adobe Commerce

Obtenga información acerca de los eventos condicionales en Adobe Commerce que se pueden utilizar en Adobe Developer App Builder. Encontrará documentación adicional en [Instalar Adobe I/O Events para Adobe Commerce](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder que utilizan eventos de E/S y que necesitan crear un proyecto de App Builder de Adobe.

## Contenido de vídeo {#video-content}

* Más información sobre los eventos condicionales
* Aprenda el uso adecuado del nuevo archivo XML io_events.xml
* Obtenga información sobre cómo configurar eventos condicionales
* Definición de reglas para su uso en eventos condicionales
* Obtenga información sobre cómo registrar eventos en las instancias de Commerce `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3419798?captions=spa&quality=12&learn=on)

## Comandos útiles {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}

---
title: Aprenda a utilizar eventos condicionales en Adobe Commerce
description: Aprenda a utilizar eventos condicionales para utilizarlos en Adobe Developer App Builder.
landing-page-description: Aprenda a utilizar eventos condicionales de Adobe Commerce.
short-description: Learn how to use Adobe Commerce conditional events.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---


# Eventos condicionales de Adobe Commerce

Obtenga información sobre los eventos condicionales en Adobe Commerce que se pueden usar en Adobe Developer App Builder. Documentación adicional encontrada en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## ¿Para quién es este vídeo?

* Los desarrolladores que utilicen nuevos eventos de E/S para Adobe Commerce y Adobe Developer App Builder deberán crear un proyecto de Adobe App Builder .

## Contenido del vídeo {#video-content}

* Descubra más información sobre los eventos condicionales
* Aprenda a utilizar correctamente el nuevo archivo XML io_events.xml
* Obtenga información sobre cómo configurar eventos condicionales
* Definición de reglas para su uso en eventos condicionales
* Obtenga información sobre cómo registrar eventos en las instancias de comercio `app/etc/config.php`

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

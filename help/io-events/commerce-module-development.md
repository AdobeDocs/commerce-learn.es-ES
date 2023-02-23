---
title: Aprenda a crear un módulo en Adobe Commerce para utilizar eventos.
description: Obtenga información sobre cómo crear un módulo de comercio para utilizar eventos.
landing-page-description: Obtenga información sobre cómo crear un módulo de Adobe Commerce para utilizar eventos.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: f1295c93652a9a2ae40a82009a8eb6895f59b909
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---


# Desarrollo de módulos Adobe Commerce

Obtenga información sobre cómo registrar eventos, buscar eventos admitidos y cómo utilizar un nuevo archivo XML `io_events.xml` en el desarrollo de módulos personalizados. El vídeo también mostrará a los desarrolladores cómo encontrar eventos registrados que se puedan usar, así como cancelar la suscripción de cualquier evento que ya se haya definido. Documentación adicional encontrada en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder mediante eventos de E/S.

## Contenido del vídeo {#video-content}

* Registro de eventos en Commerce para su uso en Adobe Developer App Builder
* Identificar los eventos que se pueden registrar
* Obtenga información sobre cómo registrar eventos en io_events.xml
* Obtenga información sobre cómo registrar eventos en las instancias de comercio `app/etc/config.php`
* Obtenga información sobre cómo cancelar la suscripción a un evento

>[!VIDEO](https://video.tv.adobe.com/v/3415802)

## Comandos útiles {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
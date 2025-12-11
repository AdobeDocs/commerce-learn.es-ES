---
title: Obtenga información sobre cómo crear un módulo en Adobe Commerce para utilizar eventos.
description: Obtenga información sobre cómo crear un módulo de Commerce para utilizar eventos.
landing-page-description: Obtenga información sobre cómo crear un módulo de Adobe Commerce para utilizar eventos.
short-description: Obtenga información sobre cómo crear un módulo de Adobe Commerce para utilizar eventos.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Desarrollo del módulo Adobe Commerce

Obtenga información sobre cómo registrar eventos, buscar eventos admitidos y cómo utilizar un nuevo archivo XML `io_events.xml` en el desarrollo de módulos personalizados. El vídeo también mostrará a los desarrolladores cómo encontrar eventos registrados que se pueden utilizar, así como cancelar la suscripción de cualquier evento que ya esté definido. Encontrará documentación adicional en [Instalar Adobe I/O Events para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder que utilizan eventos de I/O.

## Contenido de vídeo {#video-content}

* Registro de eventos en Commerce para su uso en Adobe Developer App Builder
* Identificar eventos que se pueden registrar
* Obtenga información sobre cómo registrar eventos en io_events.xml
* Obtenga información sobre cómo registrar eventos en las instancias de Commerce `app/etc/config.php`
* Obtenga información sobre cómo cancelar la suscripción a un evento

>[!VIDEO](https://video.tv.adobe.com/v/3419834?captions=spa&quality=12&learn=on)

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

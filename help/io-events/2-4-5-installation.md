---
title: Obtenga información sobre cómo instalar eventos IO para Adobe Commerce 2.4.5
description: Obtenga información sobre cómo instalar los módulos necesarios para los eventos de E/S en Adobe Commerce 2.4.5 para su uso en Adobe Developer App Builder
landing-page-description: Obtenga información sobre cómo instalar varios módulos necesarios para Adobe Commerce 2.4.5 mediante el compositor.
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.5 using composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.5"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---


# Instalación de Adobe Commerce 2.4.5

Obtenga información sobre cómo instalar varios módulos nuevos en Adobe Commerce mediante Composer para la versión 2.4.5. Esto configura los módulos necesarios para utilizarlos en la aplicación Adobe Commerce. Documentación adicional encontrada en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder mediante eventos de E/S

## Contenido del vídeo {#video-content}

* Instalación de los módulos necesarios mediante el compositor
* Comandos para ejecutar el alojamiento local
* Comandos que se ejecutan para Adobe Commerce Cloud
* La edición requerida para Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## Comandos útiles {#useful-commands}

Hay varios comandos que difieren ligeramente, dependiendo de si se encuentra en un entorno autoalojado o si utiliza Adobe Commerce Cloud.

### Alojamiento local {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce en la nube {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

---
title: Obtenga información sobre cómo instalar eventos IO para Adobe Commerce 2.4.6
description: Obtenga información sobre cómo instalar los módulos necesarios para los eventos de E/S en Adobe Commerce 2.4.6 para su uso en Adobe Developer App Builder
landing-page-description: Obtenga información sobre cómo instalar varios módulos necesarios para Adobe Commerce 2.4.6.
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---


# Instalación de Adobe Commerce 2.4.6

Obtenga información sobre cómo instalar varios módulos nuevos en Adobe Commerce con Composer para la versión 2.4.6. Encontrará documentación adicional en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder mediante eventos de E/S.

## Contenido del vídeo {#video-content}

* Comandos para ejecutar el alojamiento local
* Comandos que se ejecutan para Adobe Commerce Cloud
* La edición requerida para Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415795)

## Comandos útiles {#useful-commands}

Hay varios comandos que difieren ligeramente, dependiendo de si se encuentra en un entorno autoalojado o si utiliza Adobe Commerce Cloud.

### Alojamiento local {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce en la nube {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

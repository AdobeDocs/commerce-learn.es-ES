---
title: Obtenga información sobre cómo instalar eventos de IO para Adobe Commerce 2.4.5
description: Obtenga información sobre cómo instalar los módulos necesarios para eventos de E/S en Adobe Commerce 2.4.5 para utilizarlos en Adobe Developer App Builder
landing-page-description: Aprenda a instalar varios módulos necesarios para Adobe Commerce 2.4.5 con Composer.
short-description: Aprenda a instalar varios módulos necesarios para Adobe Commerce 2.4.5 con Composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Instalación de Adobe Commerce 2.4.5

Obtenga información sobre cómo instalar varios módulos nuevos en Adobe Commerce mediante Composer para la versión 2.4.5. Esto configura los módulos necesarios para utilizarlos en la aplicación de Adobe Commerce. Encontrará documentación adicional en [Instalar eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder que utilizan eventos de I/O

## Contenido de vídeo {#video-content}

* Instalación de los módulos necesarios utilizando Composer
* Comandos para ejecutar en el alojamiento local
* Comandos que se ejecutarán para Adobe Commerce Cloud
* Adobe Commerce Cloud yaml edición requerida

>[!VIDEO](https://video.tv.adobe.com/v/3419826?quality=12&learn=on&captions=spa)

## Comandos útiles {#useful-commands}

Existen varios comandos que difieren ligeramente, según si se encuentra en un entorno autoalojado o utiliza Adobe Commerce Cloud.

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

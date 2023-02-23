---
title: Configuración de Adobe Commerce
description: Obtenga información sobre cómo configurar Adobe Commerce para permitir que se utilicen eventos en Adobe Developer App Builder.
landing-page-description: Obtenga información sobre cómo configurar Adobe Commerce para que use el mecanismo de eventos para el consumo mediante Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: fe59ed078ac0fa410b9f0a7a62719a279f73390c
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# Configuración de Adobe Commerce

Obtenga información sobre cómo configurar Adobe Commerce para que muestre los eventos. Documentación adicional encontrada en [Instalación de eventos de Adobe I/O para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## ¿Para quién es este vídeo?

* Los desarrolladores que utilicen nuevos eventos de E/S para Adobe Commerce y Adobe Developer App Builder deberán crear un proyecto de Adobe App Builder .

## Contenido del vídeo {#video-content}

* Configuración de los eventos de Adobe I/O en el administrador de comercio
* Guardar una clave privada en el administrador de comercio
* Guardar el identificador único en el administrador de comercio
* Creación de un proveedor de eventos

>[!VIDEO](https://video.tv.adobe.com/v/3415799)

## Comandos útiles {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

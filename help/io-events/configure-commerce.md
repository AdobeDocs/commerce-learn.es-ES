---
title: Configuración de Adobe Commerce
description: Obtenga información sobre cómo configurar Adobe Commerce para que permita el uso de eventos en Adobe Developer App Builder.
landing-page-description: Obtenga información sobre cómo configurar Adobe Commerce para que utilice el mecanismo de eventos para que Adobe Developer App Builder lo consuma.
short-description: Obtenga información sobre cómo configurar Adobe Commerce para que utilice el mecanismo de eventos para que Adobe Developer App Builder lo consuma.
kt: 11889
doc-type: tutorial
duration: 299
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 151
ht-degree: 0%

---

# Configure Adobe Commerce

Learn how to configure Adobe Commerce to expose the events. Encontrará documentación adicional en [Instalar Adobe I/O Events para Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Who is this video for?

* Desarrolladores nuevos en Adobe Commerce y Adobe Developer App Builder que utilizan eventos de E/S y que necesitan crear un proyecto de App Builder de Adobe.

## Contenido de vídeo {#video-content}

* Configuración de los eventos de Adobe I/O en el administrador de Commerce
* Guardar una clave privada en el administrador de Commerce
* Saving the unique identifier in the Commerce admin
* Creación de un proveedor de eventos

>[!VIDEO](https://video.tv.adobe.com/v/3415799?learn=on)

## Comandos útiles {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

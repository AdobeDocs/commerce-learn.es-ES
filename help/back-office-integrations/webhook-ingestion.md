---
title: Configuración, implementación y personalización de un webhook de ingesta
description: Aprenda a configurar y personalizar un webhook de ingesta para facilitar la comunicación entre Commerce y un sistema de back office de terceros.
landing-page-description: Aprenda a utilizar el Starter Kit de integración de Commerce para integrar Commerce con un sistema de back office de terceros mediante un webhook de ingesta.
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Configurar, implementar y personalizar un webhook de ingesta

Obtenga información acerca de la configuración y personalización de un webhook de ingesta para integrar Commerce con un sistema de back office de terceros&#x200B; En este vídeo se explica cómo el webhook puede solucionar las limitaciones en la comunicación de eventos entre sistemas mediante un punto final disponible públicamente para adaptar los mensajes del sistema de terceros a la API de eventos de Adobe IO. El proceso implica configurar el webhook en el archivo `actions.config.yaml`, habilitarlo en el archivo `app.config.yaml` e implementarlo para garantizar la funcionalidad adecuada.

El vídeo explica los pasos para modificar el código del webhook para traducir eventos de terceros a formatos compatibles con los tipos de eventos suscritos de la integración. Explica cómo agregar un archivo de `event-mapping.json` para facilitar esta traducción y resalta la importancia de volver a implementar la acción de tiempo de ejecución después de realizar los cambios&#x200B; El vídeo también destaca la importancia de validar y transformar las cargas útiles de evento entrantes para alinearlas con el esquema esperado, lo que garantiza un procesamiento y una integración correctos con la API de Commerce para crear clientes.

## Público

* Desarrolladores que deseen configurar un webhook de ingesta
* Cualquiera que desee personalizar el código para la traducción de eventos
* Desarrolladores y arquitectos que deseen comprender la importancia de la autenticación y la administración de la carga útil

## Contenido de vídeo

* Configuración e implementación: En el vídeo se destaca la importancia de configurar el webhook de ingesta en el archivo `actions.config.yaml` y habilitarlo en el archivo `app.config.yaml`. También resalta la necesidad de volver a implementar el proyecto después de realizar cambios para garantizar que el webhook funcione correctamente.
* Personalización para la compatibilidad: Es crucial personalizar el código del webhook para traducir eventos de terceros a formatos que se alineen con los tipos de eventos suscritos de la integración. palo de golf Esta personalización garantiza una comunicación perfecta entre los sistemas y un procesamiento de eventos exitoso.
* Implementación de autenticación: las empresas son responsables de implementar mecanismos de autenticación adecuados para sus necesidades a fin de evitar solicitudes no autorizadas al utilizar el webhook de ingesta. Este paso es esencial para mantener la seguridad y la integridad de la integración.
* Validación y transformación de la carga útil: la validación y transformación de las cargas útiles de evento entrantes para que coincidan con el esquema esperado es vital para un procesamiento y una integración correctos con la API de Commerce. Al recortar y asignar correctamente los campos, la integración puede funcionar de forma eficaz con los datos necesarios.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Ejemplos de código

* [webhook de ingesta personalizada](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Agregar programador de ingesta](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)

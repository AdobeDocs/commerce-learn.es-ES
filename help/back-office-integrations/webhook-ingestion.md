---
title: Configuración, implementación y personalización de un webhook de ingesta
description: Aprenda a configurar, implementar y personalizar un webhook de ingesta para conectar Adobe Commerce con un sistema de back office de terceros y gestionar la traducción de eventos.
doc-type: Technical Video
duration: 697
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15870
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 412
ht-degree: 0%

---

# Configurar, implementar y personalizar un webhook de ingesta

Obtenga información acerca de la configuración y personalización de un webhook de ingesta para integrar Commerce con un sistema de back office de terceros&#x200B; En este vídeo se explica cómo el webhook puede solucionar las limitaciones en la comunicación de eventos entre sistemas al proporcionar un punto final disponible públicamente para adaptar los mensajes del sistema de terceros a la API de eventos de Adobe IO. El proceso implica configurar el webhook en el archivo `actions.config.yaml`, habilitarlo en el archivo `app.config.yaml` e implementarlo para garantizar la funcionalidad adecuada.

El vídeo explica los pasos para modificar el código del webhook para traducir eventos de terceros a formatos compatibles con los tipos de eventos suscritos de la integración. Explica cómo agregar un archivo de `event-mapping.json` para facilitar esta traducción y enfatiza la importancia de volver a implementar la acción de tiempo de ejecución después de realizar cambios.&#x200B; El vídeo también resalta la importancia de validar y transformar las cargas útiles de evento entrantes para alinearse con el esquema esperado, lo que garantiza un procesamiento y una integración correctos con la API de Commerce para crear clientes.

## Público

* Desarrolladores que deseen configurar un webhook de ingesta
* Cualquiera que desee personalizar el código para la traducción de eventos
* Desarrolladores y arquitectos que deseen comprender la importancia de la autenticación y la administración de la carga útil

## Contenido de vídeo

* Configuración e implementación: En el vídeo se destaca la importancia de configurar el webhook de ingesta en el archivo `actions.config.yaml` y habilitarlo en el archivo `app.config.yaml`. También resalta la necesidad de volver a implementar el proyecto después de realizar cambios para garantizar que el webhook funcione correctamente.
* Personalización para la compatibilidad: Es crucial personalizar el código del webhook para traducir eventos de terceros a formatos que se alineen con los tipos de eventos suscritos de la integración. Esta personalización garantiza una comunicación fluida entre los sistemas y un procesamiento de eventos exitoso.
* Implementación de autenticación: las empresas son responsables de implementar mecanismos de autenticación adecuados para sus necesidades a fin de evitar solicitudes no autorizadas al utilizar el webhook de ingesta. Este paso es esencial para mantener la seguridad y la integridad de la integración.
* Validación y transformación de carga útil: la validación y transformación de las cargas útiles de evento entrantes para que coincidan con el esquema esperado es vital para un procesamiento y una integración correctos con la API de Commerce. Al recortar y asignar correctamente los campos, la integración puede funcionar de forma eficaz con los datos necesarios.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Ejemplos de código

* [Webhook de ingesta personalizada](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Agregar programador de ingesta](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)

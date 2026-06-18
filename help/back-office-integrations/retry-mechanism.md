---
title: Uso de la funcionalidad nativa de un mecanismo de reintento
description: Aprenda a utilizar el mecanismo de reintentos de Adobe I/O Events para crear aplicaciones resistentes, lo que abarca condiciones de reintentos, estrategias de retroceso e indicadores visuales.
doc-type: Technical Video
duration: 402
last-substantial-update: 2024-07-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15872
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
TQID: https://experienceleague.adobe.com/hrzcmSY8cAke4LBLRtqfkP8-t6jP4KMoMc7iL3WPRng
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 382
ht-degree: 0%

---

# Uso del mecanismo de reintentos de Adobe I/O Events para la resiliencia de la aplicación

El vídeo describe una guía completa sobre cómo aprovechar el mecanismo de reintentos integrado de Adobe I/O Events para mejorar la resistencia de la aplicación. Descubra cómo los códigos de estado de respuesta HTTP específicos almacenan en déclencheur los reintentos de eventos. Adobe I/O Events emplea estrategias de back-off exponenciales y fijas para los reintentos, con intervalos que aumentan de un minuto a 15 minutos. La documentación también detalla cómo aparecen los indicadores de reintento en la consola del desarrollador, con indicaciones visuales como iconos de advertencia y flechas circulares que indican eventos fallidos y reintentos, respectivamente.

Obtenga información sobre cómo funciona el mecanismo de reintentos en el contexto de las acciones de tiempo de ejecución de &quot;consumidor&quot; y determine si se vuelve a intentar un evento. Las respuestas correctas se indican con un código de estado 200, mientras que las respuestas de error incluyen un objeto de error con un atributo statusCode. La acción de tiempo de ejecución &quot;consumidor&quot; determina el código de respuesta HTTP que se devolverá en función de los resultados del procesamiento descendente, lo que garantiza una administración eficiente de los eventos y las posibles activaciones exitosas.

## Público

* Desarrolladores que deseen comprender los códigos de estado de respuesta HTTP específicos que almacenan en déclencheur los reintentos de eventos.
* Equipos que deseen conocer las estrategias de back-off exponenciales y fijas empleadas por Adobe I/O Events para los reintentos.
* Desarrolladores que deseen comprender cómo los indicadores visuales de la consola de desarrollador representan los eventos fallidos y reintentados.

## Contenido de vídeo

* Adobe I/O Events tiene un mecanismo de reintentos integrado que reintenta automáticamente las activaciones de eventos en función de códigos de estado de respuesta HTTP específicos.
* El mecanismo de reintentos implementado por Adobe I/O Events implica estrategias de retroceso exponenciales y fijas.
* Indicadores visuales en Developer Console, como iconos de advertencia para eventos fallidos e iconos de flecha circulares para eventos reintentados.
* Las acciones de tiempo de ejecución &quot;consumidor&quot; desempeñan un papel crucial a la hora de determinar los códigos de estado de respuesta HTTP adecuados para la gestión de eventos.

>[!VIDEO](https://video.tv.adobe.com/v/3449076?captions=spa&learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Documentación relacionada

* [Webhook no puede gestionar un evento](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Gancho web hacia abajo y marcado como inestable](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)

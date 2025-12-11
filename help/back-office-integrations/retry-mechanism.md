---
title: Uso de la funcionalidad nativa de un mecanismo de reintento
description: Aproveche el mecanismo de reintentos de Adobe I/O Events para aplicaciones resistentes, incluidas las condiciones de reintento y los indicadores visuales.
landing-page-description: Comprenda y utilice el mecanismo de reintentos integrado de Adobe I/O Events para mejorar la resiliencia de la aplicación y administrar las activaciones de eventos de forma eficaz.
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# Aproveche el mecanismo de reintentos de Adobe I/O Events para la resiliencia de la aplicación

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

* [Webhook no puede controlar un evento](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [webhook caído y marcado como inestable](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)

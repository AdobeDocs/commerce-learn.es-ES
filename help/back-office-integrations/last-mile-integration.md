---
title: Integración de la última milla en el Starter Kit de integración de Commerce.
description: Integración de la última milla en Commerce, que resalta los vínculos de extensibilidad como validación, transformación, preprocesamiento, envío y posprocesamiento​
landing-page-description: Conozca la estructura y las funciones de los enlaces de extensibilidad en la integración de la última milla para los sistemas Commerce.
kt: 15869
doc-type: video
duration: 465
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# Integración de la última milla con Adobe Starter Kit

Obtenga información acerca de los elementos que debe tener en cuenta al iniciar la integración de la última milla con Adobe Commerce, centrándose en el uso de enlaces de extensibilidad para mejorar la conectividad con sistemas de terceros. Este vídeo describe un enfoque estructurado en el que varios vínculos como validación, transformación, preprocesamiento, envío y posprocesamiento garantizan un flujo de datos y una sincronización del sistema sin problemas. Cada vínculo tiene un propósito distinto, que incluye:

* Validación de datos entrantes con esquemas
* Transformación de objetos de datos entre sistemas
* Realización de cálculos antes de enviar información relevante
* Envío de los datos al sistema de destino

Es importante mantener archivos JavaScript independientes para cada bloque para mantener la integridad de la lógica empresarial y facilitar futuras actualizaciones del marco de trabajo, lo que garantiza una configuración de integración sólida y adaptable.

Obtenga información acerca de la importancia de las actividades posteriores al procesamiento mediante el vínculo posterior al proceso, que permite a los usuarios realizar acciones adicionales después de la sincronización de datos, como agregar comentarios a pedidos o almacenar ID externos. El vídeo incluye prácticas recomendadas como encapsular solicitudes de API dentro de bibliotecas específicas para optimizar las conexiones con sistemas de terceros. También aprenderá casos de uso típicos para cada gancho y directrices sobre la administración de diferentes escenarios.

## Público

* Desarrolladores que deseen conocer la estructura y la funcionalidad de los vínculos de extensibilidad y cómo estos pueden mejorar la conectividad con sistemas de terceros.
* Los desarrolladores que deseen conocer los casos de uso típicos y las prácticas recomendadas asociadas con cada vínculo de extensibilidad, como validación, transformación, preprocesamiento, envío y posprocesamiento, para facilitar un flujo de datos fluido, la sincronización del sistema y el mantenimiento eficaz de la configuración de la integración. palo de golf

## Contenido de vídeo

* Obtenga información acerca de la estructura de las acciones invocadas en la integración de última milla.
* Comprenda los casos de uso típicos dentro del vínculo de validación, incluida la validación de datos entrantes con esquemas y la omisión de eventos específicos basados en determinados criterios. palo de golf
* Conozca el papel del gancho de transformación en la transformación de objetos de datos entre los sistemas de origen y destino.
* Obtenga información acerca de la importancia del vínculo de envío para facilitar los datos reales que se envían al sistema de destino.

>[!VIDEO](https://video.tv.adobe.com/v/3451922?captions=spa&learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

---
title: Obtenga información acerca del Starter Kit de integración de Commerce con carpetas clave y scripts de automatización explicados
description: Obtenga información acerca de la organización del código fuente en Commerce Integration starter kit.
landing-page-description: Exploración de la organización de código Source en un Starter Kit de integración de Commerce
kt: 15868
doc-type: video
duration: 534
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
source-git-commit: b859664f02cf6eac99a551e5f58dff34ca55e37a
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Organización del código Source para Adobe Starter Kit

Obtenga información acerca de la organización del código fuente dentro del Starter Kit de integración de Adobe Commerce&#x200B; Explore la estructura del proyecto, resaltando las carpetas clave como `actions` y `scripts`, y su contenido respectivo&#x200B; La carpeta &quot;acciones&quot; contiene subcarpetas como `ingestion` y `webhook` que contienen código esencial para la administración y el seguimiento de eventos. También obtendrá información sobre las carpetas `starter-kit-info` y `scripts`. La carpeta `scripts` se centra en scripts de automatización como `commerce-event-subscribe` y `onboarding` que optimizan la configuración de eventos y la configuración de proveedores dentro del proyecto.
palo de golf
Explore la lógica detrás de la estructura del código fuente, detallando cómo las carpetas `commerce` y `external` de cada carpeta de entidad administran eventos que se originan en diferentes sistemas. En el vídeo se explica la función de la carpeta `consumer` en la distribución de eventos a la acción de tiempo de ejecución del controlador de eventos adecuada, lo que garantiza un procesamiento sin problemas. El vídeo también cubre el mecanismo de reintentos implementado en las acciones de tiempo de ejecución para gestionar eventos fallidos de forma eficaz.Comprenda la organización y la funcionalidad del código fuente en el Starter Kit de integración de Adobe Commerce, que ofrece información valiosa sobre la gestión de eventos, los scripts de automatización y las configuraciones.

## Público

* Desarrolladores que deseen comprender cómo se organiza el código fuente en las carpetas clave como `actions` y `scripts`.
* Obtenga información acerca de la carpeta `actions` que contiene subcarpetas como `ingestion` y ` webhook` que contienen código esencial para administrar eventos y realizar el seguimiento de implementaciones.
* Desarrolladores que deseen obtener información sobre la carpeta `actions` que incluye carpetas para entidades como `customer`, `order`, `product` y `stock`.

## Contenido de vídeo

* Tenga en cuenta que las cuatro carpetas principales: `actions`, `scripts`, `test` y `utils`, se centraron en las carpetas `actions` y `scripts` durante la sesión.
* Obtenga información acerca de la carpeta `actions` y cómo contiene subcarpetas cruciales como `ingestion` y `webhook`.
* Explore la carpeta `actions` y por qué existen carpetas específicas para entidades como `customer`, `order`, `product` y `stock`, cada una de las cuales contiene acciones de tiempo de ejecución estructuradas en carpetas `commerce` y `external` para administrar eventos de Commerce y sistemas de terceros de forma eficaz.
* Aprenda lo importante que es no alterar el código en la carpeta `starter-kit-info`, que contiene una acción de tiempo de ejecución utilizada por Adobe para rastrear implementaciones de proyectos basadas en el Starter Kit.
* Comprenda la carpeta `scripts` que contiene scripts de automatización como `commerce-event-subscribe` y `onboarding`, que automatizan la configuración de eventos, la configuración del proveedor y la configuración del módulo de Adobe I/O Events en Commerce.

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

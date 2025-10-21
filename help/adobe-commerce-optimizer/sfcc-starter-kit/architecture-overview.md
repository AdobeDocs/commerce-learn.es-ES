---
title: Información general de arquitectura para la aplicación del conector de nube de Salesforce Commerce
description: Obtenga información acerca de la arquitectura de Salesforce Commerce Cloud con Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Architect, Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: 54a1a8e62e86f8ae3456bb41a1b0450134f26b71
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Obtenga información acerca de la arquitectura del kit de inicio en la nube de Salesforce Commerce

Obtenga información acerca de la arquitectura y la funcionalidad del Starter Kit del conector de Commerce Optimizer, que integra Salesforce Commerce Cloud (SFC), Adobe App Builder. Adobe Commerce Optimizer utiliza el Starter Kit para optimizar la sincronización de catálogos para tiendas Edge Delivery. Explica cómo un cartucho personalizado de SFC detecta los cambios de catálogo a través de archivos de exportación delta y los expone a través de API personalizadas. Estos cambios los consumen las acciones de tiempo de ejecución de App Builder (tanto sincrónicas como asincrónicas) para realizar sincronizaciones completas y delta, actualizaciones de metadatos y sincronizaciones específicas del producto. El sistema también incluye herramientas de validación para garantizar la precisión de la tienda y utiliza la administración de estado de App Builder para rastrear el estado de sincronización y evitar conflictos.

## ¿Para quién es este vídeo?

* Arquitecto de soluciones Commerce
* Ingenieros técnicos de marketing
* Administradores de eCommerce Platform

## Contenido de vídeo

* Los cartuchos y las API personalizados de SFC detectan cambios de catálogo a través de exportaciones delta, lo que permite una sincronización de datos eficaz con Adobe App Builder.
* Las acciones de tiempo de ejecución de App Builder administran las sincronizaciones completas y delta, la validación y el seguimiento de estado para garantizar actualizaciones precisas y sin conflictos en Commerce Optimizer.

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)

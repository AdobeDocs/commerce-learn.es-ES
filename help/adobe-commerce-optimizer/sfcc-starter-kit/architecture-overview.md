---
title: Arquitectura del conector de Salesforce Commerce Cloud
description: Descubra cómo el Starter Kit Salesforce Commerce Cloud Connector utiliza acciones de tiempo de ejecución de App Builder y exportaciones delta para sincronizar catálogos con Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00.000Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%
---
# Arquitectura del Starter Kit de Salesforce Commerce Cloud

Obtenga información acerca de la arquitectura y la funcionalidad del Starter Kit de conector de Commerce Optimizer, que integra Salesforce Commerce Cloud (SFC) y Adobe App Builder. Adobe Commerce Optimizer utiliza el Starter Kit para optimizar la sincronización de catálogos para tiendas Edge Delivery. Explica cómo un cartucho personalizado de SFC detecta los cambios de catálogo a través de archivos de exportación delta y los expone a través de API personalizadas. Estos cambios los consumen las acciones de tiempo de ejecución de App Builder (tanto sincrónicas como asincrónicas) para realizar sincronizaciones completas y delta, actualizaciones de metadatos y sincronizaciones específicas del producto. El sistema también incluye herramientas de validación para garantizar la precisión de la tienda y utiliza la administración de estado de App Builder para rastrear el estado de sincronización y evitar conflictos.

## ¿Para quién es este vídeo?

* Arquitecto de soluciones Commerce
* Ingenieros técnicos de marketing
* Administradores de eCommerce Platform

## Contenido de vídeo

* Los cartuchos y las API personalizados de SFC detectan cambios de catálogo a través de exportaciones delta, lo que permite una sincronización de datos eficaz con Adobe App Builder.
* Las acciones de tiempo de ejecución de App Builder administran las sincronizaciones completas y delta, la validación y el seguimiento de estado para garantizar actualizaciones precisas y sin conflictos en Commerce Optimizer.

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)


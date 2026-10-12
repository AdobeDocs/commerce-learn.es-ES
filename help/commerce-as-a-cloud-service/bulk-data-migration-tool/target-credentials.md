---
title: 'Herramienta de migración masiva de datos: Credenciales de Target'
description: Obtenga información sobre cómo configurar las direcciones URL de la instancia de Target, las credenciales de Adobe IMS y la configuración de CDMS en el archivo .env antes de ejecutar la herramienta de migración masiva de datos.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22107
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%
---
# Configure las credenciales de destino para la herramienta de migración masiva de datos

Establezca las direcciones URL de la instancia de destino, las credenciales de Adobe IMS y la configuración de CDMS en su archivo `.env` antes de ejecutar la herramienta de migración masiva de datos. Asegúrese de que la dirección URL de Adobe IMS, la dirección URL de destino y el host de CDMS coincidan con el mismo nivel de entorno (fase o producción).

## ¿Para quién es este vídeo?

* Arquitecto de soluciones
* Ingeniero de DevOps
* Desarrollador back-end

## Contenido de vídeo

* Establezca las direcciones URL de REST y GraphQL de la instancia de destino y el ID del inquilino de destino en el archivo `.env`, con los valores del panel de información de instancia en experience.adobe.com.
* Establezca la URL de IMS de Adobe para que coincida con su nivel de entorno (fase o producción) y región.
* Recupere el ID de cliente IMS de Adobe y el secreto de cliente de **Project** > **OAuth Server-to-Server** en Adobe Developer Console.
* Copie el ID de organización de destino y configure el host de CDMS, el puerto y el servidor local para que coincidan con su entorno.

>[!VIDEO](https://video.tv.adobe.com/v/3496170?captions=spa&learn=on)

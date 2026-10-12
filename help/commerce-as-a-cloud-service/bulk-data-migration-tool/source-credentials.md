---
title: 'Herramienta de migración masiva de datos: Credenciales de Source'
description: Obtenga información sobre cómo configurar la URL de instancia de origen y las credenciales de autenticación en el archivo .env antes de ejecutar la herramienta de migración masiva de datos.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22095
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
source-wordcount: '146'
ht-degree: 0%
---
# Configurar las credenciales de origen para la herramienta de migración masiva de datos

Establezca la URL de instancia de origen y las credenciales de autenticación en su archivo `.env` antes de ejecutar la herramienta de migración masiva de datos. Los pasos de autenticación difieren ligeramente en función de si el entorno de origen es local o Adobe Commerce as a Cloud Service (PaaS).

## ¿Para quién es este vídeo?

* Arquitecto de soluciones
* Ingeniero de DevOps
* Desarrollador back-end

## Contenido de vídeo

* Establezca la URL de instancia de origen y las URL de REST y GraphQL en el archivo `.env`.
* Recupere o cree claves de integración desde **System** > **Extensions** > **Integrations** en el administrador de Adobe Commerce.
* Para generar los cuatro tokens necesarios, active la integración.
* Recupere el token de CLI de Magento de account.magento.cloud si su fuente es Adobe Commerce as a Cloud Service (PaaS).

>[!VIDEO](https://video.tv.adobe.com/v/3496142?learn=on)

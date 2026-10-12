---
title: 'Herramienta de migración masiva de datos: Credenciales de BD'
description: Obtenga información sobre cómo configurar la conexión a la base de datos de origen en el archivo .my.cnf mediante la CLI de Magento Cloud o un ID de proyecto antes de ejecutar la herramienta de migración.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
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
source-wordcount: '151'
ht-degree: 0%
---
# Configurar las credenciales de la base de datos para la herramienta de migración masiva de datos

Configure la conexión de base de datos de origen en el archivo `.my.cnf` antes de ejecutar la herramienta de migración masiva de datos. Los pasos difieren según si el entorno de origen es local o Adobe Commerce as a Cloud Service (PaaS).

## ¿Para quién es este vídeo?

* Arquitecto de soluciones
* Ingeniero de DevOps
* Desarrollador back-end

## Contenido de vídeo

* Copie `.my.cnf.example` en `.my.cnf` y cree una nueva sección con el nombre de su conexión de origen.
* Establezca el ID del proyecto en `.my.cnf` si el origen es Adobe Commerce as a Cloud Service (PaaS).
* Utilice los comandos del túnel CLI de Magento Cloud para obtener los valores de host, usuario, contraseña, puerto y base de datos.
* Confirme la conectividad de host y puerto antes de ejecutar la herramienta si el origen es local.

>[!VIDEO](https://video.tv.adobe.com/v/3496160?captions=spa&learn=on)

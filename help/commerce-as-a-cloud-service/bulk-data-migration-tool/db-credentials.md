---
title: 'Herramienta de migración masiva de datos: Credenciales de BD'
description: Obtenga información sobre cómo configurar la conexión a la base de datos de origen en el archivo .my.cnf mediante la CLI de Magento Cloud o un ID de proyecto antes de ejecutar la herramienta de migración.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22105
source-git-commit: 0dcb41e9138a36528f10333b0b5a9a9b2a39ed40
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

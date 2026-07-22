---
title: 'Herramienta de migración masiva de datos: Credenciales de Target'
description: Obtenga información sobre cómo configurar las direcciones URL de la instancia de Target, las credenciales de Adobe IMS y la configuración de CDMS en el archivo .env antes de ejecutar la herramienta de migración masiva de datos.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
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

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)

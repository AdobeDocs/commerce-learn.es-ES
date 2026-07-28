---
title: 'Herramienta de migración masiva de datos: migración multifase'
description: Obtenga información sobre cómo ejecutar una migración multifase con la herramienta de migración masiva de datos mediante el modo de mantenimiento cuando el origen debe permanecer congelado durante la migración de producción.
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00Z
jira: KT-22157
source-git-commit: c3b81a5ffc652bc7ce7640b67fe5529067607251
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---


# Ejecute una migración de varias fases con la herramienta de migración masiva de datos

Ejecute una migración de varias fases cuando el entorno de origen deba congelarse durante la extracción. Es ideal para los recortes de producción en los que los nuevos pedidos no pueden llegar a mitad de la migración. Utiliza el modo de mantenimiento y tiene cinco fases que deben ejecutarse en orden. Si la fuente puede permanecer activa, consulte el vídeo de migración de una sola fase de esta serie.

## ¿Para quién es este vídeo?

* Arquitecto de soluciones
* Ingeniero de DevOps
* Desarrollador back-end

## Contenido de vídeo

* Una distinción clave antes de comenzar: `bin/console` comandos se ejecutan en la propia herramienta de migración; `bin/magento maintenance` comandos se ejecutan en el servidor Commerce de origen. La herramienta no activa ni desactiva el modo de mantenimiento, se trata de un paso manual.
* La fase uno se ejecuta mientras el origen sigue activo: `bin console migration:before-maintenance` comprueba la configuración, inicializa el entorno, se conecta a CDMS, registra la migración, ejecuta pruebas funcionales y crea datos de prueba sintéticos. No activar el modo de mantenimiento hasta que finalice esta fase.
* La fase tres es la extracción de un entorno congelado: `bin/console migration:during-maintenance` vuelve a abrir los túneles PaaS si es necesario, extrae del origen, limpia las vistas de ensayo, carga en el destino ACCS, ejecuta la verificación y limpia los datos de prueba en el destino.

>[!VIDEO](https://video.tv.adobe.com/v/3496413?learn=on)

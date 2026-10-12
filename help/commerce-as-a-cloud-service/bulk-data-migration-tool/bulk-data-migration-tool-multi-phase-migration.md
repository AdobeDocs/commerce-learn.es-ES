---
title: 'Herramienta de migración masiva de datos: migración multifase'
description: Obtenga información sobre cómo ejecutar una migración multifase con la herramienta de migración masiva de datos mediante el modo de mantenimiento cuando el origen debe permanecer congelado durante la migración de producción.
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00.000Z
jira: KT-22157
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
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

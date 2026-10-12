---
title: 'Herramienta de migración masiva de datos: migración monofásica'
description: Aprenda a ejecutar una migración de una sola fase con la herramienta de migración masiva de datos para ejecuciones en seco y entornos en los que el origen puede permanecer activo durante la extracción.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00.000Z
jira: KT-22139
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
source-wordcount: '238'
ht-degree: 0%
---
# Ejecute una migración de una sola fase con la herramienta de migración masiva de datos

Ejecute una migración de una sola fase cuando su entorno de origen pueda permanecer activo durante la extracción: ideal para ejecuciones en seco y entornos de desarrollo o de zona protegida. Si necesita una fuente inmovilizada, como una migración total de producción en la que los nuevos pedidos no puedan llegar a mitad de la migración, consulte el vídeo de migración por fases de esta serie.

## ¿Para quién es este vídeo?

* Arquitecto de soluciones
* Ingeniero de DevOps
* Desarrollador back-end

## Contenido de vídeo

* Genere la imagen de Docker con `bin console build`; vuelva a ejecutarla únicamente si cambia el archivo Docker.
* Para iniciar el administrador de contenedores de CLI de CDMS, ejecute `bin console start` y, a continuación, abra un shell en el contenedor una vez para descargar sus dependencias.
* Para ejecutar la canalización completa de diez pasos, ejecute `bin console migration`: compruebe la configuración, inicialice el entorno, abra los túneles de PaaS, ejecute pruebas de integración, regístrese en CDMS, analice el esquema de destino, genere datos de prueba, extraiga datos de origen, cargue en ACCS, verifique sumas de comprobación, limpie y resuma.
* Compruebe el informe de resumen de la migración: el paso 8 (verificación de la integridad de los datos) registra los errores sin detener la canalización, de modo que una ejecución completada no garantiza una verificación limpia.
* Este comando de una sola fase es una canalización completa e independiente; no lo utilice como paso dentro del flujo de trabajo del modo de mantenimiento (migración por fases), que tiene sus propios comandos dedicados.

>[!VIDEO](https://video.tv.adobe.com/v/3496319?captions=spa&learn=on)

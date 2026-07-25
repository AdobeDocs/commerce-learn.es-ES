---
title: 'Herramienta de migración masiva de datos: migración monofásica'
description: Aprenda a ejecutar una migración de una sola fase con la herramienta de migración masiva de datos para ejecuciones en seco y entornos en los que el origen puede permanecer activo durante la extracción.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00Z
jira: KT-22139
source-git-commit: 838387ffddbd8bee3ef3ec22694818eb2de5fe2d
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

---
title: Agregar una tabla a una base de datos
description: '"[!DNL Commerce] tiene un mecanismo especial que le permite crear tablas de base de datos, modificar las existentes e incluso añadir algunos datos en ellas."'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: e540bc1e1c8ae5c34c16503a381f6bd5c674f824
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Agregar una tabla a una base de datos

[!DNL Commerce] tiene un mecanismo especial que le permite crear tablas de base de datos, modificar las existentes e incluso añadir algunos datos, como datos de configuración, que deben añadirse cuando se instala un módulo. Este mecanismo permite que estos cambios sean transferibles entre diferentes instalaciones.

En lugar de realizar operaciones SQL manuales repetidamente al reinstalar el sistema, los desarrolladores crean un script de instalación (o actualización) que contiene los datos. La secuencia de comandos se ejecuta cada vez que se instala un módulo.

## ¿Para quién es este vídeo?

- Desarrolladores

## Contenido del vídeo

- Creación de un módulo
- Crear script InstallSchema
- Crear script InstallData
- Añadir un nuevo módulo y comprobar que se ha creado una tabla con datos
- Creación de una secuencia de comandos UpgradeSchema
- Creación de una secuencia de comandos UpgradeData
- Ejecute secuencias de comandos de actualización y compruebe que la tabla ha cambiado

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## Recursos útiles

- [Comandos de migración](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/migration-commands.html)

---
title: Agregar una tabla a una base de datos
description: '"[!DNL Commerce] tiene un mecanismo especial que permite crear tablas de base de datos, modificar las existentes e incluso añadir algunos datos en ellas".'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: e8d2631b31319701beb327f42fdf1372d9dad9b7
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Agregar una tabla a una base de datos

>[!IMPORTANT]
>
>Ya no se recomienda, consulte https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] tiene un mecanismo especial que le permite crear tablas de base de datos, modificar las existentes e incluso añadir algunos datos en ellas, como datos de configuración, que deben añadirse cuando se instala un módulo. Este mecanismo permite que los cambios sean transferibles entre diferentes instalaciones.

En lugar de realizar operaciones SQL manuales repetidamente al reinstalar el sistema, los desarrolladores crean un script de instalación (o actualización) que contiene los datos. La secuencia de comandos se ejecuta cada vez que se instala un módulo.

## ¿Para quién es este vídeo?

- Desarrolladores

## Contenido de vídeo

- Creación de un módulo
- Crear script de InstallSchema
- Crear script InstallData
- Añada un módulo nuevo y compruebe que se ha creado una tabla con datos
- Creación de un script UpgradeSchema
- Creación de un script UpgradeData
- Ejecute scripts de actualización y compruebe que la tabla haya cambiado

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## Recursos útiles

- [Migrar scripts de instalación/actualización al esquema declarativo](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)

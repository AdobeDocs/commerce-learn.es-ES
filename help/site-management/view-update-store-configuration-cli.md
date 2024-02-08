---
title: Ver y establecer configuraciones de administración mediante la línea de comandos
description: Obtenga información sobre cómo ver y establecer configuraciones de administración mediante la línea de comandos.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
source-git-commit: a5ddf7591519b89efa2feb20ae601d36f5e5a1a7
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---


# Ver y establecer configuraciones de administración mediante la línea de comandos

Una demostración de cómo ver, establecer y encontrar valores de configuración con la CLI de Commerce. Comprenda dónde se guardan los valores y también de dónde provienen.

## ¿Para quién es este vídeo?

- Desarrolladores de Adobe Commerce

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## Algunos comandos utilizados en el tutorial

Cambie la configuración de seguridad de contraseña a recomendada:

`$ php bin/magento config:set admin/security/password_is_forced 0`

Mostrar la dirección de correo electrónico de la función de copia automática de pedidos de venta

`$ php bin/magento config:show sales_email/order/copy_to`

Mostrar el resultado vacío para una configuración que tiene un valor en el administrador

`php bin/magento config:show trans_email/ident_sales/email`

## Consultas Mysql utilizadas en el tutorial

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## Dónde encontrar el correo electrónico de ventas predeterminado

¿Cómo encontrar el valor de configuración definido en algún lugar de la base de código?
`grep -rnw vendor/magento/ -e 'sales@example.com'`

Para ver una página en un terminal y mostrar los números de línea `cat -n vendor/magento/module-email/etc/config.xml`

## Recursos adicionales

- [Herramienta Línea de comandos](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [Configurar la seguridad de administración](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}

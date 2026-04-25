---
title: Ver y establecer configuraciones de administración mediante la línea de comandos
description: Obtenga información sobre cómo ver y establecer configuraciones de administración mediante la línea de comandos.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00.000Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
TQID: https://experienceleague.adobe.com/W0kaFd-DJAPA1kFO0hWaBZXsSarojbt5Hnv3QkW85hA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 168
ht-degree: 0%

---

# Ver y establecer configuraciones de administración mediante la línea de comandos

Demostración sobre cómo ver, establecer y buscar valores de configuración con la CLI de Commerce. Comprenda dónde se guardan los valores y también de dónde provienen.

## ¿Para quién es este vídeo?

* Desarrolladores de Adobe Commerce

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3427123?learn=on)

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

Para ver una página en el terminal y mostrar los números de línea `cat -n vendor/magento/module-email/etc/config.xml`

## Recursos adicionales

* [Herramienta Línea de comandos](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html?lang=es){target="_blank"}
* [Configurar la seguridad de administración](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html?lang=es){target="_blank"}

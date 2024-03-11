---
title: Conexión y ejecución de consultas en la base de datos
description: Conozca varios métodos para conectarse a un proyecto de nube de Adobe Commerce. Aprenda a extraer una base de datos para utilizarla fuera del sitio. Aprenda algunos métodos para enmascarar PII y eliminarlo.
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-02-14T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: a951f61ff71ad3777f8aebfa3c237b2ec1a4b1a5
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# Conexión y ejecución de consultas en la base de datos de Adobe Commerce

En este tutorial, aprenderá a conectarse a un proyecto de Adobe Commerce en la nube, volcar una base de datos para su uso fuera del sitio, y enmascarar PII y eliminarla.

Puede acceder a los datos de Adobe Commerce desde su proyecto en la nube mediante cualquiera de los siguientes métodos:

* Uso de un volcado de BD local
* Una conexión de base de datos a su entorno de nube remoto mediante una aplicación como Mysql Workbench o Tables Plus
* Conéctese directamente al entorno de la nube mediante la herramienta CLI de Magento en la nube y ejecute comandos en el servidor remoto

El método preferido es hacer un volcado de la base de datos y depurarlo para eliminar cualquier información del cliente. Elimine los datos del cliente por completo si no son necesarios.

## Uso de la herramienta CLI de Adobe Commerce Cloud

La creación de un volcado de base de datos requiere que tenga [CLI de Adobe Commerce Cloud](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) instalado. En el equipo portátil local, vaya a un directorio y ejecute el siguiente comando. Asegúrese de reemplazar `your-project-id` con el ID de proyecto, que es similar a `asasdasd45q`. También debe reemplazar `your-environment-name` con el nombre de su entorno, como `master` o `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

Si no está seguro del ID del proyecto o del entorno, puede omitirlos en el comando:

`magento-cloud db:dump`

La CLI le pide que especifique el proyecto y el entorno correctos. El ejemplo siguiente muestra ese cuadro de diálogo. Este ejemplo muestra varios proyectos asignados a su cuenta, pero es probable que solo tenga disponible un proyecto.

Cambiar a directorio

```bash
cd ~/Downloads/db-tutorial 
```

Ahora ejecute el comando para crear el volcado de la base de datos

```bash
magento-cloud db:dump
```

Como no especificamos un proyecto o entorno, la CLI de Adobe Commerce hará algunas preguntas. A continuación, se muestra un cuadro de diálogo de ejemplo

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## Uso de las herramientas ECE de Adobe Commerce

Si no dispone de la herramienta CLI de Adobe Commerce, puede `ssh` en el proyecto y ejecute el `ece` mando `vendor/bin/ece-tools db-dump`: Respuesta de ejemplo:

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

Uso `SFTP` o `rsync` para extraer el volcado de la base de datos en el entorno local.

El siguiente ejemplo utiliza `rsync` para extraer el archivo a `~/Downloads/db-tutorial` carpeta.

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

La ventana del terminal mostrará alguna información, aquí hay algunos ejemplos de salida

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

Vea el contenido del archivo para comprobar que se descargó correctamente.

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

Una vez que tenga los datos, asegúrese de limpiarlos eliminando o enmascarando los datos del cliente. El siguiente script de ejemplo le ayudará a empezar.

Este ejemplo convierte los datos del cliente en cadenas aleatorias, pero conserva todos los elementos. Este ejemplo contiene algunas tablas adicionales para demostrar que el PII del cliente se puede encontrar en tablas de terceros, así como en tablas principales. Examine cuidadosamente los datos de cada tabla y enmascara o elimine los datos del cliente.

Normalmente, el arquitecto o el desarrollador principal es la única persona responsable de enmascarar y sanear los volcados de base de datos. Tener un desinfectante específico reduce la exposición de los datos sin procesar, lo que reduce la oportunidad de infringir las reglas y regulaciones de cumplimiento.

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

Como alternativa, puede eliminar los registros en lugar de ocultar la información, lo que también reduce el tamaño de la nueva base de datos. Una vez enmascarada o eliminada la PII, los datos se pueden proporcionar de forma segura a un compañero para que los utilice en su entorno local.

## Conexión de base de datos remota a un proyecto Adobe Commerce Cloud

Este método permite la edición y eliminación accidentales de datos reales. Este enfoque debe utilizarse con precaución. El uso de una copia de seguridad de base de datos y la revisión de los datos sin conexión es el enfoque preferido. Hay ocasiones en las que es necesario acceder a los datos directamente en Adobe Commerce Cloud, pero esto conlleva riesgos. No hay &quot;¿estás seguro?&quot; preguntas formuladas, por lo que es posible alterar o eliminar datos de forma involuntaria.

¡Súper importante! Hacer una conexión a base de datos remota es conveniente y el uso de datos reales en vivo, pero viene con riesgo. Yo personalmente y como Arquitecto Técnico Principal de Adobe Commerce no lo recomiendo. Es demasiado fácil olvidar que está en la base de datos remota y eliminar o modificar datos accidentalmente. Hay una opción para conectarse a la réplica de sólo lectura, pero eso proporciona cierto impacto al sitio en función de la carga de las actividades SQL. Sin embargo, como es posible, estos son los pasos para lograrlo.

Establezca un túnel SSH:

```bash
magento-cloud tunnel:open
```

Después de elegir el proyecto y el entorno, hay salida del comando que se usa en la configuración de la interfaz gráfica mysql.

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

Establezca una conexión usando una interfaz gráfica MySQL usando el `SSH tunnel opened to database at` opción de comando.

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

Ahora que tiene la información correcta, siga insertando estos valores en la consola de Cloud.

Puede encontrar el nombre de host SSH y el nombre de usuario de las credenciales de la nube en Cloud Console.

![logotipo: consola de Adobe Commerce Cloud](./assets/cloud-ui-screenshot.png "Consola de Adobe Commerce Cloud")

Este es un ejemplo: `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
El nombre de host SSH lo es todo después del signo @: `ssh.us-4.magento.cloud` en este ejemplo.
El nombre de usuario SSH lo es todo antes del signo @:  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## Búsqueda de valores para conectarse a la base de datos

El acceso a la base de datos MariaDB requiere directamente el uso de SSH para iniciar sesión en el entorno remoto de la nube y conectarse a la base de datos.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recupere las credenciales de inicio de sesión de MySQL de la `database` y `type` propiedades en la [$MAGENTO_CLOUD_RELATIONSHIPS](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) variable.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   En la respuesta, busque la información de MySQL. Por ejemplo:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

A continuación, utilice los valores de configuración en la GUI de MySQL. El siguiente ejemplo utiliza MySQL Workbench, pero cualquier aplicación que admita conexiones MySQL tendrá campos similares.

![logotipo: ejemplo de interfaz gráfica de usuario de Mysql con Mysql Workbench](./assets/mysql-workbench-after-connecting.png " Ejemplo de GUI de Mysql con Mysql Workbench")

![logo - Ejemplo de interfaz gráfica de usuario de Mysql con TablesPlus](./assets/tablesPlus-db-connection.png " Ejemplo de interfaz gráfica de Mysql con TablesPlus")

Después de configurar todo, es posible usar una GUI de MySQL para ejecutar consultas en un proyecto Adobe Commerce Cloud remoto.

## Conectarse directamente a la base de datos del proyecto en la nube para ejecutar SQL

El siguiente método utiliza el `magento-cloud` cli para conectarse directamente a la base de datos mysql y ejecutar SQL, lo que permite una consulta de base de datos más rápida. Si necesita copiar esta base de datos, consulte uno de los métodos alternativos para [crear un volcado de base de datos](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

Por ejemplo, puede encontrar todos los registros en la `core_config_data` tabla que contiene la palabra `secure` como parte de la columna `path`:

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## Recursos adicionales

[CLI de Adobe Commerce Cloud](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[Configurar el servicio MySQL](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[Configurar una conexión de base de datos MySQL remota](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[Crear volcado de base de datos en Adobe Commerce en la infraestructura en la nube](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)

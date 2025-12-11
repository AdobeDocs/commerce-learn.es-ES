---
title: Lista de comprobación previa al inicio de Adobe Commerce Cloud
description: Obtenga información acerca de la lista de comprobación previa al inicio de Adobe Commerce Cloud.
feature: Cloud
topic: Commerce, Architecture, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1668'
ht-degree: 0%

---

# Lista de comprobación previa al inicio de Commerce Cloud

A continuación se ofrece una sinopsis de la [documentación de inicio del sitio](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"} de Adobe Commerce.

Esta lista de comprobación tiene como objetivo ayudar a planificar y ejecutar correctamente el lanzamiento del sitio de Adobe Commerce Cloud. Colabore con el integrador de sistemas para Adobe Commerce Cloud para asegurarse de que todas las tareas de configuración y los elementos de la lista de comprobación se hayan completado y verificado. Si encuentra dificultades con algún elemento de la lista de comprobación o tiene alguna pregunta, póngase en contacto con el asesor técnico del cliente o el ingeniero de éxito del cliente designados. Si su cuenta no tiene un CTA/CSE asignado, puede crear un ticket de asistencia para obtener ayuda.

Si tiene un CTA/CSE asignado a la cuenta, póngase en contacto con ellos y con el administrador de cuentas al menos 4 semanas antes del lanzamiento del nuevo sitio de Adobe Commerce Cloud para notificarles su **intención** de lanzarlo.

- Algunas comprobaciones se resaltan con [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"}, ya que podrían bloquear el lanzamiento si no se revisa con cuidado.
- Asegúrese de colaborar con su desarrollador o socio de integración de sistemas para alinearse con su enfoque de implementación.

>[!IMPORTANT]
> Acepta [responsabilidad](https://experienceleague.adobe.com/es/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} por cualquier efecto adverso y riesgos asociados a la programación del lanzamiento de producción y a la estabilidad del sitio en curso, si no usa y completa esta lista de comprobación.

## &#x200B;1. Puesta En Marcha Previa

1. Revise la documentación sobre las pruebas y la puesta en marcha de la [documentación de lanzamiento del sitio](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Asegúrese de que el &quot;plan de preparación para la puesta en marcha&quot;_de_ esté completamente preparado con su socio o integrador de sistemas, incorporando todos los elementos de acción necesarios. Recuerde, mientras que la lista de comprobación previa al lanzamiento enfatiza las prácticas recomendadas de Adobe, _&#x200B;**no**&#x200B;_ reemplaza la necesidad de su propio plan de preparación para el lanzamiento.

2. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} revisa la información y las recomendaciones de las perspectivas de soporte técnico (SWAT) ([Guía del usuario](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. El usuario/comerciante final realizó UAT (Pruebas de aceptación de usuarios), incluidas las operaciones back-end.
4. El equipo del integrador de sistemas ha realizado un UAT completo de ensayo y producción. Consulte la [Documentación de Experience League](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Confirme la implementación y prueba del código en los entornos de ensayo y producción ([Más información](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}).
6. El clúster de producción se ha ampliado permanentemente a la línea de base diaria contratada. Hable con el CTA/CSE asignado para obtener más detalles o envíe un ticket de asistencia.

## &#x200B;2. Configuraciones actuales

1. Actualice Adobe Commerce y los paquetes o servicios relacionados a la [última versión](https://experienceleague.adobe.com/es/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Revise las configuraciones y servicios actuales con su socio de servicio/socio y [siga las prácticas recomendadas](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Revise el uso de disco [MySQL/Shared-Files](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## &#x200B;3. Configuraciones rápidas

1. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} Asegúrese de que el almacenamiento en caché funcione ([Caché de página completa](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} o [Almacenamiento en caché de GraphQL](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Lea la [guía de configuración rápida](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}.
2. Utilice el método GET para consultas de GraphQL en sitios web PWA/sin encabezado cuando corresponda.

   >[!NOTE]
   > Solo se pueden almacenar en caché (si corresponde) las consultas enviadas con una operación HTTP GET. [No se pueden almacenar en caché las consultas POST](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Asegúrese de que la optimización de imágenes de Fastly esté habilitada ([Consulte Optimización de imágenes de Fastly](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Compruebe que esté configurada la ubicación de escudo correcta ([Configurar caché, backends y blindaje de origen](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. El firewall de aplicaciones web (**WAF**) está funcionando. (Ver [Resolución de problemas de solicitudes bloqueadas](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}, si las hay, y limitaciones)
6. Actualice rápidamente la lista [&quot;Parámetros de URL ignorados&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} en el panel de administración para mejorar el rendimiento de la caché.

   >[!NOTE]
   > En la configuración de Fastly en _Administración > Tiendas > Configuraciones > Sistema > Caché de página completa > Configuración de Fastly > Configuración avanzada > Parámetros de URL ignorados (Global)_, puede encontrar una lista de parámetros separados por comas que Fastly debe ignorar al buscar páginas en caché. Asegúrese de volver a cargar la VCL después de modificar esta lista

## &#x200B;4. DNS y SSL

1. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} Confirme que se han solicitado todos los nombres de dominio necesarios. _(Enviar un ticket de asistencia por adelantado para cualquier dominio agregado o cambiado)_
2. El certificado SSL (TLS) [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} se ha aplicado a los dominios. Lea [este artículo](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} para obtener más información.
3. Actualice el valor del DNS [TTL (Time to Live)](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} al mínimo posible para el lanzamiento.
4. Habilitar Sendgrid SPF y DKIM

   >[!NOTE]
   > Agregue los registros CNAME de SendGrid para cada dominio a la configuración DNS. Lea [Servicio de correo electrónico de SendGrid](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} para ver cómo cambiar los dominios de los remitentes y más.

## &#x200B;5. Configuraciones de Base de Datos

Adobe Commerce Cloud utiliza un clúster MariaDB Galera como base de datos para los entornos de ensayo y producción. Los clústeres Galera son fundamentales para mejorar el rendimiento y la escalabilidad. Para obtener información sobre las prácticas óptimas y las limitaciones de las réplicas en clúster de Galera, consulte los siguientes artículos.

- [Prácticas recomendadas de configuración de MySQL](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Alertas administradas en Adobe Commerce: [alertas de MariaDB](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- Prácticas recomendadas para [configuración de la base de datos](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
- Análisis profundo de [replicaciones de clúster de Galera y control de flujo.](https://experienceleague.adobe.com/es/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. Se recomienda la conexión [MYSQL Slave](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} para mejorar el rendimiento durante las cargas altas de la base de datos.
2. Asegúrese de que el formato de fila de todas las tablas de base de datos esté establecido en [DINÁMICO en lugar de COMPACTO](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (esto es especialmente cierto para las migraciones locales a la nube).
3. Cambie el motor de almacenamiento de la base de datos de [MyISAM a InnoDB](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} para todas las tablas.
4. Revise y optimice las tablas de bases de datos que superen 1 GB de tamaño con mucha anticipación.
5. La información del esquema de la base de datos está actualizada y actualizada. (Consulte [esta guía](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## &#x200B;6. Implementaciones

1. Revise el estado ideal de la implementación de contenido estático (SCD) para reducir el tiempo de mantenimiento durante las implementaciones en el entorno de producción. Revise las [estrategias de implementación de contenido estático (SCD)](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} y la guía de [administración de la configuración de la tienda](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"}.
2. Revise la configuración de minificación para HTML, JavaScript y CSS. (Esto no se aplica a los sitios web de PWA/sin encabezado).
3. Confirme que la utilización de las siguientes variables de nube se ajusta a los fines previstos. ([SCD_MATRIX](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} y [SKIP_SCD](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## &#x200B;7. Pruebas y resolución de problemas

1. Pruebe los correos electrónicos transaccionales salientes. Más información sobre [Adobe Commerce Cloud - funcionalidad de correo SendGrid](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} ¿Algún bloqueador con Adobe?
3. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} Realice pruebas de carga y esfuerzo en la instancia de producción antes de publicar y comparta los resultados con el CTA/CSE asignado.

   >[!NOTE]
   > Una prueba de carga y esfuerzo [sirve para identificar cuellos de botella y descubrir problemas de rendimiento en la aplicación](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"}. Desempeña un papel crucial en la gestión de las expectativas con respecto al tamaño del clúster y en la determinación de los ajustes de escala necesarios para satisfacer los requisitos empresariales de forma eficaz.

   >[!IMPORTANT]
   > **_ADVERTENCIA:_** Al preparar una prueba de carga, por favor_ **_no_** envíe correos electrónicos de transacciones activos (incluso a direcciones ficticias). El envío de correos electrónicos durante la prueba puede hacer que el proyecto alcance el límite de envío predeterminado (12 K) configurado para SendGrid antes del lanzamiento.
   > 
   > - Cómo desactivar la comunicación por correo electrónico:
   >   Vaya a _Tienda > Configuración > Avanzadas > Sistema > Configuración de envío de correo electrónico_.

4. Realice pruebas de penetración de seguridad en la instancia de producción como parte del [modelo de seguridad de responsabilidad compartida](https://business.adobe.com/es/products/magento/secure-ecommerce.html){target="_blank"}. Para el cumplimiento de PCI (industria de tarjetas de pago), el sitio personalizado requiere pruebas de penetración.

## &#x200B;8. Otras configuraciones

1. Cambie la indexación a _&quot;actualización según lo programado_&quot;, excepto **_customer_grid_**, que permanece en &quot;SAVE&quot; (consulte [Modos de indexación](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. ¿Está utilizando motores de búsqueda o extensiones de terceros?
3. Confirme que las configuraciones de [SEO (Optimización de motores de búsqueda) están configuradas correctamente](https://experienceleague.adobe.com/es/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} para permitir que los indexadores/rastreadores analicen el sitio web, si corresponde.
4. Agregar redirecciones y rutas (consulte [Configurar rutas](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   >Añada redirecciones y rutas al archivo routes.yaml en el entorno de integración y compruebe la configuración en este entorno antes de implementarlo en Ensayo y producción.

       &quot;http://{all}/&quot;:
       tipo: ascendente
       flujo ascendente: &quot;mymagento:http&quot;
       
       &quot;http://{all}/&quot;:
       tipo: ascendente
       flujo ascendente: &quot;mymagento:http&quot;
   
5. Asegúrese de que XDebug esté deshabilitado si se habilita durante el desarrollo (consulte [Configurar Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}).
6. Compruebe que op-cache y otras configuraciones se hayan actualizado con precisión en el archivo php.ini ([consulte este ejemplo](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Suscribirse a la [**página de estado de Adobe Commerce**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}.
8. Suscríbase a los canales de notificación de New Relic &quot;[Alertas administradas para Adobe Commerce](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}&quot; para supervisar las métricas de rendimiento dadas ([leer más](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## &#x200B;9. Seguridad

1. Configuración del análisis de seguridad de Adobe Commerce

   >[!NOTE]
   > [Adobe Commerce Security Scan es una herramienta útil](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-scan){target="_blank"} que ayuda a descubrir versiones de software obsoletas, configuraciones incorrectas y malware potencial en el sitio. Regístrese, programe la ejecución con frecuencia y asegúrese de que los correos electrónicos se envían al contacto de seguridad técnica correcto.
   > 
   > Complete esta tarea durante UAT. Si utiliza la opción de análisis periódicos, asegúrese de programar los análisis en tiempos de baja demanda. Consulte la página [Examen de seguridad](https://account.magento.com/scanner/index/dashboard/){target="_blank"} en la cuenta de Adobe Commerce. Debe iniciar sesión en una cuenta de Adobe Commerce para acceder al análisis de seguridad.

2. Cambie la configuración predeterminada del administrador de Adobe Commerce.
3. Cambie la contraseña de administrador (consulte [Configuración de Admin Security](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Cambiar la URL de administrador (consulte [Usar una URL de administrador personalizada](https://experienceleague.adobe.com/es/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Elimine los usuarios que ya no estén en el proyecto (consulte [Crear y administrar usuarios](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}).
6. Se han configurado las contraseñas de los administradores (consulte [Requisitos de contraseña de administrador](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Configure la autenticación de doble factor (consulte [Autenticación de doble factor](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}).

## &#x200B;10. Puesta en marcha

Cuando sea el momento de la migración, realice los siguientes pasos (para obtener más información, consulte [Configuraciones de DNS](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}):

1. Acceda al servicio DNS y actualice los registros A y CNAME de cada uno de sus dominios y nombres de host:
   1. Agregue un registro CNAME para _&lt;&lt;www.yourdomain.com>_, que apunte a **prod.magentocloud.map.fastly.net**
   2. Establezca cuatro registros A para _&lt;&lt;yourdomain.com>_, que apunten a:\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Cambiar la dirección URL base de Adobe Commerce a _&lt;&lt;www.yourdomain.com>_
3. Espere a que transcurra el tiempo TTL y, a continuación, reinicie el explorador web.
4. Pruebe el sitio web.

### Si tiene algún problema que bloquee el go-live:

Si encuentra algún problema o algún problema que le impida iniciar el servicio durante el cambio, el método más rápido para obtener soporte adecuado y oportuno es utilizar el servicio de asistencia y abrir un ticket con el motivo &quot;No se puede iniciar la tienda&quot;, y llamar a un número de soporte de línea directa (consulte [la lista de números de línea directa de Adobe Commerce P1 (Prioridad 1)](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}):

- Teléfono gratuito en EE. UU.: (+1) 877 282 7436 (directo a la línea directa Adobe Commerce P1)
- Número gratuito en EE. UU.: (+1) 800 685 3620 (en el primer menú, presione 7 para la línea directa Adobe Commerce P1)
- US Local: (+1) 408 537 8777

## &#x200B;11. Publicar el lanzamiento

Una vez que el sitio esté activo, envíe por correo electrónico al CTA (Customer Technical Advisory), CSE (Customer Success Engineer) y AM (Account Manager) asignados. Sin embargo, si no tiene un administrador de cuentas asignado al proyecto, puede crear un ticket de asistencia en el que se solicite que la monitorización de High SLA se habilite una vez que el sitio se haya puesto en marcha. El CTA/CSE realiza las siguientes tareas en cuanto se verifica que el sitio se inicia con Fastly enabled y almacenamiento en caché:

- Etiquete el clúster como activo y cree un vale de soporte para activar la monitorización de High SLA (Acuerdos de nivel de servicio).
- Active New Relic Synthetics para la monitorización del tiempo de actividad.

>[!MORELIKETHIS]
> 
> - [Descripción general de la preparación para el lanzamiento - Guía de implementación](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [Iniciar lista de comprobación - Guía del usuario](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Lista de comprobación previa al inicio - Administrador del sitio/Guía del administrador de Commerce](https://experienceleague.adobe.com/es/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [Modelo de seguridad de responsabilidad compartida](https://experienceleague.adobe.com/es/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}

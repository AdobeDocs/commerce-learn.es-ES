---
title: Lista de comprobación previa al inicio de Adobe Commerce Cloud
description: Obtenga información sobre la lista de comprobación previa al inicio de Adobe Commerce Cloud y cómo solucionarla con su integrador antes del lanzamiento.
feature: Cloud
topic: Commerce, Architecture, Development
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 451
last-substantial-update: 2024-04-17T00:00:00.000Z
jira: KT-15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
TQID: https://experienceleague.adobe.com/czbb8zkX55fzgKiZthAj4whBCF-IL2bEox0M7rDr9oE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 2365
ht-degree: 0%

---

# Lista de comprobación previa al inicio

Esta página resume la [documentación de inicio del sitio](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/launch/overview){target="_blank"} de Adobe Commerce.

Esta lista de comprobación tiene como objetivo ayudar a planificar y ejecutar correctamente el lanzamiento del sitio de Adobe Commerce Cloud. Colabore con el integrador de sistemas para Adobe Commerce Cloud para asegurarse de que todas las tareas de configuración y los elementos de la lista de comprobación se hayan completado y verificado. Si encuentra dificultades con algún elemento de la lista de comprobación o tiene alguna pregunta, póngase en contacto con el asesor técnico del cliente o el ingeniero de éxito del cliente designados. Si su cuenta no tiene un CTA/CSE asignado, puede crear un ticket de asistencia para obtener ayuda.

Si tiene un CTA/CSE asignado a la cuenta, póngase en contacto con ellos y con el administrador de cuentas al menos 4 semanas antes del lanzamiento del nuevo sitio de Adobe Commerce Cloud para notificarles su **intención** de lanzarlo.

* Algunas comprobaciones se resaltan con [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"}, ya que podrían bloquear el lanzamiento si no se revisa con cuidado.
* Colabore con su desarrollador o socio de integración de sistemas para que su enfoque de implementación permanezca alineado.

>[!IMPORTANT]
> Acepta [responsabilidad](https://experienceleague.adobe.com/es/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} por cualquier efecto adverso y riesgos asociados a la programación del lanzamiento de producción y a la estabilidad del sitio en curso, si no usa y completa esta lista de comprobación.

## &#x200B;1. Publicación previa al lanzamiento

1. Revise la documentación sobre las pruebas y la puesta en marcha de la [documentación de lanzamiento del sitio](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Asegúrese de que el &quot;plan de preparación para la puesta en marcha&quot;_de_ esté completamente preparado con su socio o integrador de sistemas, incorporando todos los elementos de acción necesarios. Recuerde, mientras que la lista de comprobación previa al lanzamiento enfatiza las prácticas recomendadas de Adobe, _&#x200B;**no**&#x200B;_ reemplaza la necesidad de su propio plan de preparación para el lanzamiento.

2. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} revisa la información y las recomendaciones de las perspectivas de soporte técnico (SWAT) ([Guía del usuario](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. Confirm end users and merchants have completed UAT (User Acceptance Testing), including backend operations.
4. Confirm the system integrator team has performed end-to-end UAT on staging and production. Consulte la [Documentación de Experience League](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Confirm code deployment and testing in staging and production environments ([Read more](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/test/staging-and-production){target="_blank"}).
6. Confirme que el clúster de producción se ha ampliado permanentemente a la línea de base diaria contratada. Speak to the assigned CTA/CSE for more details, or raise a support ticket.

## &#x200B;2. Configuraciones actuales

1. Upgrade Adobe Commerce and related packages/services to the [latest version](https://experienceleague.adobe.com/es/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Revise las configuraciones y servicios actuales con su socio de servicio/socio y [siga las prácticas recomendadas](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Review the MySQL/Shared-Files [disk usage](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/storage/manage-disk-space){target="_blank"}

## &#x200B;3. Configuraciones rápidas

1. [!BADGE Blocker]{type=caution tooltip="Posible bloqueador"} Make sure that caching is working ([Full-Page Cache](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/tools/cache-management){target="_blank"} or [GraphQL caching](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Lea la [guía de configuración rápida](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/fastly){target="_blank"}.
2. Utilice el método GET para consultas de GraphQL en sitios web PWA/sin encabezado cuando corresponda.

   >[!NOTE]
   > Solo se pueden almacenar en caché (si corresponde) las consultas enviadas con una operación HTTP GET. [No se pueden almacenar en caché las consultas POST](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Ensure that Fastly Image Optimization is enabled ([See Fastly Image Optimization](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Compruebe que esté configurada la ubicación de escudo correcta ([Configurar caché, backends y blindaje de origen](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. Confirme que el firewall de aplicaciones web (**WAF**) funciona. (Consulte [Resolución de problemas de solicitudes bloqueadas](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/fastly-waf-service){target="_blank"}, si las hay, y limitaciones).
6. Actualice rápidamente la lista [&quot;Parámetros de URL ignorados&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} en el panel de administración para mejorar el rendimiento de la caché.

   >[!NOTE]
   > En la configuración de Fastly en _Administración > Tiendas > Configuraciones > Sistema > Caché de página completa > Configuración de Fastly > Configuración avanzada > Parámetros de URL ignorados (Global)_, puede encontrar una lista de parámetros separados por comas que Fastly debe ignorar al buscar páginas en caché. Vuelva a cargar la VCL después de modificar esta lista.

## &#x200B;4. DNS y SSL

1. [!BADGE Bloqueador]{type=caution tooltip="Potential Blocker"} Confirme que se han solicitado todos los nombres de dominio necesarios. _(Enviar un ticket de asistencia por adelantado para cualquier dominio agregado o cambiado.)_
2. El certificado SSL (TLS) [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} se ha aplicado a los dominios. Lea [este artículo](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} para obtener más información.
3. Actualice el valor del DNS [TTL (Time to Live)](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"} al mínimo posible para el lanzamiento.
4. Habilite SendGrid SPF y DKIM.

   >[!NOTE]
   > Agregue los registros CNAME de SendGrid para cada dominio a la configuración DNS. Lea [Servicio de correo electrónico de SendGrid](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/project/sendgrid){target="_blank"} para ver cómo cambiar los dominios de los remitentes y más.

## &#x200B;5. Configuraciones de base de datos

Adobe Commerce Cloud utiliza un clúster MariaDB Galera como base de datos para los entornos de ensayo y producción. Los clústeres Galera son fundamentales para mejorar el rendimiento y la escalabilidad. Para obtener información sobre las prácticas óptimas y las limitaciones de las réplicas en clúster de Galera, consulte los siguientes artículos.

* [Prácticas recomendadas de configuraciones de MySQL](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
* Alertas administradas en Adobe Commerce: [alertas de MariaDB](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
* Prácticas recomendadas para [configuración de la base de datos](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
* [Replicación de clúster de Galera y control de flujo](https://experienceleague.adobe.com/es/docs/commerce-learn/tutorials/extensibility/backend-development/galera-db-slow-replication){target="_blank"} (análisis más profundo)

1. Se recomienda la conexión esclava [MySQL](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} para mejorar el rendimiento durante cargas altas de la base de datos.
2. Asegúrese de que el formato de fila de todas las tablas de base de datos esté establecido en [DINÁMICO en lugar de COMPACTO](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (esto es especialmente cierto para las migraciones locales a la nube).
3. Cambie el motor de almacenamiento de la base de datos de [MyISAM a InnoDB](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} para todas las tablas.
4. Revise y optimice las tablas de bases de datos que superen 1 GB de tamaño con mucha anticipación.
5. La información del esquema de la base de datos está actualizada y actualizada. (Consulte [esta guía](https://mariadb.com/docs/server/ha-and-performance/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## &#x200B;6. Implementaciones

1. Revise el estado ideal de la implementación de contenido estático (SCD) para reducir el tiempo de mantenimiento durante las implementaciones en el entorno de producción. Revise las [estrategias de implementación de contenido estático (SCD)](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/deploy/static-content){target="_blank"} y la guía de [administración de la configuración de la tienda](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure-store/store-settings){target="_blank"}.
2. Revise la configuración de minificación para HTML, JavaScript y CSS. (Esto no se aplica a los sitios web de PWA/sin encabezado).
3. Confirme que la utilización de las siguientes variables de nube se ajusta a los fines previstos. ([SCD_MATRIX](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} y [SKIP_SCD](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## &#x200B;7. Pruebas y resolución de problemas

1. Pruebe los correos electrónicos transaccionales salientes. Más información sobre [Adobe Commerce Cloud - funcionalidad de correo SendGrid](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"}: confirma que no hay bloqueadores relacionados con Adobe que iniciar.
3. [!BADGE Bloqueador]{type=caution tooltip="Posible bloqueador"} Realice pruebas de carga y esfuerzo en la instancia de producción antes de publicar y comparta los resultados con el CTA/CSE asignado.

   >[!NOTE]
   > Una prueba de carga y esfuerzo [sirve para identificar cuellos de botella y descubrir problemas de rendimiento en la aplicación](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"}. Desempeña un papel crucial en la gestión de las expectativas con respecto al tamaño del clúster y en la determinación de los ajustes de escala necesarios para satisfacer los requisitos empresariales de forma eficaz.

   >[!IMPORTANT]
   > **_ADVERTENCIA:_** Al preparar una prueba de carga, **no** envíe correos electrónicos de transacción activos (incluso a direcciones ficticias). El envío de correos electrónicos durante la prueba puede hacer que el proyecto alcance el límite de envío predeterminado (12 K) configurado para SendGrid antes del lanzamiento.
   > 
   > * Cómo desactivar la comunicación por correo electrónico:
   >   Vaya a _Tienda > Configuración > Avanzadas > Sistema > Configuración de envío de correo electrónico_.

4. Realice pruebas de penetración de seguridad en la instancia de producción como parte del [modelo de seguridad de responsabilidad compartida](https://business.adobe.com/es/products/commerce.html){target="_blank"}. Para el cumplimiento de PCI (industria de tarjetas de pago), el sitio personalizado requiere pruebas de penetración.

## &#x200B;8. Otras configuraciones

1. Cambie la indexación a _&quot;actualización según lo programado_&quot;, excepto **_customer_grid_**, que permanece en &quot;SAVE&quot; (consulte [Modos de indexación](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. Documente cualquier motor de búsqueda o extensión de terceros en uso.
3. Confirm that [SEO (Search Engine Optimization) configurations are properly set up](https://experienceleague.adobe.com/es/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} to enable indexers/crawlers to scan the website, if relevant.
4. Add redirects and routes (see [Configure routes](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   > Add redirects and routes to the routes.yaml file in the Integration environment and verify the configuration in this environment before deploying to Staging and Production.

   Example `routes.yaml` fragment:

   ```yaml
           "http://{all}/":
               type: upstream
               upstream: "mymagento:http"
   
           "http://{all}/":
               type: upstream
               upstream: "mymagento:http"
   ```

5. Ensure Xdebug is disabled if it was enabled during development (see [Configure Xdebug for Commerce on Cloud](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/test/debug){target="_blank"}).
6. Verify that OPcache and other configurations have been accurately updated in the php.ini file ([refer to this sample](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Subscribe to the [**Adobe Commerce status page**](https://status.adobe.com/es-es/cloud/experience_cloud#/){target="_blank"}.
8. Subscribe to New Relic &quot;[Managed Alerts for Adobe Commerce](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-for-magento-commerce){target="_blank"}&quot; notification channels to monitor the given performance metrics ([read more](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## 9. Security

1. Set up the Adobe Commerce Security Scan.

   >[!NOTE]
   > [Adobe Commerce Security Scan is a useful tool](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-scan){target="_blank"} that helps discover outdated software versions, incorrect configuration, and potential malware on the site. Sign up, schedule it to run often, and make sure that emails are sent to the right technical security contact.
   > 
   > Complete this task during UAT. If you use the periodic scans option, be sure to schedule scans at low demand times. After you sign in to your Adobe Commerce account, open the Security Scan tool from your account (see [Security scan](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-scan){target="_blank"} for access and usage).

2. Change the default settings for the Adobe Commerce Admin.
3. Change the admin password (see [Configuring Admin Security](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Change the admin URL (see [Using a custom Admin URL](https://experienceleague.adobe.com/es/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Remove any users no longer on the project (see [Create and manage users](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/project/user-access){target="_blank"}).
6. Confirm administrator passwords meet requirements (see [Admin Password Requirements](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Configure two-factor authentication (see [Two-factor authentication (Admin)](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-admin#two-factor-authentication){target="_blank"}).

## 10. Go Live

When it is time to cutover, please perform the following steps (for more information, see [DNS Configurations](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}):

1. Access your DNS service and update A and CNAME records for each of your domains and hostnames:
   1. Add a CNAME record for _&lt;&lt;www.yourdomain.com>>_, pointing at **prod.magentocloud.map.fastly.net**
   2. Establezca cuatro registros A para _&lt;&lt;yourdomain.com>_, apuntando a:\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Cambie la dirección URL base de Adobe Commerce a _&lt;&lt;www.yourdomain.com>_
3. Espere a que transcurra el tiempo TTL y, a continuación, reinicie el explorador web.
4. Pruebe el sitio web.

### Si tiene algún problema que bloquee el go-live:

Si encuentra algún problema que le impida iniciar el servicio durante el cambio, la forma más rápida de obtener asistencia oportuna es usar el servicio de asistencia y abrir un ticket con el motivo &quot;No se puede iniciar la tienda&quot;, y llamar a un número de soporte de línea directa (consulte la [línea directa de notificaciones P1 de Adobe Commerce](https://experienceleague.adobe.com/es/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-p1-notification-hotline){target="_blank"} para conocer los números y procedimientos actuales):

* Teléfono gratuito en EE. UU.: (+1) 877 282 7436 (directo a la línea directa Adobe Commerce P1)
* Número gratuito en EE. UU.: (+1) 800 685 3620 (en el primer menú, presione 7 para la línea directa Adobe Commerce P1)
* US Local: (+1) 408 537 8777

## &#x200B;11. Publicar Go-Live

Una vez que la ubicación esté activa, envíe un correo electrónico al CTA (Asesor técnico del cliente), CSE (Ingeniero de éxito del cliente) y AEM (Administrador de cuentas) asignados. Sin embargo, si no tiene un administrador de cuentas asignado al proyecto, puede crear un ticket de asistencia en el que se solicite que la monitorización de High SLA se habilite una vez que el sitio se haya puesto en marcha. El CTA/CSE realiza las siguientes tareas en cuanto se verifica que el sitio se inicia con Fastly enabled y almacenamiento en caché:

* Etiquete el clúster como activo y cree un vale de soporte para activar la monitorización de High SLA (Acuerdos de nivel de servicio).
* Active New Relic Synthetics para la monitorización del tiempo de actividad.

>[!MORELIKETHIS]
> 
> * [Descripción general de la preparación para el lanzamiento - Guía de implementación](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> * [Iniciar lista de comprobación - Guía del usuario](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}
> * [Lista de comprobación previa al inicio - Administrador del sitio/Guía del administrador de Commerce](https://experienceleague.adobe.com/es/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> * [Modelo de seguridad de responsabilidad compartida](https://experienceleague.adobe.com/es/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}

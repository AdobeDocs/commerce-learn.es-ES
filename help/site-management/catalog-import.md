---
title: Obtenga información acerca de las opciones de importación de catálogos nativas de Adobe Commerce
description: Conozca algunas de las opciones nativas para importar el catálogo a su tienda de Adobe Commerce.
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
source-git-commit: 273119420a7051b1833d9b796bdce36e17d893c7
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 0%

---

# Conozca las opciones para importar un catálogo

Existen algunos métodos nativos para importar un catálogo en Adobe Commerce. Cada método tiene su propio razonamiento para el uso, junto con los pros y los contras que deben tenerse en cuenta.

Elija una de las opciones a continuación para obtener más información.

>[!BEGINTABS]

>[!TAB Manual]

## Creación manual de los productos {#manual-import}

Si tiene un catálogo limitado y las actualizaciones son poco frecuentes, crearlas manualmente podría ser la mejor opción. Se requiere tiempo para introducir cada producto y cierta formación limitada sobre cómo utilizar el administrador de comercio. La gestión manual de catálogos no es la opción adecuada para la mayoría de las tiendas, pero en determinadas situaciones puede tener sentido. Para ver documentación adicional sobre este proceso, visite [Crear un producto](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. Do not forget, you can use more than one method to manage your catalog, however once automation is used, manual edits must be limited. Automated updates have the opportunity to overwrite any changes performed manually, and therefore cause confusion. Once the integration with Adobe Commerce to manage the catalog is using automation and APIs, it is advised to restrict management of the catalog from the admin through [user roles and permissions](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.



### Cuándo considerar este enfoque

- Catálogo muy pequeño, por ejemplo menos de 50 productos
- Las actualizaciones son poco frecuentes
- Tiene todos los detalles del producto, imágenes, vídeos y no quiere tomarse el tiempo para aprender a convertir los datos a CSV
- Desea incluir imágenes y vídeos al crear los productos
- Su equipo está `not` familiarizado con las API y con el funcionamiento de OAUTH



>[!TAB CSV de administración]

## Herramienta de importación CSV de administrador {#admin-csv}

Esta herramienta permite al propietario de una tienda importar un catálogo con un CSV desde el administrador de comercio.
[Importación de datos desde el administrador de Commerce](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

Profesionales: cargar un CSV desde el administrador es un enfoque directo para la administración de catálogos. Permite actualizaciones de productos de catálogo más rápidas para un catálogo de tamaño moderado.

Desventajas:

- Lento
- El tamaño máximo de archivo de carga se define en el servidor y es posible que el propietario de una tienda no lo pueda ajustar fácilmente.
- Requiere acceso de administrador y alguien que realice la acción. La automatización es limitada
- Las importaciones programadas están limitadas a 1 vez al día como máximo
- Las imágenes y los vídeos asociados deben cargarse por separado



### Cuándo considerar este enfoque

- Tamaño de catálogo moderado
- Las actualizaciones no se realizan más de una vez al día
- tiene acceso a las configuraciones del servidor en caso de que deba aumentar el tamaño máximo de carga de archivos
- Su equipo está `not` familiarizado con las API y con el funcionamiento de OAUTH



>[!TAB API de REST en lotes]

## API de REST en lotes {#bulk-rest-api}

La API de REST en bloque permite la automatización y actualizaciones más frecuentes. Esta API es más rápida que el uso de la carga administrativa de CSV.
[Documentación de extremos masivos](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Profesionales: la capacidad de importar grandes conjuntos de datos que no están en formato CSV.

Desventajas:

- Las imágenes y los vídeos asociados deben cargarse por separado
- Puede estar limitado por restricciones de ancho de banda en el proveedor de alojamiento
- Es necesario utilizar ID de atributo de opción, no las etiquetas



### Cuándo considerar este enfoque

- El catálogo es de cualquier tamaño
- Las actualizaciones son frecuentes, se acepta más de 1x al día
- El tiempo de importación es importante, pero no lo es
- Los datos no están estructurados en formato CSV y no es posible transformarlos mediante la automatización



>[!TAB API DE REST ASÍNCRONA]

## API DE REST ASÍNCRONA {#async-rest-api}

Un extremo web asincrónico intercepta mensajes en una API web y los escribe en la cola de mensajes. Cada vez que el sistema acepta una solicitud de API de este tipo, genera un identificador UUID. Adobe Commerce incluye este UUID cuando agrega el mensaje a la cola. A continuación, un consumidor lee los mensajes de la cola y los ejecuta uno a uno.
[Documentación de extremos web asincrónicos](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Ventajas:

- Importación rápida de datos
- Se admite el ámbito de almacenamiento o puede especificar `all` para realizar la operación en todas las tiendas existentes

Desventajas:

- No se admiten las solicitudes de GET
- Se requiere que utilice los ID de atributo de opción en lugar de las etiquetas


### Cuándo considerar este enfoque

- Las importaciones son frecuentes
- No hay problema con un pequeño retraso desde el momento en que se envían a través de la API y luego se procesan desde la cola de mensajes.



>[!TAB API REST DE CSV]

## API REST DE CSV {#csv-rest-api}

Esta opción de API permite importaciones extremadamente rápidas en comparación con todas las demás opciones nativas.

[Importar datos desde la API CSV de REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Ventajas:

- Método más rápido para procesar los datos entrantes
- Se puede realizar varias veces al día
- Los datos se pueden comprimir con gzip para solicitudes grandes a fin de evitar límites de tamaño de solicitud HTTP.

Desventajas:

- Las imágenes y los vídeos asociados deben cargarse por separado
- Debe utilizar los ID de atributo de opción en lugar de las etiquetas
- Los datos deben estar en formato CSV

### Cuándo considerar este enfoque

- El catálogo es de cualquier tamaño
- Las actualizaciones son frecuentes, se acepta más de 1x al día
- El tiempo total de importación es importante
- Los datos ya están en formato CSV o se pueden transformar fácilmente mediante la automatización



>[!ENDTABS]

## Recursos adicionales

- [Importación de datos con el nuevo CSV REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [Documentación principal de importación de datos](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
- [Notas de la versión de Adobe Commerce 2.4.6](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
- [Usuarios, funciones y permisos](../site-management/users-roles-permissions.md){target="_blank"}

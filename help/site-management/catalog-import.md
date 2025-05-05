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
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
source-git-commit: b0fe49352b00a68554e662327cd66983c30d8285
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---

# Opciones para importar un catálogo

Existen algunos métodos nativos para importar un catálogo en Adobe Commerce. Cada método tiene su propio razonamiento para el uso, junto con los pros y los contras que deben tenerse en cuenta.

Elija una de las opciones a continuación para obtener más información.

>[!BEGINTABS]

>[!TAB Manual]

## Creación manual de los productos {#manual-import}

Si tiene un catálogo limitado y las actualizaciones son poco frecuentes, crearlas manualmente podría ser la mejor opción. Se requiere tiempo para introducir cada producto y cierta formación limitada sobre cómo utilizar el administrador de Commerce. La gestión manual de catálogos no es la opción adecuada para la mayoría de las tiendas, pero en determinadas situaciones puede tener sentido. Para ver documentación adicional para este proceso, visita [Crear un producto](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html?lang=es){target="_blank"}. No olvide que puede utilizar más de un método para administrar el catálogo. Sin embargo, una vez que se utiliza la automatización, las ediciones manuales deben estar limitadas. Las actualizaciones automatizadas tienen la oportunidad de sobrescribir cualquier cambio realizado manualmente y, por lo tanto, causar confusión. Una vez que la integración con Adobe Commerce para administrar el catálogo esté usando automatización y API, se recomienda restringir la administración del catálogo desde el administrador a través de [roles de usuario y permisos](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html?lang=es){target="_blank"}.

### Cuándo considerar este enfoque

- Catálogo muy pequeño, por ejemplo menos de 50 productos
- Las actualizaciones son poco frecuentes
- Tiene todos los detalles del producto, imágenes, vídeos y no quiere tomarse el tiempo para aprender a convertir los datos a CSV
- Desea incluir imágenes y vídeos al crear los productos
- Su equipo está `not` familiarizado con las API y con el funcionamiento de OAUTH

>[!TAB CSV de administrador]

## Herramienta de importación CSV de administrador {#admin-csv}

Esta herramienta permite al propietario de una tienda importar un catálogo con un CSV desde el administrador de comercio.
[Importar datos del administrador de Commerce](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=es){target="_blank"}

Ventajas:
Cargar un CSV del administrador es un enfoque directo para la administración de catálogos. Permite actualizaciones de productos de catálogo más rápidas para un catálogo de tamaño moderado.

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
[Documentación de extremos en lotes](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Ventajas:
La capacidad de importar grandes conjuntos de datos que no están en formato CSV.

Desventajas:

- Las imágenes y los vídeos asociados deben cargarse por separado
- Puede estar limitado por restricciones de ancho de banda en el proveedor de alojamiento

### Cuándo considerar este enfoque

- El catálogo es de cualquier tamaño
- Las actualizaciones son frecuentes, se acepta más de 1x al día
- El tiempo de importación es importante, pero no esencial, y se acepta un breve retraso en el procesamiento de los datos de importación
- Los datos no están estructurados en formato CSV y no es posible transformarlos mediante la automatización

>[!TAB API DE REST ASINCRÓNICA]

## API DE REST ASÍNCRONA {#async-rest-api}

Un extremo web asincrónico intercepta mensajes en una API web y los escribe en la cola de mensajes. Cada vez que el sistema acepta una solicitud de API de este tipo, genera un identificador UUID. Adobe Commerce incluye este UUID cuando agrega el mensaje a la cola. A continuación, un consumidor lee los mensajes de la cola y los ejecuta uno a uno.
[Documentación asincrónica de extremos web](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Ventajas:

- Importación rápida de datos
- Se admite el ámbito del almacén o puede especificar `all` para que realice la operación en todos los almacenes existentes

Desventajas:

- No se admiten las solicitudes de GET

### Cuándo considerar este enfoque

- Las importaciones son frecuentes
- No hay problema con un pequeño retraso desde el momento en que se envían a través de la API y luego se procesan desde la cola de mensajes.


>[!TAB API DE REST DE CSV]

## API REST DE CSV {#csv-rest-api}

Esta opción de API permite importaciones extremadamente rápidas en comparación con todas las demás opciones nativas.

[Importar datos desde la API de CSV de REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Ventajas:

- Método más rápido para procesar los datos entrantes
- Se puede realizar varias veces al día
- Los datos se pueden comprimir con gzip para solicitudes grandes a fin de evitar límites de tamaño de solicitud HTTP.

Desventajas:

- Las imágenes y los vídeos asociados deben cargarse por separado
- Los datos deben estar en formato CSV

### Cuándo considerar este enfoque

- El catálogo es de cualquier tamaño
- Las actualizaciones son frecuentes, se acepta más de 1x al día
- El tiempo total de importación es importante
- Los datos ya están en formato CSV o se pueden transformar fácilmente mediante la automatización

>[!ENDTABS]

## Recursos adicionales

- [Importar datos con el nuevo CSV de REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [Importar documentación principal de datos](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=es){target="_blank"}
- [Notas de la versión de Adobe Commerce 2.4.6](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html?lang=es){target="_blank"}
- [Usuarios, funciones y permisos](../site-management/users-roles-permissions.md){target="_blank"}

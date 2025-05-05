---
title: Crear un producto descargable
description: Obtenga información sobre cómo crear un producto descargable mediante la API de REST y el administrador de Adobe Commerce.
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Crear un producto descargable

Obtenga información sobre cómo crear un producto descargable mediante la API de REST y el administrador de Adobe Commerce.

## ¿Para quién es este vídeo?

- Administradores de sitios web
- Comerciantes de comercio electrónico
- Nuevos desarrolladores de Adobe Commerce que desean aprender a crear productos en Adobe Commerce mediante la API de REST

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3453945?learn=on&captions=spa)

## Dominios descargables permitidos

Debe especificar qué dominios pueden permitir las descargas. Los dominios se agregan al archivo `env.php` del proyecto mediante la línea de comandos. El archivo `env.php` detalla los dominios que pueden contener contenido descargable. Se produce un error si se crea un producto descargable con la API de REST _antes_ de que se actualice el archivo `php.env`:

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

Para establecer el dominio, conéctese al servidor: `bin/magento downloadable:domains:add www.example.com`

Una vez que finalice, `env.php` se modifica dentro de la matriz _downloadable_domains_.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

Ahora que el dominio se agrega a `env.php`, puede crear un producto descargable en el administrador de Adobe Commerce o mediante la API de REST.

Consulte [Referencia de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=es#downloadable_domains) para obtener más información.

>[!IMPORTANT]
>En algunas versiones de Adobe Commerce, es posible que reciba el siguiente error cuando se edita un producto en el administrador de Adobe Commerce. El producto se crea usando la API de REST pero la descarga vinculada tiene un precio de `null`.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

Para corregir este error, use el vínculo de actualización API: `POST V1/products/{sku}/downloadable-links.`

Consulte la sección [Actualizar un vínculo de descarga de producto mediante cURL](#update-downloadable-links) para obtener más información.

## Crear un producto descargable mediante cURL (descarga desde servidor remoto)

Este ejemplo muestra cómo crear un producto descargable mediante cURL cuando el archivo que se va a descargar no está en el mismo servidor. Este caso de uso es típico si el archivo se almacena en un compartimento de S3 u otro administrador de recursos digitales.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## Creación de un producto descargable mediante cURL (descarga desde el servidor de aplicaciones de Commerce)

En este ejemplo se muestra cómo utilizar cURL para crear un producto descargable desde el administrador de Adobe Commerce cuando el archivo se almacena en el mismo servidor que la aplicación de Adobe Commerce.

En este caso de uso, cuando el administrador que administra el catálogo elige `upload file`, el archivo se transfiere al directorio `pub/media/downloadable/files/links/`.  La automatización crea y mueve los archivos a sus respectivas ubicaciones en función del siguiente patrón:

- Cada archivo cargado se almacena en una carpeta basada en los dos primeros caracteres del nombre del archivo.
- Cuando se inicia la carga, la aplicación de Commerce crea o utiliza carpetas existentes para transferir el archivo.
- Al descargar el archivo, la sección `link_file` de la ruta de acceso utiliza la parte de la ruta anexada al directorio `pub/media/downloadable/files/links/`.

Por ejemplo, si el archivo cargado se llama `download-example.zip`:

- El archivo se ha subido a la ruta de acceso `pub/media/downloadable/files/links/d/o/`.
Los subdirectorios `/d` y `/d/o` se crean si no existen.

- La ruta final al archivo es `/pub/media/downloadable/files/links/d/o/download-example.zip`.

- El valor `link_url` de este ejemplo es `d/o/download-example.zip`

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## Obtención de un producto con curl

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Actualizar el producto con Postman {#update-downloadable-links}

Usar el extremo `rest/all/V1/products/{sku}/downloadable-links`
`SKU` es el identificador de producto que se generó cuando se creó el producto. Por ejemplo, en el ejemplo de código siguiente, es el número 39, pero asegúrese de que se actualiza para utilizar el ID de su sitio web. Esto actualiza los vínculos de los productos descargables.

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## Actualización de un vínculo de descarga de producto mediante CURL

Al actualizar un vínculo de descarga de producto mediante cURL, la URL incluye el SKU del producto que se actualiza.  En el siguiente ejemplo de código, el SKU es `abcd12345`. Cuando envíe el comando, cambie el valor para que coincida con el SKU del producto que desea actualizar.

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## Recursos adicionales

- [Tipo de producto descargable](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=es){target="_blank"}
- [Guía de configuración de dominios descargables](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=es#downloadable_domains){target="_blank"}
- [Tutoriales de REST de Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}

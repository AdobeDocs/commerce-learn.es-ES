---
title: Crear un producto agrupado
description: Obtenga información sobre cómo crear un producto agrupado mediante la API de REST y el administrador de Commerce.
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
source-git-commit: 76a67af957b0d8c1eb64ad42f92412f338650d4b
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Crear un producto agrupado

Un producto agrupado consiste en productos independientes simples que se presentan como un grupo. Puede ofrecer variaciones de un solo producto o agruparlas por temporada o tema. Antes de crear un producto agrupado, compruebe que todos los productos simples que se incluyen en el grupo están disponibles en Adobe Commerce y cree los que no existan.

En este tutorial, aprenderá a crear un producto agrupado mediante la API de REST y el administrador de Adobe Commerce.

Utilice la API de REST para crear un producto de grupo en Admin:

1. Cree un producto agrupado vacío.
1. Cree productos simples para utilizarlos en el producto agrupado.
1. Rellene el producto agrupado vacío con productos simples.
1. Cree un producto agrupado vacío y asocie los productos simples.

   Cuando asocia productos simples al producto agrupado, el atributo de criterio de ordenación (`position`) de la carga útil lo utiliza el front-end para mostrar los productos asociados en el orden deseado. Si no se especifica el atributo `position`, los productos se muestran en el orden en que se agregaron al producto agrupado.

Al crear productos agrupados desde el administrador de Adobe Commerce, cree primero los productos simples. Cuando esté listo para crear el producto agrupado, asocie los productos simples asignándolos al producto agrupado en un lote.

## ¿Para quién es este vídeo?

- Administradores de sitios web
- Comerciantes de comercio electrónico
- Nuevos desarrolladores de Adobe Commerce que desean aprender a crear productos agrupados en Adobe Commerce mediante la API de REST.

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## Configuración del producto agrupado

En este ejemplo, hay tres productos simples (creados primero) y un producto agrupado. Se asocian dos productos simples con el producto agrupado y, a continuación, se agrega el tercer producto simple al producto agrupado.

## Cree el primer producto simple con cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## Cree el segundo producto simple con cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## Cree el tercer producto simple con cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## Creación de un producto agrupado vacío mediante cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## Añadir el primer y el segundo producto simple al producto agrupado

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## Añadir el tercer producto simple al producto agrupado existente

Incluya el número de posición apropiado (cualquier cosa excepto `1` o `2`), que se utilizan para los dos primeros productos asociados originalmente al producto agrupado. Para este ejemplo, la posición es `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## Eliminar un producto simple de un producto agrupado

Para [eliminar un producto simple](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) de un producto agrupado, use: `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

Para descubrir qué usar como `{type}`, use xdebug para capturar la solicitud y evaluar los $linkTypes: `related`, `crosssell`, `uupsell` y `associated`.
![Tipos de vínculos de productos agrupados: texto alternativo](/help/assets/site-management/catalog/grouped-types.png "Tipos de vínculos de productos agrupados capturados durante la sesión de xdebug")

Al vincular los productos simples al producto agrupado, la carga útil contenía algunas secciones similares a:

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

En la carga útil, el valor `link_type` `associated` proporciona el valor `{type}` requerido en la solicitud del DELETE. La dirección URL de la solicitud será similar a `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

Consulte la solicitud cURL para eliminar el producto simple con el SKU `product-sku-three` del producto agrupado con el SKU `my-new-grouped-product`:

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## Obtener un producto agrupado mediante cURL

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## Recursos adicionales

- [Crear y administrar productos agrupados](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [Producto agrupado](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
- [Tutoriales de REST de Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}

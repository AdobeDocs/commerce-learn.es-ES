---
title: Crear un producto agrupado
description: Obtenga información sobre cómo crear un producto agrupado mediante la API de REST y el administrador de Commerce.
kt: 14585
doc-type: video
duration: 979
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
TQID: 'https://experienceleague.adobe.com/nosJh3ytiC54wmNWaUmSu9qjZCN-ssjolNZD702EpEg'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: cc04bd17-78d5-5120-8c3d-1b8a57e49280
    internal-label: Integration
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: bb2df8be-afdd-4818-b6b5-95ca1dd3bc3a
    internal-label: Admin workspace
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '559'
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

* Administradores de sitios web
* Comerciantes de comercio electrónico
* Nuevos desarrolladores de Adobe Commerce que desean aprender a crear productos agrupados en Adobe Commerce mediante la API de REST.

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

En la carga útil, el valor `link_type` `associated` proporciona el valor `{type}` requerido en la solicitud de DELETE. La dirección URL de la solicitud será similar a `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

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

* [Creación y administración de productos agrupados](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
* [Producto agrupado](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html?lang=es){target="_blank"}
* [Tutoriales de REST de Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [ReDoc de REST de Adobe Commerce](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}

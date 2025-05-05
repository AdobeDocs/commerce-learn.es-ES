---
title: Crear un producto configurable
description: Obtenga información sobre cómo crear un producto configurable mediante la API de REST y el administrador de Commerce.
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# Crear un producto configurable

Un producto configurable es un producto principal de varios productos simples. Defina un producto configurable para exigir al comprador que realice una o más selecciones para seleccionar una variación de producto específica. Por ejemplo, si el producto es una camisa, el comprador debe elegir las opciones de talla y color para seleccionarla.

Aunque un producto configurable utiliza más SKU y puede tardar un poco más en configurarse, puede ahorrarle tiempo al final. Si planea hacer crecer su negocio, el tipo de producto configurable es una buena opción para productos con varias opciones.

Antes de crear un producto configurable, compruebe que todos los productos simples que se incluyen en el producto configurable estén disponibles en Adobe Commerce. Cree cualquier que no exista.

En este tutorial, aprenderá a crear un producto configurable mediante la API de REST y el administrador de Adobe Commerce.

Utilice la API de REST para crear un producto configurable:

1. Obtenga los atributos de un [conjunto de atributos](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html?lang=es) para usar los números de identificación en llamadas de API posteriores.
1. Cree productos simples para utilizarlos en el producto configurable.
1. Cree un producto configurable vacío y asocie los productos simples.
1. Establezca los atributos del producto para el producto configurable.
1. Rellene el producto configurable vacío con productos simples.
1. Obtenga el producto configurable y todos los atributos.
1. Obtenga los productos secundarios asignados para el producto configurable.
1. Elimine la asociación de productos simples a productos configurables.

Al crear productos configurables desde el administrador de Adobe Commerce, puede crear primero los productos simples o utilizar la herramienta automatizada que crea nuevos productos simples para utilizarlos con el asistente.

## ¿Para quién es este vídeo?

- Administradores de sitios web
- Comerciantes de comercio electrónico
- Nuevos desarrolladores de Adobe Commerce que desean aprender a crear productos configurables en Adobe Commerce mediante la API de REST

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## Obtener los atributos de color mediante cURL

En este ejemplo, se devuelve todo el conjunto de atributos con todos los atributos individuales para el conjunto de atributos 10. Puede ser larga, cientos de líneas no son infrecuentes. Al revisar la respuesta, es probable que el ID de atributo de color esté en el medio. Acelere la búsqueda de estos valores utilizando grep u otros métodos para buscar los resultados. Mi respuesta estaba cerca de la línea 665 y se incluye en el siguiente fragmento de la respuesta JSON.

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


Para recuperar los identificadores de atributo para configurar su producto configurable, actualice la porción `attribute-sets/10/attributes` de la siguiente solicitud cURL para reemplazar `10` con el identificador del conjunto de atributos en su entorno. Esta solicitud utiliza el método de GET.

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Cree el primer producto simple con cURL

### Ajuste de ID de entorno y detalles del producto

Cree el primer producto simple con la API para enviar la siguiente solicitud de POST mediante cURL.

Antes de enviar la solicitud, actualice el ejemplo con los valores de su entorno.

- Cambie `"attribute-set": 10` para reemplazar `10` con el ID del conjunto de atributos de su entorno.
- Cambie `"value": "13"` para reemplazar `13` con el valor de su entorno.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## Cree el segundo producto simple con cURL

Cree el segundo producto simple con la API para enviar la siguiente solicitud de POST mediante cURL.

Antes de enviar la solicitud, actualice el ejemplo con los valores de su entorno.

- Cambie `"attribute_set_id": 10,` y reemplace `10` con el identificador del conjunto de atributos de en su entorno.
- Cambie `"value": "14"` y reemplace `14` por el valor de su entorno.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## Cree el tercer producto simple con cURL

Cree el tercer producto simple enviando la siguiente solicitud de POST mediante cURL.

Antes de enviar la solicitud, actualice el ejemplo con los valores de su entorno.

- Cambie `"attribute_set_id": 10,` para reemplazar `10` con el ID del conjunto de atributos de su entorno.
- Cambie `"value": "15"` y reemplace `15` por el valor de su entorno.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## Crear un producto configurable vacío mediante cURL

Cree un producto configurable vacío enviando la siguiente solicitud de POST mediante cURL.

Antes de enviar la solicitud, actualice el ejemplo con los valores de su entorno.

- Cambie `"attribute_set_id": 10,` y reemplace `10` con el ID del conjunto de atributos de su entorno.
- Cambie `"value": "93"` y reemplace `93` por el valor de su entorno.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## Definir las opciones disponibles para el producto configurable

Defina las opciones disponibles para el producto configurable enviando la siguiente solicitud de POST mediante cURL.

Antes de enviar la solicitud, cambie `"attribute_id": 93,` para reemplazar `93` con el ID de atributo de su entorno.

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

Si olvida establecer las opciones del producto configurable (principal), obtendrá un error cuando intente asociar un producto secundario al producto configurable. El mensaje de error es similar al siguiente ejemplo:

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## Vincule el producto secundario al configurable

Ahora, ha creado tres productos simples:

- `"Kids Hawaiian Ukulele Red"`,
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

Añada estos productos simples como productos secundarios del producto configurable enviando la siguiente solicitud del POST. Envíe una solicitud independiente para cada producto.

Para cada solicitud, actualice el valor `childSKU` con el valor del producto secundario que está agregando. El siguiente ejemplo asigna el producto simple `kids-Hawaiian-Ukulele-red` al producto configurable con el SKU `Kids-Hawaiian-Ukulele-red`.


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## Obtener un producto configurable mediante cURL

Ahora que ha creado un producto configurable con tres SKU secundarias asignadas. Puede ver los ID vinculados de los productos asignados enviando la siguiente solicitud de GET mediante cURL. Esta solicitud devuelve información detallada sobre el producto configurable.

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

Lo siguiente utiliza el método de GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Obtener el producto secundario asociado a un producto configurable

Devuelva solo los elementos secundarios asociados con el producto configurable enviando la siguiente solicitud de GET. La respuesta incluirá todos los atributos del producto secundario, incluidos el SKU y el precio.

Lo siguiente utiliza el método de GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Eliminar o quitar un producto secundario del elemento principal configurable

Puede quitar un producto secundario de un producto configurable sin eliminar el producto del catálogo enviando la siguiente solicitud de DELETE mediante cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Recursos adicionales

- [Crear un tutorial de producto configurable](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [Producto configurable](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html?lang=es){target="_blank"}
- [Tutoriales de REST de Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}

---
title: Crear un producto sencillo
description: Aprenda a crear un producto sencillo mediante la API de REST y el administrador de Commerce.
kt: 14446
doc-type: video
duration: 197
audience: all
activity: use
last-substantial-update: 2023-11-14T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 62ba8e71-dcff-4c72-8850-029be2c42620
TQID: https://experienceleague.adobe.com/LaJ36-CuEiOAzS4VzeONwI6HCfdOFExwuKusXFmqzns
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 106
ht-degree: 0%

---

# Crear un producto sencillo

Aprenda a crear un producto sencillo mediante la API de REST y el administrador de Adobe Commerce.

## ¿Para quién es este vídeo?

* Administradores de sitios web
* Comerciantes de comercio electrónico
* Nuevos desarrolladores de Adobe Commerce que desean aprender a crear productos en Adobe Commerce mediante la API de REST.

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3425650?learn=on)

## Creación de un producto con curl

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "some-product-sku",
    "name": "My curl created product test",
    "attribute_set_id": 4,
    "price": 222,
    "type_id": "simple"
  }
}
```

## Obtención de un producto con curl

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## Recursos adicionales

* [Tutoriales de REST de Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [ReDoc de REST de Adobe Commerce](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}

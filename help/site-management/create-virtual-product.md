---
title: Crear un producto virtual
description: Obtenga información sobre cómo crear un producto virtual mediante la API de REST y el administrador de Commerce.
kt: 14464
doc-type: video
duration: 213
audience: all
activity: use
last-substantial-update: 2023-11-15T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5149b6b4-5fbf-467a-a412-6dce7188bcb9
TQID: https://experienceleague.adobe.com/OktfC13mVgaoxyfKlZYH6HPDoCqP-m07pz5VEQ4cssM
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
source-wordcount: 107
ht-degree: 0%

---

# Crear un producto virtual

Obtenga información sobre cómo crear un producto virtual mediante la API de REST y el administrador de Adobe Commerce.

## ¿Para quién es este vídeo?

* Administradores de sitios web
* Comerciantes de comercio electrónico
* Nuevos desarrolladores de Adobe Commerce que desean aprender a crear productos en Adobe Commerce mediante la API de REST.

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3425723?learn=on)

## Creación de un producto virtual con curl

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "postman-rest-virtual-product-test",
    "name": "POSTMAN virtual product test",
    "attribute_set_id": 4,
    "price": 44,
    "type_id": "virtual"
  }
}
'
```

## Obtención de un producto con curl

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Admin-created-virtual-product' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e'
```

## Recursos adicionales

* [Tutoriales de REST de Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [ReDoc de REST de Adobe Commerce](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}

---
title: Explore New Customer REST APIs
description: Discover how to use new customer REST APIs in Adobe Commerce Cloud Service. Ideal for architects and developers.
feature: REST, Customers, Saas
topic: Development, Integrations
role: Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2026-01-27T00:00:00.000Z
jira: KT-20160
exl-id: f40d9b21-1f41-4c76-84a9-161168dbfb1a
TQID: https://experienceleague.adobe.com/DiP21e4T-iLM-IuOVDVkJIvHOJ6y-q4IIdSKVplxcX0
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 505
ht-degree: 0%

---

# Customer REST API

Learn to use new customer REST APIs in Adobe Commerce as a Cloud Service. This tutorial is perfect for architects and developers looking to integrate and optimize API solutions effectively.

## Who is this video for?

* Backend developers responsible for building integrations with Adobe Commerce
* Technical architects designing customer management workflows for headless commerce implementations

## Video content

* Authenticate with Adobe IMS using server-to-server credentials to obtain an access token for API requests
* Use the correct REST API endpoint format for Commerce as a Cloud Service
* Create and update customer accounts programmatically using POST and PUT requests with proper JSON payloads

>[!VIDEO](https://video.tv.adobe.com/v/3479361?learn=on)

## Code samples

Before starting, gather all the required values from [Experience Cloud](https://experience.adobe.com) and the [Adobe Developer Console](https://developer.adobe.com/console). Having these values ready ensures a smooth setup process.

>[!NOTE]
>
>Make sure you are working in the correct organization. Your organization selection affects which instances and environments are visible in both Experience Cloud and the Developer Console.

### Instance details - experience.adobe.com

The instance details contain things like your Instance ID, GraphQL endpoints, credentials.

### Developer details - https://developer.adobe.com/console/

The Developer Console is where you manage your API credentials, including client IDs, client secrets, and access tokens. You can also create new credential types, such as Server-to-Server or Native App authentication.

## Requisitos previos

| Elemento | Valor | Donde es este valor |
|--- |--- |--- |
| ID de instancia | `<instance_id>` | experience.adobe.com |
| Punto final REST | `<rest_endpoint>` | experience.adobe.com |
| ID de cliente | `<client_id>` | developer.adobe.com/console |
| Secreto del cliente | `<client_secret>` | developer.adobe.com/console |


## Paso 1: Obtener token de acceso (autenticación de servidor a servidor)

>[!IMPORTANT]
>
> Las variables mostradas en este ejemplo no son válidas. Utilice el &lt;client_id> y el &lt;client_secret> de las credenciales del proyecto.

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**Respuesta de ejemplo:**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## Paso 2: Crear un cliente

>[!IMPORTANT]
>
> La URL proporcionada en este ejemplo no es válida. Utilice la URL base de REST. Intercambie &quot;&lt;rest_endpoint>&quot; con su URL. Se parece a este(a) `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.
>
> Este extremo no tiene /rest/ como parte de la dirección URL. Incluir esto conduce a un error.

**Punto final:** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**Respuesta:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## Paso 3: Actualización de un cliente

>[!IMPORTANT]
>
> La URL proporcionada en este ejemplo no es válida. Utilice la URL base de REST. Intercambie &quot;&lt;rest_endpoint>&quot; con su URL. Se parece a este(a) `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.

El número `5` del siguiente ejemplo es el ID del cliente creado anteriormente mediante POST `"id": 5,`. Asegúrese de cambiar `5` al identificador que se haya devuelto en la solicitud.

**Punto final:** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**Respuesta:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## Script completo (todo en uno)

>[!IMPORTANT]
>
> Las variables mostradas en este ejemplo no son válidas. Utilice el ID de cliente y el secreto de cliente de las credenciales del proyecto. Utilice la URL base de REST. Intercambie &quot;&lt;rest_endpoint>&quot; con la URL de su extremo REST desde experience.adobe.com. Se parece a este(a) `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`.

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## Notas importantes sobre este tutorial

1. **Ruta de URL**: usar `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` — **NO** `https://<host>/rest/<store-view-code>/V1/customers`
1. **Autenticación**: este tutorial usó el tipo de concesión Servidor a servidor (`client_credentials`)
1. **Ámbito requerido**: `commerce.accs`
1. **Caducidad del token**: 86400 segundos (24 horas)

## Referencias

* [Notas de la versión de Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/release-notes)
* [Referencia de API de REST de SaaS](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [Guía de autenticación de usuario](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)

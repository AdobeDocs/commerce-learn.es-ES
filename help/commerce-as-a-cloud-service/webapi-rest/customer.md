---
title: Explorar las nuevas API de REST del cliente
description: Descubra cómo utilizar las nuevas API de REST de cliente en Adobe Commerce Cloud Service. Ideal para arquitectos y desarrolladores.
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: 9e644b4dac87eeb98c9e97c7a931a460e1ef3b81
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# ACCS: nuevas API de REST de cliente

Aprenda a utilizar las nuevas API de REST del cliente en Adobe Commerce as a Cloud Service. Este tutorial es perfecto para arquitectos y desarrolladores que buscan integrar y optimizar las soluciones de API de forma eficaz.

## ¿Para quién es este vídeo?

* Desarrolladores back-end responsables de crear integraciones con Adobe Commerce
* Arquitectos técnicos que diseñan flujos de trabajo de administración de clientes para implementaciones de comercio sin encabezado

## Contenido de vídeo

* Autentique con Adobe IMS mediante credenciales de servidor a servidor para obtener un token de acceso para solicitudes de API
* Utilice el formato de extremo de API de REST correcto para Commerce as a Cloud Service
* Cree y actualice cuentas de cliente mediante programación utilizando solicitudes POST y PUT con cargas JSON adecuadas

>[!VIDEO](https://video.tv.adobe.com/v/3479365/?captions=spa&learn=on&enablevpops)

## Muestras de código

Antes de empezar, recopile todos los valores necesarios de [Experience Cloud](https://experience.adobe.com) y [Adobe Developer Console](https://developer.adobe.com/console). Tener estos valores listos garantiza un proceso de configuración suave.

>[!NOTE]
>
>Asegúrese de que está trabajando en la organización correcta. La selección de su organización afecta a qué instancias y entornos son visibles tanto en Experience Cloud como en Developer Console.

### Detalles de instancia: experience.adobe.com

Los detalles de la instancia contienen elementos como su ID de instancia, puntos de conexión de GraphQL o credenciales.

### Detalles del desarrollador: https://developer.adobe.com/console/

En Developer Console es donde administra las credenciales de la API, incluidos los ID de cliente, los secretos de cliente y los tokens de acceso. También puede crear nuevos tipos de credenciales, como autenticación de servidor a servidor o de aplicación nativa.

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

## Secuencia de comandos completa (todo en uno)

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

* [Notas de la versión de Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/es/docs/commerce/cloud-service/release-notes)
* [Referencia de API de REST de SaaS](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [Guía de autenticación de usuario](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)

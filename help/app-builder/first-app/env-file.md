---
title: El archivo .env
description: Obtenga información sobre los tipos de archivos del archivo .env para esta aplicación de ejemplo
landing-page-description: Obtenga información sobre Adobe Developer App Builder utilizado con Adobe Commerce y los tipos de contenido que se utilizan en el archivo .env
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Generar y configurar el archivo .env {#env-file}

La variable `.env` es un archivo especial que no forma parte del módulo de muestra, pero que es importante para su uso en la aplicación de Adobe Developer App Builder. Este archivo contiene secretos y otra información. Evite enviar este archivo a cualquier repositorio de código.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce con experiencia limitada que utilizan Adobe App Builder que desean obtener más información sobre `.env` archivo.

## Contenido del vídeo

* Introducción al archivo .env y su finalidad
* Cómo generar el archivo .env
* Cómo anexar el archivo para añadir nuevos secretos
* Evite cometer este archivo porque contiene información confidencial

>[!VIDEO](https://video.tv.adobe.com/v/3416593?quality=12&learn=on)

## Ejemplo de código

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

Puede ver estos valores estáticos que se utilizan en el módulo de muestra en el archivo `actions/commerce.index.js`.

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}

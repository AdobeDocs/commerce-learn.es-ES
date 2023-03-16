---
title: El archivo app.config.yaml
description: Obtenga información sobre los tipos de archivos del archivo app.config.yaml para esta aplicación de ejemplo.
landing-page-description: Obtenga información sobre Adobe Developer App Builder utilizado con Adobe Commerce y los tipos de archivos que aparecen en app.config.yaml.
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: 037a7571c87f328dde0f39dd830c7379bd2230b6
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---


# La variable `app.config.yaml` file {#app-config-yaml}

Este archivo determina la configuración de la aplicación.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce con experiencia limitada con el Creador de aplicaciones de Adobe que están aprendiendo sobre el `app.config.yaml` en la aplicación de ejemplo.

## Contenido del vídeo

* La variable `app.config.yaml` archivo discutido
* ¿Cómo se vinculan las definiciones a otras `.js` files

>[!VIDEO](https://video.tv.adobe.com/v/3416592)

## Ejemplo de código

```bash
# Specify your secrets here
# This file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can also include commerce OAuth credentials, our sample module will use the following example credentials:
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

Puede ver estos valores estáticos que se utilizan en el módulo de muestra en el archivo `actions/commerce.index.js`

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

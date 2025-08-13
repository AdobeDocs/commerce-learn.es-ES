---
title: Cómo consultar los datos
description: Obtenga información sobre cómo consultar datos de una instancia de Adobe Commerce Optimizer.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Query data Adobe Commerce Optimizer

Obtenga información sobre cómo consultar datos mediante GraphQL en una instancia de Adobe Commerce Optimizer.

## ¿Para quién es este vídeo?

* Arquitecto de soluciones de Commerce y desarrolladores

## Contenido de vídeo

* Consulta de datos mediante GraphQL
* Usar jq para facilitar la lectura de json

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

## Ejemplos de código

Asegúrese de intercambiar valores como `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-source-locale}}` y `{{your-search-query-string}}` con los valores necesarios en la consulta.

Consulta de muestra básica

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Consulta de muestra básica con `jq` para imprimir el resultado con sangría

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## Contenido relacionado

* [Introducción a la API de comercialización](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] Guía](https://experienceleague.adobe.com/es/docs/commerce/optimizer/overview){target="_blank"}

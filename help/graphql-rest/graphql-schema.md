---
title: Lenguaje de esquemas con GraphQL
description: Obtenga información sobre el esquema relacionado con GraphQL. Lea una descripción del esquema, junto con algunos patrones interesantes y formas de leer el esquema.
landing-page-description: Esta es una introducción a GraphQL. Explicación del esquema y cómo interpretar algunos de los elementos
short-description: Esta es una introducción a GraphQL. Explicación del esquema y cómo interpretar algunos de los elementos
kt: 13939
doc-type: video
duration: 363
audience: all
last-substantial-update: 2023-10-12T00:00:00.000Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
TQID: 'https://experienceleague.adobe.com/P9gopqc6BD8qZJlc1-gvVem4Gq3fqpX8AsRvglLAQYI'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
subfeature_v2:
  - id: e396cff5-f586-484c-89f0-7f1da3308f92
    internal-label: GraphQL
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%
---
# Idioma del esquema

Esta es la parte 4 de la serie para GraphQL y Adobe Commerce. Las consultas y mutaciones utilizadas dependen de un gráfico de datos específico que se implementa en el servidor, que el tiempo de ejecución de GraphQL consume y utiliza para resolver la consulta. La especificación de GraphQL define un lenguaje agnóstico para expresar los tipos y relaciones del gráfico de datos.

>[!VIDEO](https://video.tv.adobe.com/v/3424123?learn=on)

## Vídeos y tutoriales relacionados sobre GraphQL en esta serie

* [Parte 1 GraphQL: Introducción](../graphql-rest/intro-graphql.md)
* [Parte 2 GraphQL: Consultas](../graphql-rest/graphql-queries.md)
* [Parte 3 GraphQL - Mutaciones](../graphql-rest/graphql-mutations.md)

## Esquema de ejemplo

Este es un esquema de tipo abreviado que admite las consultas y mutaciones que ha visto hasta ahora:

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

Puede profundizar en [la documentación de GraphQL](https://graphql.org/learn/schema/){target="_blank"} para obtener más información sobre los detalles del sistema de tipos, incluida la sintaxis de algunos conceptos que no se representan aquí. Sin embargo, el ejemplo anterior se explica por sí mismo. (Tenga en cuenta también lo similar que es la sintaxis a la sintaxis de consulta). La definición de un esquema de GraphQL es simplemente una cuestión de expresar los argumentos y campos disponibles de un tipo determinado, junto con los tipos de esos campos. Cada tipo de campo complejo debe tener una definición, y así sucesivamente, a través del árbol, hasta que llegue a tipos escalares simples como `String`.

La declaración `input` es similar en todos los aspectos a `type`, pero define un tipo que se puede utilizar como entrada para un argumento. Observe también la declaración `interface`. Esto sirve a una función más o menos igual que las interfaces en PHP. Otros tipos heredan de esta interfaz.

La sintaxis `[CartItemInput!]!` parece complicada, pero al final es bastante intuitiva. El `!` _dentro_ del corchete declara que cada valor de la matriz no debe ser nulo, mientras que el _fuera_ declara que el valor de la matriz en sí debe ser no nulo (por ejemplo, una matriz vacía).

>[!NOTE]
>
>La lógica de cómo se recuperan los datos y se les da formato según un esquema, y cómo se asigna esa lógica a tipos particulares, depende de la implementación en tiempo de ejecución de GraphQL. Sin embargo, las implementaciones deben seguir un flujo conceptual que tenga sentido a la luz de la comprensión de los campos anidados: se realiza una operación de resolución asociada al tipo raíz `Query` o `Mutation`, que examina cada campo especificado en la solicitud. Para cada campo que se resuelve en un tipo complejo, se realiza una resolución similar para ese tipo, y así sucesivamente, hasta que todo se haya resuelto en valores escalares.

{{$include /help/_includes/graphql-rest-related-links.md}}

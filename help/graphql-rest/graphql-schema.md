---
title: Lenguaje de esquemas con GraphQL
description: Obtenga información sobre el esquema relacionado con GraphQL. Lea una descripción del esquema, junto con algunos patrones interesantes y formas de leer el esquema.
landing-page-description: Esta es una introducción a GraphQL. Explicación del esquema y cómo interpretar algunos de los elementos
short-description: Esta es una introducción a GraphQL. Explicación del esquema y cómo interpretar algunos de los elementos
kt: 13939
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '429'
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

El `input` declaración es en todos los aspectos como una `type` pero define un tipo que se puede utilizar como entrada para un argumento. También tenga en cuenta lo siguiente `interface` declaración. Esto sirve a una función más o menos igual que las interfaces en PHP. Otros tipos heredan de esta interfaz.

La sintaxis `[CartItemInput!]!` parece complicado, pero al final es bastante intuitivo. El `!` _interior_ el corchete declara que cada valor de la matriz no debe ser nulo, mientras que el de _exterior_ declara que el propio valor de matriz no debe ser nulo (por ejemplo, una matriz vacía).

>[!NOTE]
>
>La lógica de cómo se recuperan los datos y se les da formato según un esquema, y cómo se asigna esa lógica a tipos particulares, depende de la implementación en tiempo de ejecución de GraphQL. Sin embargo, las implementaciones deben seguir un flujo conceptual que tenga sentido a la luz de una comprensión de los campos anidados: Una operación de resolución asociada a la raíz `Query` o `Mutation` type, que examina cada campo especificado en la solicitud. Para cada campo que se resuelve en un tipo complejo, se realiza una resolución similar para ese tipo, y así sucesivamente, hasta que todo se haya resuelto en valores escalares.

{{$include /help/_includes/graphql-rest-related-links.md}}

---
title: Lenguaje del esquema con GraphQL
description: Obtenga información sobre el esquema involucrado en GraphQL. Lea una descripción del esquema, junto con algunos patrones interesantes y maneras de leerlo.
landing-page-description: Esta es una introducción a GraphQL. Explicación del esquema y cómo interpretar algunos de los elementos
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 0fa7ba038f542172c47bea859f8712759fcc52f7
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Idioma del esquema

Las consultas y mutaciones utilizadas dependen de la implementación de un gráfico de datos específico en el servidor, que el tiempo de ejecución de GraphQL consume y utiliza para resolver la consulta. La especificación de GraphQL define un lenguaje agnóstico para expresar los tipos y las relaciones del gráfico de datos.

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

Puede profundizar en [la documentación de GraphQL](https://graphql.org/learn/schema/){target="_blank"} para obtener más información sobre los detalles del sistema de tipo , incluida la sintaxis para algunos conceptos no representados aquí. Sin embargo, el ejemplo anterior es autoexplicativo. (Tenga en cuenta también que la sintaxis es similar a la sintaxis de consulta). La definición de un esquema de GraphQL es simplemente una cuestión de expresar los argumentos y campos disponibles de un tipo determinado, junto con los tipos de esos campos. Cada tipo de campo complejo debe tener una definición, etc., a través del árbol, hasta que llegue a tipos escalares simples como `String`.

La variable `input` la declaración es, en todos los aspectos, como una `type` pero define un tipo que puede utilizarse como entrada para un argumento. Tenga en cuenta también que `interface` declaración. Esto sirve a una función más o menos igual que las interfaces en PHP. Otros tipos heredan de esta interfaz.

La sintaxis `[CartItemInput!]!` parece complicado, pero al final es bastante intuitivo. La variable `!` _interior_ el corchete declara que todos los valores de la matriz deben ser no nulos, mientras que el valor de _outside_ declara que el valor de la matriz en sí no debe ser nulo (por ejemplo, una matriz vacía).

>[!NOTE]
>
>La lógica de cómo se recuperan los datos y el formato según un esquema y cómo se asigna dicha lógica a tipos concretos depende de la implementación del tiempo de ejecución de GraphQL. Sin embargo, las implementaciones deben seguir un flujo conceptual que tenga sentido a la luz de un entendimiento en torno a los campos anidados: Una operación de resolución asociada con la raíz `Query` o `Mutation` se realiza, lo que examina cada campo especificado en la solicitud. Para cada campo que se resuelve en un tipo complejo, se realiza una resolución similar para ese tipo, etc., hasta que todo se haya resuelto en valores escalares.

{{$include /help/_includes/graphql-rest-related-links.md}}

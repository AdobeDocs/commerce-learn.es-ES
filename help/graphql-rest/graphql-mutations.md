---
title: Realización de una mutación con GraphQL
description: Obtenga una introducción sobre cómo realizar una mutación con GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Realice su primera mutación utilizando llamadas al POST.
landing-page-description: Obtenga una introducción sobre cómo realizar una mutación con GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Realice su primera mutación utilizando llamadas al POST.
short-description: Obtenga una introducción sobre cómo realizar una mutación con GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Realice su primera mutación utilizando llamadas al POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Mutaciones

Cualquier especificación de API completa debe ofrecer la capacidad no solo de consultar datos, sino también de crearlos y actualizarlos.

REST distingue entre solicitudes que cambian datos y aquellas que no lo hacen con el tipo de solicitud o &quot;verbo&quot; (GET vs. POST o PUT).
Al utilizar GraphQL, las consultas de modificación de datos se distinguen por la variable `mutation` palabra clave que corresponde a un tipo raíz diferente en el esquema definido en el servidor.

Observe este ejemplo de mutación para agregar un producto al carro de compras de un usuario. (Esto requiere un ID de carro de compras generado para la sesión del cliente que inició sesión o que usa el `createEmptyCart` mutación.)

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

Puede imaginar que la mutación anterior se envíe en una solicitud junto con el siguiente diccionario de variables:

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

Y, finalmente, podría recibir una respuesta como esta:

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

Lo más importante a tener en cuenta sobre el ejemplo anterior es que, aparte del uso de la `mutation` palabra clave en lugar de `query`, la sintaxis es idéntica a una consulta. Al igual que las consultas, la mutación incluye:

* Un nombre de operación arbitrario (`doAddToCart`)
* Una lista de variables (por ejemplo, `$cartId`)
* Un campo inicial (`addProductsToCart`) con argumentos (por ejemplo, `cartId`, establezca en el valor de `$cartId`) entre paréntesis
* Una subselección de campos entre llaves

La subselección de campos permite definir de forma flexible los campos que desea devolver (desde el tipo asignado como valor devuelto de `addProductsToCart` - `AddProductsToCartOutput`) una vez completada la mutación.

Como se explicó anteriormente, los campos definidos en un esquema de GraphQL comienzan en un tipo raíz para consultas (generalmente denominado `Query`). Del mismo modo, existe otro tipo de raíz para las mutaciones (generalmente denominado `Mutation`). `addProductsToCart` es un campo de ese tipo raíz.

Algunas otras notas sobre el ejemplo anterior:

* El `!` sufijo de caracteres `String` y `CartItemInput` indica que la variable es obligatoria.
* Los corchetes (`[]`) alrededor del `CartItemInput` tipo especificado para `$cartItems` indicar una lista de ese tipo en lugar de un valor único.

{{$include /help/_includes/graphql-rest-related-links.md}}

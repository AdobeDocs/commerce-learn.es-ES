---
title: Realización de una consulta con GraphQL
description: Obtenga información sobre cómo realizar una consulta con GraphQL en Adobe Commerce y  [!DNL Magento Open Source]. Introducción a GraphQL mediante llamadas de GET y POST.
landing-page-description: Obtenga información sobre cómo realizar una consulta con GraphQL en Adobe Commerce y  [!DNL Magento Open Source]. Introducción a GraphQL mediante llamadas de GET y POST.
short-description: Obtenga información sobre cómo realizar una consulta con GraphQL en Adobe Commerce y  [!DNL Magento Open Source]. Introducción a GraphQL mediante llamadas de GET y POST.
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 0%

---

# Consultas de GraphQL

Esta es la parte 2 de la serie para GraphQL y Adobe Commerce. En este tutorial y vídeo, aprenderá acerca de las consultas de GraphQL y cómo realizarlas con Adobe Commerce.

>[!VIDEO](https://video.tv.adobe.com/v/3450061?learn=on&captions=spa)

## Vídeos y tutoriales relacionados sobre GraphQL en esta serie

* [Parte 1 GraphQL: Introducción](../graphql-rest/intro-graphql.md)
* [Parte 3 GraphQL - Mutaciones](../graphql-rest/graphql-mutations.md)
* [Parte 4 GraphQL - Esquema](../graphql-rest/graphql-schema.md)

## Ejemplo de sintaxis de GraphQL

Vamos a profundizar directamente en la sintaxis de consultas de GraphQL con un ejemplo completo. (Recuerde, puede probar esto usted mismo en https://venia.magento.com/graphql.)

Observe la siguiente consulta de GraphQL, que se desglosa pieza a pieza:

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

Una respuesta plausible de un servidor de GraphQL para la consulta anterior podría ser:

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

El ejemplo anterior se basa en el esquema de GraphQL predeterminado para Adobe Commerce, definido en el servidor. En esta solicitud, usted
consultar varios tipos de datos a la vez. La consulta expresa exactamente los campos que desea y los datos devueltos tienen formato
de forma similar a la propia consulta.

>[!NOTE]
>
>Los clientes de GraphQL ofuscan la forma de la solicitud HTTP real que se envía, pero esto es fácil de descubrir. Si utiliza un cliente basado en explorador, observe la ficha [!UICONTROL Network] cuando se envíe una consulta. Verá que la solicitud contiene un cuerpo sin procesar formado por &quot;consulta: `{string}`&quot;, donde `{string}` es simplemente la cadena sin procesar de toda la consulta. Si la solicitud se envía como una GET, esta podría codificarse en el parámetro de cadena de consulta &quot;query&quot; en su lugar. A diferencia de REST, el tipo de solicitud HTTP no importa, solo el contenido de la consulta.


## Consulta de lo que desea

`country` y `categories` en el ejemplo representan dos &quot;consultas&quot; diferentes para dos tipos diferentes de datos. A diferencia de un paradigma de API tradicional como REST, que definiría puntos de conexión separados y explícitos para cada tipo de datos. GraphQL le proporciona la flexibilidad para consultar un único extremo con una expresión que puede recuperar muchos tipos de datos a la vez.

De la misma manera, la consulta especifica exactamente los campos deseados para `country` (`id` y `full_name_english`) y `categories` (`items`, que tiene una subselección de campos), y los datos que recibe reflejan esa especificación de campo. Es de suponer que hay muchos más campos disponibles para estos tipos de datos, pero solo se obtiene lo que se ha solicitado.


>[!NOTE]
>
>Puede observar que el valor devuelto para `items` es en realidad una _matriz_ de valores, pero está seleccionando directamente subcampos para él. Cuando el tipo de un campo es una lista, GraphQL entiende implícitamente las subselecciones que se deben aplicar a cada elemento de la lista.

## Argumentos

Aunque los campos que desea que se devuelvan se especifican entre llaves de cada tipo, los argumentos con nombre y los valores correspondientes se especifican entre paréntesis después del nombre del tipo. Los argumentos suelen ser opcionales y a menudo afectan a la forma en que se filtran, formatean o transforman los resultados de la consulta.

Va a pasar un argumento `id` a `country`, especificando el país en particular que se va a consultar, y un argumento `filters` para `categories`.

## Campos hasta abajo

Aunque `country` y `categories` suelen considerarse como consultas o entidades independientes, todo el árbol expresado en la consulta consiste únicamente en campos. La expresión de `products` sintácticamente no es diferente de la de `categories`. Ambos son campos, y no hay diferencia entre su construcción.

Cualquier gráfico de datos de GraphQL tiene un solo tipo &quot;raíz&quot; (generalmente denominado `Query`) para iniciar el árbol y los tipos que a menudo se consideran entidades se asignan a los campos de esta raíz. La consulta de ejemplo está realizando una consulta genérica para el tipo raíz y seleccionando los campos `country` y `categories`. A continuación, se seleccionan subcampos de esos campos, y así sucesivamente, potencialmente varios niveles de profundidad. Siempre que el tipo de valor devuelto de un campo sea un tipo complejo (por ejemplo, uno con sus propios campos, en lugar de un tipo escalar), siga seleccionando los campos que desee.

Este concepto de campos anidados también es la razón por la que puede pasar argumentos para `products` (`pageSize` y `currentPage`) del mismo modo que lo hizo para el campo `categories` de nivel superior.

![Árbol de campos de GraphQL](../assets/graphql-field-tree.png)

## Variables

Vamos a probar una consulta diferente:

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

Lo primero que hay que tener en cuenta es que se agregó la palabra clave `query` antes de la llave de apertura de la consulta, junto con un nombre de operación (`getProducts`). Este nombre de operación es arbitrario; no corresponde a nada en el esquema del servidor. Esta sintaxis se ha agregado para admitir la introducción de variables.

En la consulta anterior, se codificaban directamente los valores de los argumentos de los campos, como cadenas o números enteros. Sin embargo, la especificación de GraphQL es compatible de primera clase para separar los datos introducidos por el usuario de la consulta principal mediante variables.

En la nueva consulta, se utilizan paréntesis antes de la llave de apertura de toda la consulta para definir una variable `$search` (las variables siempre utilizan la sintaxis de prefijo de signo de dólar). Esta variable se proporciona al argumento `search` de `products`.

Cuando una consulta contiene variables, se espera que la solicitud de GraphQL incluya un diccionario de valores independiente con codificación JSON junto a la propia consulta. Para la consulta anterior, puede enviar el siguiente JSON de valores de variables además del cuerpo de la consulta:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Si está probando estas consultas en el sitio de ejemplo de Venia en lugar de en su propia instancia de Adobe Commerce, es probable que los resultados devueltos estén vacíos para `related_products`.

En cualquier cliente compatible con GraphQL que utilice para realizar pruebas (como Altair y GraphiQL), la interfaz de usuario admite la introducción de las variables JSON de forma independiente de la consulta.

Tal como ha visto que la solicitud HTTP real para una consulta GraphQL contiene &quot;query: `{string}`&quot; en su cuerpo, cualquier solicitud que contenga un diccionario de variables simplemente incluye una &quot;variables: `{json}`&quot; adicional en ese mismo cuerpo, donde `{json}` es la cadena JSON con los valores de las variables.

La nueva consulta también usa un _fragmento_ (`productDetails`) para reutilizar la misma selección de campo en varios lugares. [Obtenga más información acerca de los fragmentos](https://graphql.org/learn/queries/#fragments){target="_blank"} en la documentación de GraphQL.

{{$include /help/_includes/graphql-rest-related-links.md}}

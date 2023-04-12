---
title: Realizar una consulta mediante GraphQL
description: Obtenga información sobre cómo realizar una consulta mediante GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Esta es una introducción a GraphQL mediante llamadas de GET y POST.
landing-page-description: Obtenga información sobre cómo realizar una consulta mediante GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Esta es una introducción a GraphQL mediante llamadas de GET y POST.
short-description: Obtenga información sobre cómo realizar una consulta mediante GraphQL en Adobe Commerce y [!DNL Magento Open Source]. Esta es una introducción a GraphQL mediante llamadas de GET y POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# Consultas de GraphQL

Veamos directamente la sintaxis de las consultas de GraphQL con un ejemplo completo. (Recuerde, puede probarlo usted mismo contra https://venia.magento.com/graphql).

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

Una respuesta posible de un servidor de GraphQL para la consulta anterior podría ser:

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

El ejemplo anterior se basa en el esquema predeterminado de GraphQL para Adobe Commerce, definido en el servidor. En esta solicitud, se consulta varios tipos de datos a la vez. La consulta expresa exactamente los campos que desea y los datos devueltos tienen un formato similar al de la propia consulta.

>[!NOTE]
>
>Los clientes de GraphQL confunden la forma en que se envía la solicitud HTTP real, pero esto es fácil de descubrir. Si utiliza un cliente basado en explorador, observe la [!UICONTROL Network] cuando se envía una consulta. Verá que la solicitud contiene un cuerpo sin procesar que consiste en &quot;consulta: `{string}`&quot;, donde `{string}` es simplemente la cadena sin procesar de toda la consulta. Si la solicitud se envía como GET, la consulta puede estar codificada en el parámetro de cadena de consulta &quot;query&quot; en su lugar. A diferencia de REST, el tipo de solicitud HTTP no importa, solo el contenido de la consulta.


## Consulta de qué desea

`country` y `categories` en el ejemplo representan dos &quot;consultas&quot; diferentes para dos tipos diferentes de datos. A diferencia de un paradigma de API tradicional como REST, que definiría puntos de conexión separados y explícitos para cada tipo de datos. GraphQL le ofrece la flexibilidad de consultar un único extremo con una expresión que pueda recuperar muchos tipos de datos a la vez.

Del mismo modo, la consulta especifica exactamente los campos que se desean para ambos `country` (`id` y `full_name_english`) y `categories` (`items`, que tiene una subselección de campos), y los datos que recibe reflejan esa especificación de campo. Probablemente haya muchos campos más disponibles para estos tipos de datos, pero solo recuperará lo que solicitó.


>[!NOTE]
>
>Puede observar que el valor devuelto de `items` es en realidad un _matriz_ de valores, pero se están seleccionando subcampos directamente para él. Cuando el tipo de campo es una lista, GraphQL entiende implícitamente las subselecciones que se deben aplicar a cada elemento de la lista.

## Argumentos

Mientras que los campos que desea que se devuelvan se especifican entre las llaves de cada tipo, los argumentos y valores asignados para ellos se especifican entre paréntesis después del nombre del tipo. Los argumentos suelen ser opcionales y a menudo afectan a la forma en que se filtran, formatean o transforman los resultados de la consulta.

Está pasando un `id` argumento a `country`, especificando el país concreto que se va a consultar, y `filters` argumento para `categories`.

## Campos hasta abajo

Mientras que puede que considere `country` y `categories` como consultas o entidades independientes, todo el árbol expresado en la consulta consiste únicamente en campos. La expresión de `products` no es sintácticamente diferente a la de `categories`. Ambos son campos, y no hay diferencia entre su construcción.

Cualquier gráfico de datos de GraphQL tiene un solo tipo &quot;raíz&quot; (generalmente se hace referencia a `Query`) para iniciar el árbol, y los tipos que se consideran entidades se asignan a los campos de esta raíz. La consulta de ejemplo está realizando una consulta genérica para el tipo raíz y seleccionando los campos `country` y `categories`. A continuación, se seleccionan subcampos de esos campos, etc., que pueden tener varios niveles de profundidad. Siempre que el tipo devuelto de un campo sea un tipo complejo (por ejemplo, uno con sus propios campos, en lugar de un tipo escalar), continúe seleccionando los campos que desee.

Este concepto de campos anidados también explica por qué se pueden pasar argumentos para `products` (`pageSize` y `currentPage`) de la misma manera que lo hizo para el nivel superior `categories` campo .

![Árbol de campos de GraphQL](../assets/graphql-field-tree.png)

## Variables

Probemos una consulta diferente:

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

Lo primero que hay que tener en cuenta es que se ha añadido la palabra clave `query` antes de la llave de apertura de la consulta, junto con un nombre de operación (`getProducts`). Este nombre de operación es arbitrario; no se corresponde con nada en el esquema del servidor. Esta sintaxis se agregó para admitir la introducción de variables.

En la consulta anterior, se han codificado los valores para los argumentos de los campos directamente, como cadenas o enteros. Sin embargo, la especificación de GraphQL es compatible de primera clase con la separación de los datos introducidos por el usuario de la consulta principal mediante variables.

En la nueva consulta, se utilizan paréntesis antes de la llave de apertura de toda la consulta para definir una `$search` (las variables siempre usan la sintaxis del prefijo del signo de dólar). Es esta variable la que se proporciona al `search` argumento para `products`.

Cuando una consulta contiene variables, se espera que la solicitud de GraphQL incluya un diccionario de valores con codificación JSON independiente junto a la propia consulta. Para la consulta anterior, puede enviar el siguiente JSON de valores de variables además del cuerpo de la consulta:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Si está probando estas consultas con el sitio de ejemplo de Venia en lugar de con su propia instancia de Adobe Commerce, es probable que los resultados devueltos estén vacíos durante `related_products`.

En cualquier cliente compatible con GraphQL que utilice para realizar pruebas (como Altair y GraphiQL), la interfaz de usuario permite introducir las variables JSON de forma independiente de la consulta.

Tal como vio que la solicitud HTTP real para una consulta de GraphQL contiene &quot;consulta: `{string}`&quot; en su cuerpo, cualquier solicitud que contenga un diccionario de variables simplemente incluye &quot;variables&quot; adicionales: `{json}`&quot; en ese mismo cuerpo, donde `{json}` es la cadena JSON con los valores de las variables.

La nueva consulta también utiliza un _fragmento_ (`productDetails`) para reutilizar la misma selección de campos en varios lugares. [Más información sobre fragmentos](https://graphql.org/learn/queries/#fragments){target="_blank"} en la documentación de GraphQL.

{{$include /help/_includes/graphql-rest-related-links.md}}

---
title: El estado del inventario comprueba las consideraciones de desarrollo y rendimiento
description: Conozca algunas ideas y consideraciones para realizar comprobaciones del estado del inventario para Adobe Commerce.
feature: Best Practices, Inventory
topic: Development, Performance
old-role: Architect, Developer
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 498
last-substantial-update: 2024-05-09T00:00:00.000Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1982
ht-degree: 0%

---

# El estado del inventario comprueba las consideraciones de desarrollo y rendimiento

La precisión con el inventario es una consideración muy importante. Existen algunas funciones nativas que pueden ayudar a garantizar que este riesgo sea lo más bajo posible, como los pedidos pendientes y la configuración del umbral de falta de existencias. Ambos temas se pueden leer en [Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) para obtener más información.

Hay proyectos y casos de uso en los que se solicitan comprobaciones del estado del inventario en tiempo real para una tienda Adobe Commerce. Este tutorial proporciona insight para gestionar esta conversación con consideraciones de desarrollo y rendimiento.

## Validar si esta solicitud es absolutamente necesaria

Prepárese para interactuar con la conversación con la mayor cantidad de información posible. Lo más importante que debe hacer es comprobar que la funcionalidad nativa no es aceptable para este proyecto. Descubra el motivo de esta solicitud para validar que las capacidades nativas de Adobe Commerce no cumplen con esta solicitud.

Otra consideración es el coste para desarrollar, probar y mantener esta función. El hecho de que alguna parte interesada lo considere necesario no significa que sea un requisito. Existen costes asociados con la validación del inventario fuera de la funcionalidad principal de Adobe Commerce. Estos costes se deben a deudas técnicas, más pruebas y validación, así como documentación de uso y documentos de apoyo para su arquitectura.

## Determinar cuál es una cadencia de actualización de inventario aceptable

Intente considerar las comprobaciones de inventario y cómo se logra en tres enfoques. Cada uno tiene beneficios y limitaciones. También aumentan en complejidad y requieren más pruebas y reflexión para la gestión de errores. Recuerde que cuando decide seguir una ruta personalizada, hay responsabilidades y consideraciones añadidas. Algunos ejemplos incluyen un proceso de reserva, monitorización, pruebas y solución de problemas que corresponden al equipo de desarrollo. Algunos elementos buenos que se deben incluir son nueva documentación de soporte, formación y monitorización para garantizar que el equipo de desarrollo pueda admitir toda la función. Otro efecto secundario es que el equipo de desarrollo es el propietario total del proceso y ya no utiliza la funcionalidad nativa proporcionada por la aplicación principal de Adobe Commerce. El soporte de Adobe no puede ayudar con este nivel de personalización.

El primer método es utilizar la funcionalidad nativa. El uso de la funcionalidad nativa es la menor cantidad de riesgo y tiene muchas ventajas. Este método significa que puede confiar en toda la documentación y los tutoriales existentes que Adobe Commerce proporciona para el uso de la función. La gestión del inventario tiene muchos aspectos, por lo que el uso de lo que viene con la aplicación debería ser la primera consideración. Sin embargo, hay casos de uso en los que los datos encontrados en el comercio en el momento del pedido pueden no ser completamente precisos. Un ejemplo de cómo las cosas pueden no estar sincronizadas es que las ventas se permiten fuera de la aplicación de Adobe Commerce directamente en el sistema de gestión de pedidos. Un motivo es que, para garantizar que los niveles de inventario exactos se representen en Adobe Commerce, sería necesario algún tipo de integración para mantener la información de Adobe Commerce lo más cercana posible a la precisión. Si la sobreventa no es aceptable, entonces añadir un umbral de existencias es un buen método para detener la venta de artículos antes de que llegue a cero. La funcionalidad de sincronización nativa para Adobe Commerce se establece como máximo una vez al día. Esto es suficiente para algunos casos de uso, pero puede no ser lo suficientemente frecuente para otros. Lea [Importación y exportación programadas](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) para obtener información más detallada.

El segundo enfoque sería `near real-time`. Casi en tiempo real sigue utilizando la funcionalidad nativa. Sin embargo, esto incluye algo de trabajo adicional para proporcionar una integración que alimente al comercio con frecuencia para actualizar su inventario según un programa. Por ejemplo, cada hora. Esta opción necesita un poco de reflexión sobre cómo puede funcionar una integración, pero el uso de la &quot;api masiva&quot; y el hecho de que algún middleware haga la transformación de los datos y los lleve al comercio es un buen enfoque. Considere la posibilidad de utilizar Adobe App Builder o plataformas similares para realizar la mayor parte del trabajo e insertar la información en Adobe Commerce en una cadencia más frecuente.

El tercer enfoque y el más complejo con la mayor cantidad de riesgo y responsabilidad son las comprobaciones de inventario en tiempo real a una API externa o fuente de datos. Realizar una comprobación de inventario en tiempo real en un sistema externo es arriesgado y tiene otros elementos que deben tenerse en cuenta. A continuación se muestra un pequeño conjunto de otras cosas que deben evaluarse:

* ¿Puede el sistema externo aceptar solicitudes de REST o GraphQL?
* ¿Hay algún límite en el extremo, como un número X de solicitudes por minuto, que pueda no coincidir con el tráfico del sitio web?
* Qué sucede con el tiempo de respuesta bajo carga
* ¿Qué sucede cuando los tiempos de respuesta son largos? ¿Se finaliza esto automáticamente y se utiliza una opción de reserva como el inventario nativo?
* What type of monitoring is available to ensure that API requests are within the tolerance limits

## Consideraciones al considerar la administración de inventario no nativo

Keep the customizations as non-complex as possible.
¿Qué tan plana puede ser la organización del inventario? ¿Es solo 1 SKU y la cantidad total de stock disponible? ¿O hay otros atributos que deben tenerse en cuenta?

Si la información de inventario es bastante plana, por ejemplo, un SKU y la cantidad total disponible, las opciones de tiempo casi real se amplían. El concepto de casi en tiempo real significa que hay una operación en segundo plano que recopila el inventario del origen y, a continuación, rellena un motor de almacenamiento para utilizarlo para responder a la solicitud. Para ello, puede utilizar elementos como Redis, Mongo u otras bases de datos no relacionales. Estas opciones son buenas porque son muy rápidas y funcionan muy bien para pares clave/valor. Si los datos son un poco más complejos, es probable que sea necesario utilizar una base de datos de relaciones, ya sea dentro o fuera de la aplicación de comercio. By offloading this from the commerce database, you keep the core commerce application isolated from these transactions. Otro conjunto de ventajas es el ahorro de E/S de la aplicación de comercio, CPU, RAM y otros de uso. En lugar de utilizar los recursos de los servidores de aplicaciones de Adobe Commerce, aproveche las nuevas API para extraer los datos del almacenamiento fuera del sitio.  Es probable que esto necesite un middleware para ayudar a transformar cualquier dato. A continuación, asegúrese de que la aplicación que realiza la llamada puede obtener el resultado esperado. Al utilizar Adobe App Builder con API mesh, los datos se pueden transformar y devolver con el formato correcto.

El uso de Adobe App Builder con API mesh también es una buena opción cuando hay varias fuentes de inventario.


## Mover la lógica de ejecución a una ubicación fuera de proceso

Adobe Developer App Builder provides a unified third-party extensibility framework for integrating and creating custom experiences to extend Adobe solutions. Adobe Commerce puede utilizar Adobe Developer App Builder. Este sería un excelente caso de uso para ampliar alguna funcionalidad que normalmente se produce en la aplicación principal y la mueve fuera del sitio. Al eliminar la funcionalidad de la aplicación de Commerce, se reduce el número de módulos y la complejidad de la aplicación de Commerce. In turn, lower numbers of in-process customizations reduce the complexity for upgrading and maintenance.

Para inspirarse en cómo se puede lograr esto, el equipo de Adobe ha creado documentación que puede ser una buena fuente de inspiración y proporcionar ejemplos de código de trabajo. When a shopper adds a product to the cart, a third-party inventory management system checks whether the item is in stock. If it is, allow the product to be added. Otherwise, display an error message.  For code samples and further information go to [Webhook Use Cases](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## When to do inventory checks

When to check if inventory is still available will eventually be up to the business stake holder, the software architect with some input from other key stakeholders. A few appropriate times would include when adding an item to the cart and when entering the checkout workflow. Any other events will simply add load to the backend systems when it may not be necessary. Keep in mind that the goal is to catch an inventory issue only when it is paramount. Other checks may be nice to have but if they may impact the overall goal for inventory status checks, they should be carefully considered and only allowed if the business stakeholders or others are aware of the potential risk for extra load on the backend systems.

## Research how the inventory source

Comprehensive investigation of the external inventory source is required. Items that should be evaluated are available API options, support for GraphQL, and expected response times. If the inventory source has a limited connection bandwidth or was never intended to be used in a real time request, the ability for use is excluded and the architect needs to consider near-real-time instead.  If the API request times exceed the defined parameters it will also exclude this from being a viable option.  An example of this would be api responses are 200MS® for one a time requests, but rise to 500 to 900MS® under moderate load.  This would likely just get worse with more load and rules out live inventory calls from being available.

Be sure to test the api response times with simple requests as well as with a high volume similar to the expected traffic on the live website. Remember to test all areas from commerce at the same time to simulate real world scenarios. If the expectation is a live inventory call should occur on product pages, when viewing and editing a cart and during checkout, the load testing needs to simulate all of these at the same time to mimic real customer behavior and the expectation for a store.

## Fallback options

IF the inventory source is down and monitoring is available, using the native capability of Adobe Commerce is recommended. However, with proper monitoring the customer experience can dynamically change to reflect the loss of real time inventory checks. This may mean that a sale or event is canceled early or removed from display to avoid overselling. The expectation for what to do when the inventory source is down for any reason needs to be considered and discussed with the store owner so everyone is aware of any automatic process that takes over in that unfortunate circumstance.

## Conclusión

La decisión de realizar comprobaciones de inventario en tiempo real es importante. Garantizar que el propietario del sitio web, el equipo de desarrollo y otros estén completamente formados y sean conscientes de todos los beneficios y posibles escollos recae en el desarrollador principal o arquitecto. Proporcionar un plan reflexivo que cubra las razones y un proceso de reserva es clave para el éxito.

Las comprobaciones de inventario en vivo se pueden realizar, pero requieren investigación y reflexión en torno a las pruebas y la validación durante el ciclo de control de calidad. Garantizar que las pruebas de carga y las pruebas automatizadas de extremo a extremo ayudan a garantizar que todos los problemas potenciales se detecten y se clasifiquen.

Al considerar qué acciones realizar si la monitorización detecta una tendencia de llamadas fallidas al sistema externo o si los tiempos de respuesta están por encima de los umbrales aceptables, el sitio puede permanecer en línea y ofrecer la menor cantidad de irritación a los clientes. Las opciones de reserva pueden ser tan sencillas como desactivar las comprobaciones de inventario externas y utilizar la funcionalidad nativa o pueden ser tan avanzadas como deshabilitar una promoción, notificar al equipo de desarrollo de los problemas de escalado o incluso quizás volver a enrutar las solicitudes a un segundo o tercer sistema backend si está disponible. La forma en que se ejerce el mecanismo de reserva debería considerarse como la implementación real, ya que cada integración tiene problemas en algún momento. Todo lo que esté automatizado o necesite una acción manual debe estar claramente documentado.

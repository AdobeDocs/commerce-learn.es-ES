---
title: El estado del inventario comprueba las consideraciones de desarrollo y rendimiento
description: Conozca algunas ideas y consideraciones para realizar comprobaciones del estado del inventario para Adobe Commerce.
feature: Best Practices, Inventory
topic: Development, Performance
old-role: Architect, Developer
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1932'
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
* Qué tipo de monitorización está disponible para garantizar que las solicitudes de API se encuentren dentro de los límites de tolerancia

## Consideraciones al considerar la administración de inventario no nativo

Mantenga las personalizaciones tan poco complejas como sea posible.
¿Qué tan plana puede ser la organización del inventario? ¿Es solo 1 SKU y la cantidad total de stock disponible? ¿O hay otros atributos que deben tenerse en cuenta?

Si la información de inventario es bastante plana, por ejemplo, un SKU y la cantidad total disponible, las opciones de tiempo casi real se amplían. El concepto de casi en tiempo real significa que hay una operación en segundo plano que recopila el inventario del origen y, a continuación, rellena un motor de almacenamiento para utilizarlo para responder a la solicitud. Para ello, puede utilizar elementos como Redis, Mongo u otras bases de datos no relacionales. Estas opciones son buenas porque son muy rápidas y funcionan muy bien para pares clave/valor. Si los datos son un poco más complejos, es probable que sea necesario utilizar una base de datos de relaciones, ya sea dentro o fuera de la aplicación de comercio. Al descargar esto de la base de datos de comercio, mantiene la aplicación de comercio principal aislada de estas transacciones. Otro conjunto de ventajas es el ahorro de E/S de la aplicación de comercio, CPU, RAM y otros de uso. En lugar de utilizar los recursos de los servidores de aplicaciones de Adobe Commerce, aproveche las nuevas API para extraer los datos del almacenamiento fuera del sitio.  Es probable que esto necesite un middleware para ayudar a transformar cualquier dato. A continuación, asegúrese de que la aplicación que realiza la llamada puede obtener el resultado esperado. Al utilizar Adobe App Builder con API mesh, los datos se pueden transformar y devolver con el formato correcto.

El uso de Adobe App Builder con API mesh también es una buena opción cuando hay varias fuentes de inventario.


## Mover la lógica de ejecución a una ubicación fuera de proceso

Adobe Developer App Builder proporciona un marco de trabajo de extensibilidad unificado de terceros para integrar y crear experiencias personalizadas para ampliar las soluciones de Adobe. Adobe Commerce puede utilizar Adobe Developer App Builder. Este sería un excelente caso de uso para ampliar alguna funcionalidad que normalmente se produce en la aplicación principal y la mueve fuera del sitio. Al eliminar la funcionalidad de la aplicación de Commerce, se reduce el número de módulos y la complejidad de la aplicación de Commerce. A su vez, un número menor de personalizaciones en proceso reduce la complejidad de la actualización y el mantenimiento.

Para inspirarse en cómo se puede lograr esto, el equipo de Adobe ha creado documentación que puede ser una buena fuente de inspiración y proporcionar ejemplos de código de trabajo. Cuando un comprador agrega un producto al carro de compras, un sistema de administración de inventario de terceros comprueba si el artículo está en stock. Si es así, permita que se agregue el producto. De lo contrario, mostrar un mensaje de error.  Para obtener ejemplos de código y más información, visite [Casos de uso de webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Cuándo realizar comprobaciones de inventario

Cuando se compruebe si el inventario aún está disponible, dependerá finalmente del inversor empresarial, el arquitecto de software con algunas aportaciones de otras partes interesadas clave. Algunos momentos adecuados incluirían al agregar un elemento al carro de compras y al entrar en el flujo de trabajo de cierre de compra. Cualquier otro evento simplemente añadirá carga a los sistemas back-end cuando no sea necesario. Tenga en cuenta que el objetivo es detectar un problema de inventario solo cuando es primordial. Otras comprobaciones pueden ser útiles, pero si pueden afectar al objetivo general de las comprobaciones del estado del inventario, deben considerarse cuidadosamente y solo deben permitirse si las partes interesadas empresariales u otras personas son conscientes del riesgo potencial de carga adicional en los sistemas back-end.

## Investigue cómo funciona el origen del inventario

Se requiere una investigación exhaustiva de la fuente de inventario externo. Los elementos que deben evaluarse son las opciones de API disponibles, la compatibilidad con GraphQL y los tiempos de respuesta esperados. Si el origen de inventario tiene un ancho de banda de conexión limitado o no estaba previsto que se usara en una solicitud en tiempo real, se excluye la capacidad de uso y el arquitecto debe considerar la posibilidad de utilizarlo casi en tiempo real.  Si los tiempos de solicitud de la API exceden los parámetros definidos, también se excluirá esta opción de ser viable.  Un ejemplo de esto sería que las respuestas de API son de 200 MS® para solicitudes de una por vez, pero aumentan a 500 a 900 MS® con carga moderada.  Es probable que esto empeore con más carga y descarte la disponibilidad de llamadas de inventario en directo.

Asegúrese de probar los tiempos de respuesta de la API con solicitudes sencillas, así como con un gran volumen similar al tráfico esperado en el sitio web activo. Recuerde probar todas las áreas del comercio al mismo tiempo para simular escenarios del mundo real. Si la expectativa es que se produzca una llamada de inventario activo en las páginas de productos, al ver y editar un carro de compras y durante el cierre de compra, las pruebas de carga deben simular todas al mismo tiempo para imitar el comportamiento real del cliente y las expectativas de una tienda.

## Opciones de reserva

SI el origen del inventario no está operativo y la monitorización está disponible, se recomienda utilizar la capacidad nativa de Adobe Commerce. Sin embargo, con una monitorización adecuada, la experiencia del cliente puede cambiar dinámicamente para reflejar la pérdida de comprobaciones de inventario en tiempo real. Esto puede significar que una venta o evento se cancela antes de tiempo o se elimina de la pantalla para evitar una venta excesiva. La expectativa de qué hacer cuando el origen del inventario está caído por cualquier razón debe ser considerada y discutida con el propietario de la tienda para que todos estén al tanto de cualquier proceso automático que tome el control en esa desafortunada circunstancia.

## Conclusión

La decisión de realizar comprobaciones de inventario en tiempo real es importante. Garantizar que el propietario del sitio web, el equipo de desarrollo y otros estén completamente formados y sean conscientes de todos los beneficios y posibles escollos recae en el desarrollador principal o arquitecto. Proporcionar un plan reflexivo que cubra las razones y un proceso de reserva es clave para el éxito.

Las comprobaciones de inventario en vivo se pueden realizar, pero requieren investigación y reflexión en torno a las pruebas y la validación durante el ciclo de control de calidad. Garantizar que las pruebas de carga y las pruebas automatizadas de extremo a extremo ayudan a garantizar que todos los problemas potenciales se detecten y se clasifiquen.

Al considerar qué acciones realizar si la monitorización detecta una tendencia de llamadas fallidas al sistema externo o si los tiempos de respuesta están por encima de los umbrales aceptables, el sitio puede permanecer en línea y ofrecer la menor cantidad de irritación a los clientes. Las opciones de reserva pueden ser tan sencillas como desactivar las comprobaciones de inventario externas y utilizar la funcionalidad nativa o pueden ser tan avanzadas como deshabilitar una promoción, notificar al equipo de desarrollo de los problemas de escalado o incluso quizás volver a enrutar las solicitudes a un segundo o tercer sistema backend si está disponible. La forma en que se ejerce el mecanismo de reserva debería considerarse como la implementación real, ya que cada integración tiene problemas en algún momento. Todo lo que esté automatizado o necesite una acción manual debe estar claramente documentado.

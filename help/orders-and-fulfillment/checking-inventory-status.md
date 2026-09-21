---
title: 'Comprobación del estado del inventario: desarrollo y rendimiento'
description: Aprenda a evaluar si son necesarias las comprobaciones de inventario en tiempo real en Adobe Commerce y a revisar las consideraciones de desarrollo y rendimiento de su tienda.
feature: Best Practices, Inventory
topic: Development, Performance
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 496
last-substantial-update: 2024-05-09
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
    internal-label: Order Management System
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
    internal-label: 2FA
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
    internal-label: B2B
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: fcf0f5116ba75bd618f4ed80591ecc96132cdb45
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%
---
# El estado del inventario comprueba las consideraciones de desarrollo y rendimiento

La precisión con el inventario es una consideración importante. Existen algunas funciones nativas que pueden ayudar a garantizar que este riesgo sea lo más bajo posible, como los pedidos pendientes y la configuración del umbral de falta de existencias. Ambos temas se pueden leer en [Adobe Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) para obtener más información.

Hay proyectos y casos de uso en los que se solicitan comprobaciones del estado del inventario en tiempo real para una tienda Adobe Commerce. Este tutorial proporciona insight para gestionar esta conversación con consideraciones de desarrollo y rendimiento.

## Validar si esta solicitud es necesaria

Prepárese para discutir la solicitud con la mayor cantidad de información posible. Lo más importante que debe hacer es comprobar que la funcionalidad nativa no es aceptable para este proyecto. Descubra el motivo de esta solicitud para validar que las capacidades nativas de Adobe Commerce no cumplen con esta solicitud.

Otra consideración es el coste para desarrollar, probar y mantener esta función. La opinión de una parte interesada no necesariamente exige algo. Existen costes asociados con la validación del inventario fuera de la funcionalidad principal de Adobe Commerce. Estos costes se presentan en forma de deuda técnica, más pruebas y validación, así como documentación de uso y documentos de apoyo para su arquitectura.

## Determinar cuál es una cadencia de actualización de inventario aceptable

Intente considerar las comprobaciones de inventario y cómo se logra en tres enfoques. Cada uno tiene beneficios y limitaciones. También aumentan en complejidad y requieren más pruebas y reflexión para la gestión de errores. Recuerde, cuando decide implementar una solución personalizada, hay responsabilidades y consideraciones añadidas. Algunos ejemplos son un proceso de reserva, monitorización, pruebas y solución de problemas, que corresponden al equipo de desarrollo. Algunos elementos buenos que se deben incluir son nueva documentación de soporte, formación y monitorización para garantizar que el equipo de desarrollo pueda admitir toda la función. Un efecto secundario es que el equipo de desarrollo es propietario del proceso y ya no aprovecha la funcionalidad nativa proporcionada por la aplicación principal de Adobe Commerce. El soporte de Adobe no puede ayudar con este nivel de personalización.

El primer método es utilizar la funcionalidad nativa. El uso de la funcionalidad nativa es la menor cantidad de riesgo y tiene muchas ventajas. Este método significa que puede confiar en toda la documentación y los tutoriales existentes que Adobe Commerce proporciona para el uso de la función. La administración del inventario tiene muchos aspectos, así que use lo que viene con la aplicación como primera consideración. Sin embargo, hay casos de uso en los que los datos encontrados en el comercio en el momento del pedido no son precisos. Un ejemplo de cómo los datos se desincronizan es que las ventas están permitidas fuera de la aplicación de Adobe Commerce directamente en el sistema de Order Management. Un motivo es que, para garantizar que los niveles de inventario exactos se representen en Adobe Commerce, se requiere algún tipo de integración para mantener la información de Adobe Commerce lo más cercana posible a la precisión. Si la sobreventa no es aceptable, entonces añadir un umbral de existencias es un buen método para detener la venta de artículos antes de que llegue a cero. La funcionalidad de sincronización nativa para Adobe Commerce se establece como máximo una vez al día. Esta frecuencia es suficiente para algunos casos de uso, sin embargo, no es lo suficientemente frecuente para otros. Lea [Importación y exportación programadas](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) para obtener información detallada.

El segundo enfoque es `near real-time`. Casi en tiempo real sigue utilizando la funcionalidad nativa. Sin embargo, esto incluye algo de trabajo adicional para proporcionar una integración que alimente al comercio con frecuencia para actualizar su inventario según un programa. Por ejemplo, cada hora. Esta opción requiere que se piense cómo funciona una integración, pero el uso de la &quot;api masiva&quot; y el hecho de que algún middleware haga la transformación de los datos y los lleve al comercio es un buen enfoque. Considere la posibilidad de utilizar Adobe App Builder o plataformas similares para realizar la mayor parte del trabajo e insertar la información en Adobe Commerce en una cadencia más frecuente.

El tercer enfoque y el más complejo con la mayor cantidad de riesgo y responsabilidad son las comprobaciones de inventario en tiempo real a una API externa o fuente de datos. Realizar una comprobación de inventario en tiempo real en un sistema externo es arriesgado y tiene otros elementos que deben tenerse en cuenta. A continuación se muestra un pequeño conjunto de otras cosas que deben evaluarse:

* ¿Puede el sistema externo aceptar solicitudes de REST o GraphQL?
* ¿Hay algún límite en el extremo, como un número X de solicitudes por minuto que no coinciden con el tráfico del sitio web?
* Qué sucede con el tiempo de respuesta bajo carga
* ¿Qué sucede cuando los tiempos de respuesta son largos? ¿Se finaliza esto automáticamente y se utiliza una opción de reserva como el inventario nativo?
* Qué tipo de monitorización está disponible para garantizar que las solicitudes de API se encuentren dentro de los límites de tolerancia

## Consideraciones para la administración de inventario no nativo

Mantenga las personalizaciones tan poco complejas como sea posible.
¿Qué tan plana puede ser la organización del inventario? ¿Es 1 SKU y la cantidad total de stock disponible? ¿O hay otros atributos que deben tenerse en cuenta?

Si la información de inventario es bastante plana, por ejemplo, un SKU y la cantidad total disponible, las opciones de tiempo casi real se amplían. El concepto de casi en tiempo real significa que hay una operación en segundo plano que recopila el inventario del origen y, a continuación, rellena un motor de almacenamiento para utilizarlo para responder a la solicitud. Para ello, puede utilizar elementos como Redis, Mongo u otras bases de datos no relacionales. Estas opciones son rápidas y funcionan bien para pares clave/valor. Si los datos son un poco más complejos, se requiere el uso de una base de datos de relaciones, ya sea dentro o fuera de la aplicación de comercio. Al descargar esto de la base de datos de comercio, mantiene la aplicación de comercio principal aislada de estas transacciones. Otro conjunto de ventajas es el ahorro de E/S de la aplicación de comercio, CPU, RAM y otros de uso. Para ahorrar recursos desde los servidores de aplicaciones de Adobe Commerce, aproveche las nuevas API para extraer los datos del almacenamiento fuera del sitio. Este proceso requiere un middleware para ayudar a transformar cualquier dato. A continuación, asegúrese de que la aplicación que realiza la llamada puede obtener el resultado esperado. Al utilizar Adobe App Builder con API mesh, los datos se pueden transformar y devolver con el formato correcto.

El uso de Adobe App Builder con API mesh también es una buena opción cuando hay varias fuentes de inventario.


## Mover la lógica de ejecución fuera del proceso

Adobe Developer App Builder proporciona un marco de trabajo de extensibilidad unificado de terceros para integrar y crear experiencias personalizadas para ampliar las soluciones de Adobe. Adobe Commerce puede utilizar Adobe Developer App Builder. Este método es un excelente ejemplo de uso para ampliar algunas funciones que normalmente se producen en la aplicación principal y que se trasladan fuera del sitio. Al eliminar la funcionalidad de la aplicación de Commerce, se reduce el número de módulos y la complejidad de la aplicación de Commerce. A su vez, un número menor de personalizaciones en proceso reduce la complejidad de la actualización y el mantenimiento.

Para inspirarse en cómo se realiza esta tarea, el equipo de Adobe ha creado documentación que es una gran fuente de inspiración y proporciona ejemplos de código de trabajo. Cuando un comprador agrega un producto al carro de compras, un sistema de administración de inventario de terceros comprueba si el artículo está en stock. Si es así, permita que se agregue el producto. De lo contrario, mostrar un mensaje de error. Para obtener ejemplos de código y más información, visite [Casos de uso de webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Cuándo realizar comprobaciones de inventario

El momento en el que se debe comprobar si el inventario aún está disponible depende del inversor empresarial, el arquitecto de software con algunas aportaciones de otras partes interesadas clave. Algunas ocasiones adecuadas incluyen al añadir un elemento al carro de compras y al entrar en el flujo de trabajo de cierre de compra. Cualquier otro evento añade carga a los sistemas back-end cuando no es necesario. Tenga en cuenta que el objetivo es detectar un problema de inventario solo cuando es primordial. Considere cuidadosamente otras comprobaciones que afecten al objetivo general de las comprobaciones del estado del inventario y solo las permita si las partes interesadas son conscientes del riesgo potencial de una carga adicional.

## Investigue el origen del inventario

Se requiere una investigación exhaustiva de la fuente de inventario externo. Los elementos que deben evaluarse son las opciones de API disponibles, la compatibilidad con GraphQL y los tiempos de respuesta esperados. Si el origen de inventario tiene un ancho de banda de conexión limitado o no estaba previsto que se usara en una solicitud en tiempo real, se excluye la capacidad de uso y el arquitecto debe considerar la posibilidad de utilizarlo casi en tiempo real. Si los tiempos de solicitud de la API exceden los parámetros definidos, se excluye de que sea una opción viable. Un ejemplo de este comportamiento es que las respuestas de API son de 200 ms para solicitudes únicas, pero aumentan a 500 o 900 ms con carga moderada. Esta situación empeora con más carga y descarta que haya llamadas de inventario en directo disponibles.

Asegúrese de probar los tiempos de respuesta de la API con solicitudes sencillas, así como con un gran volumen similar al tráfico esperado en el sitio web activo. Recuerde probar todas las áreas del comercio al mismo tiempo para simular escenarios del mundo real. Si se producen llamadas de inventario activo en páginas de productos, en el carro de compras y durante el cierre de compra, las pruebas de carga deben simular todas estas llamadas simultáneamente para imitar el comportamiento real del cliente.

## Opciones de reserva

Si el origen del inventario no está operativo y la monitorización está disponible, se recomienda utilizar la capacidad nativa de Adobe Commerce. Sin embargo, con una monitorización adecuada, la experiencia del cliente puede cambiar dinámicamente para reflejar la pérdida de comprobaciones de inventario en tiempo real. Esto significa que una venta o evento se cancela antes de tiempo o se elimina de la pantalla para evitar la sobreventa. Analice el plan de reserva con el propietario de la tienda para que todos entiendan el proceso automático que se lleva a cabo si la fuente de inventario se desactiva.

## Conclusión

La decisión de realizar comprobaciones de inventario en tiempo real es importante. Garantizar que el propietario del sitio web, el equipo de desarrollo y otros estén completamente formados y sean conscientes de todos los beneficios y posibles escollos recae en el desarrollador principal o arquitecto. Proporcionar un plan reflexivo que cubra las razones y un proceso de reserva es clave para el éxito.

Las comprobaciones de inventario en vivo se pueden realizar, pero requieren investigación y reflexión en torno a las pruebas y la validación durante el ciclo de control de calidad. Garantizar que las pruebas de carga y las pruebas automatizadas de extremo a extremo ayudan a garantizar que todos los problemas potenciales se detecten y se clasifiquen.

Si la monitorización detecta llamadas fallidas o tiempos de respuesta lentos, realice acciones para mantener el sitio en línea y minimizar la irritación del cliente. Las opciones de reserva van desde el uso de la funcionalidad nativa hasta la desactivación de promociones, la notificación al equipo de desarrollo o el redireccionamiento de solicitudes a un sistema backend secundario. La implementación del mecanismo de reserva debería planificarse con el mismo cuidado que la integración real, ya que todos los sistemas tienen problemas en algún momento. Todo lo que esté automatizado o necesite una acción manual debe estar claramente documentado.

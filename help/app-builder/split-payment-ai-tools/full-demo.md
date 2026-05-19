---
title: 'Crear un POC de pago dividido: demostración completa de App Builder'
description: Descubra cómo funcionan el pago dividido, REST, App Builder I/O y la aceptación/rechazo del operador en esta demostración de Luma, además de un total de pedido previo configurable que puede bloquear el carro de compras.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 933
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 9add0b4bfa1eba33ec359adaa766b64711df25ba
workflow-type: tm+mt
source-wordcount: '1031'
ht-degree: 0%

---

# Crear un POC de pago dividido: demostración completa de App Builder

Este es el tutorial completo de la prueba de concepto de pago dividido creada en Adobe Commerce y Adobe App Builder. La demostración supone que ya ha utilizado las herramientas de IA y un indicador para producir la extensión de Commerce en proceso y la aplicación de App Builder; este vídeo muestra lo que sucede después de combinar ese código, implementarlo en un sitio web de Adobe Commerce Cloud con la temática nativa de Luma y de que el proyecto de App Builder esté activo.

Un comprador paga con parte en efectivo y parte **[!UICONTROL Store Credit]**. Commerce posee un cierre de compra sincrónico y las API que necesita la tienda; App Builder gestiona la orquestación, los flujos de trabajo del operador y los consumidores de eventos de E/S. La implementación de referencia utiliza un proyecto de Commerce (PaaS) y el proceso de cierre de compra nativo de Luma, en lugar de una tienda de Edge Delivery Services, que sigue siendo una ruta común para muchos comerciantes. Si usa **Adobe Commerce as a Cloud Service** en una topología diferente, el código de App Builder sigue siendo similar, pero el trabajo de tienda y en proceso tendría un aspecto diferente. Para las aplicaciones locales, autoalojadas y Commerce en la nube en Luma, este vídeo muestra una división práctica entre el código en proceso y App Builder para la nueva funcionalidad.

## Vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3484087?learn=on)

## ¿Para quién es este vídeo?

* Arquitectos técnicos que planifican la extensibilidad de Commerce
* Desarrolladores que implementan checkout e integraciones
* Equipos de implementación que necesitan una división realista entre PHP y App Builder

## Configurar el escenario en la tienda

La cuenta de demostración tiene **[!UICONTROL Store Credit]** y usted ve ese saldo en el cierre de compra. Añada productos hasta que el total del pedido sea de unos 80 $. Ese tamaño le da espacio para mostrar tanto el camino de aceptación exitoso como un camino de declinación, similar a una caída de cartas, sin que las matemáticas se opongan a usted.

**[!UICONTROL Split payment]** solo se ofrece a los clientes que iniciaron sesión, ya que la sesión debe exponer el crédito de la tienda. El cierre de compra de invitado no muestra la opción de división.

1. En el paso **[!UICONTROL Payment]**, elige el método de cobro (la demostración cambia el nombre de **[!UICONTROL Cash on delivery]** a **Solo cobro**).
1. Aparecerá un área de pago dividido debajo del método de pago. El campo de efectivo está rellenado previamente en el total del pedido y el componente lee **[!UICONTROL Store Credit]** de la sesión.
1. Para un carro de compras de ~$80, establezca **$50** efectivo y **$30** crédito de la tienda para que el componente muestre $50 + $30 = $80.
1. Cuando las cantidades sean válidas, realice el pedido.

La interfaz de usuario déclencheur una llamada a una nueva API de REST **[!UICONTROL Commerce]** para la división. La desprotección se mantiene sincrónica: Commerce aplica el crédito de la tienda, almacena líneas divididas en el pedido y emite un evento de E/S que App Builder consume más adelante. La página de éxito es inmediata para el cliente.

## Revise el pedido en el administrador de Commerce

En **[!UICONTROL Sales]** > **[!UICONTROL Orders]**, abra el nuevo pedido. El área **[!UICONTROL Payment information]** muestra la división, por ejemplo **$50** efectivo y **$30** crédito de la tienda. El estado es **Pendiente** cuando el efectivo aún no se ha cobrado; el crédito de tienda ya se recibe cuando se crea el pedido.

**[!UICONTROL Comments]** puede incluir texto que App Builder agregó. **[!UICONTROL Payment orchestrator action]** en App Builder recibió el evento de E/S, comprobó las cantidades divididas y utilizó REST autenticado para anexar comentarios. **[!UICONTROL Commerce]** trata esto como cualquier otra llamada de REST y no necesita conocer al escritor.

## Uso del panel del operador de App Builder (ERP o back office)

Una experiencia HTML simple se ejecuta como una acción web **[!UICONTROL App Builder]** (no **[!UICONTROL Admin]** para esta vista). El tablero llama a la API REST **[!UICONTROL Commerce Order]** y filtra a **Pendiente** para que encuentres la parte en efectivo de 50 $.

Normalmente, este patrón se integra con una herramienta ERP o de fraude. Se muestran dos acciones:

* **Aceptar**: confirma que se recibió efectivo (en producción, a menudo una publicación automatizada desde una caja registradora o un sistema de tienda).
* **Rechazar**: simula un fraude o un paso de recopilación fallido (en producción, normalmente automatizado desde el servicio de riesgos).

**Aceptar y completar el pedido**

1. En la aplicación **[!UICONTROL App Builder]**, usa **Accept** para confirmar el efectivo.
1. La aplicación llama a un nuevo extremo **[!UICONTROL Commerce]** que finaliza la línea de efectivo. El flujo de pedidos principal (facturación y, en esta compilación, un envío automatizado) se ejecuta con el comportamiento nativo **[!UICONTROL Commerce]**. Ninguna de esas rutas requiere un ser humano en **[!UICONTROL Admin]**; la orquestación y el extremo están activos en App Builder.

Actualice el mismo pedido en **[!UICONTROL Admin]**: el estado pasa a **Completado** cuando se acepta el efectivo, con factura y, cuando el producto se puede enviar, un envío para acortar el ciclo de vida completo en la demostración.

**Rechazar y cancelar**

1. Abra un pedido generado previamente que aún se encuentre en el escenario de disminución.
1. En la aplicación **[!UICONTROL App Builder]**, use **Rechazar** para simular un fraude o un error de colección.
1. En **[!UICONTROL Admin]**, el pedido es **Cancelado**. El historial muestra un estado de efectivo rechazado, los comentarios explican el resultado, no se cargó al comprador la línea de crédito de tienda como si fuera una venta final y la factura de crédito de tienda se anula con la cancelación. Un sistema de producción también notificaría al cliente y liberaría cualquier retención en el inventario o servicio.

## Umbral total del pedido (validación antes de crear el pedido)

La configuración de App Builder o **[!UICONTROL Commerce]** puede admitir reglas de validación; esta compilación bloquea los pedidos superiores a **$100** en el valor del carro de compras (un límite de demostración que puede cambiar). Una comprobación de valor no es la regla de negocio más común (las comprobaciones de inventario o disponibilidad son más típicas), pero muestra que la validación se puede ejecutar a las **[!UICONTROL Payment]** en **[!UICONTROL Commerce]** antes de crear una solicitud, mientras que App Builder sigue controlando la automatización de seguimiento.

1. Agregue productos hasta que el total supere **$100**.
1. La división **[!UICONTROL UI]** sigue apareciendo; el resultado de los límites superiores solo aparece en **Realizar pedido**.
1. El comprador ve un error genérico, como **No se pudo procesar el pago. Inténtelo de nuevo o póngase en contacto con el soporte técnico.**
1. **[!UICONTROL cart]** no ha cambiado para que el cliente pueda reducir el total, elegir otro método o ponerse en contacto con el servicio de soporte técnico.

Una puntuación de riesgo, regla de región o similar podría conectarse aquí; **[!UICONTROL Commerce]** bloquea la solicitud y **[!UICONTROL App Builder]** puede recibir notificaciones y casos prácticos después del hecho.

## Commerce local, Commerce en la nube y **Adobe Commerce as a Cloud Service**

El tutorial coincide con **[!UICONTROL Adobe Commerce]** en una infraestructura que administra o con **[!UICONTROL Commerce in the cloud]** (PaaS) con una tienda tradicional. Para **[!UICONTROL Adobe Commerce as a Cloud service]** (SaaS) con un front-end diferente y sin módulo en proceso en esta forma, las partes de App Builder son en gran medida iguales, mientras que el trabajo de tienda y extensión sería diferente. En todos los casos, se aplica el mismo principio: deje que **[!UICONTROL Commerce]** haga lo que debe suceder dentro de la solicitud **[!UICONTROL Commerce]** y use **[!UICONTROL App Builder]** para el resto de la experiencia.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

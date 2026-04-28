---
title: 'Create a split payment POC: App Builder full demo'
description: Learn how split payment, REST, App Builder I/O, and operator accept/decline work in this Luma demo, plus a configurable pre-order total that can block the cart.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 955
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: b3a9cee9ab59307883444650e8ee2423ab630b6b
workflow-type: tm+mt
source-wordcount: '1050'
ht-degree: 0%

---

# Create a split payment POC: App Builder full demo

This is the end-to-end walkthrough of the split payment proof of concept built on Adobe Commerce and Adobe App Builder. The demo assumes you have already used AI tools and a prompt to produce the in-process Commerce extension and the App Builder app; this video shows what happens after that code is merged, deployed to an Adobe Commerce Cloud website using the native Luma theme, and the App Builder project is live.

A shopper pays with part cash and part **[!UICONTROL Store Credit]**. Commerce owns synchronous checkout and the APIs the storefront needs; App Builder handles orchestration, operator workflows, and I/O event consumers. The reference implementation uses a Commerce (PaaS) project and the Luma native checkout rather than an Edge Delivery Services storefront, which is still a common path for many merchants. If you use **Adobe Commerce as a Cloud service** in a different topology, the App Builder code stays similar but storefront and in-process work would look different. For on-premises, self-hosted, and Commerce in the cloud on Luma, this video shows a practical split between in-process code and App Builder for new functionality.

## Vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3484087?learn=on)

## Who is this video for?

* Technical architects planning Commerce extensibility
* Developers implementing checkout and integrations
* Implementation teams who need a realistic split between PHP and App Builder

## Set up the scenario on the storefront

The demo account has **[!UICONTROL Store Credit]** and you see that balance in checkout. Add products until the order total is about $80. That size gives you room to show both the successful accept path and a decline path, similar to a card decline, without the math fighting you.

**[!UICONTROL Split payment]** is only offered to signed-in customers, because the session must expose store credit. Guest checkout does not show the split option.

1. On the **[!UICONTROL Payment]** step, choose the cash method (the demo renames **[!UICONTROL Cash on delivery]** to **Just Cash**).
1. A split payment area appears under the payment method. The cash field is prefilled to the order total, and the component reads **[!UICONTROL Store Credit]** from the session.
1. For an ~$80 cart, set **$50** cash and **$30** store credit so the component shows $50 + $30 = $80.
1. When the amounts are valid, place the order.

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

## Recursos adicionales

* [Crear un POC de pago dividido: App Builder y herramientas de IA](create-a-split-payment-poc-app-builder-and-ai-tools.md): la introducción a los objetivos y la arquitectura de la serie.

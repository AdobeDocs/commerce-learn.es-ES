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
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
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

The UI triggers a call to a new **[!UICONTROL Commerce]** REST API for the split. Checkout stays synchronous: Commerce applies store credit, stores split lines on the order, and emits an I/O event that App Builder consumes later. The success page is immediate for the customer.

## Review the order in the Commerce admin

In **[!UICONTROL Sales]** > **[!UICONTROL Orders]**, open the new order. The **[!UICONTROL Payment information]** area shows the split, for example **$50** cash and **$30** store credit. The status is **Pending** when cash is not yet collected; store credit is already taken when the order is created.

**[!UICONTROL Comments]** can include text that App Builder added. The **[!UICONTROL Payment orchestrator action]** in App Builder received the I/O event, checked the split amounts, and used authenticated REST to append comments. **[!UICONTROL Commerce]** treats that like any other REST call and does not need to know the writer.

## Use the App Builder operator dashboard (ERP or back office)

A simple HTML experience runs as an **[!UICONTROL App Builder]** web action (no **[!UICONTROL Admin]** for this view). The dashboard calls the **[!UICONTROL Commerce Order]** REST API and filters to **Pending** so you can find the $50 cash leg.

You typically integrate this pattern with an ERP or fraud tool. Two actions are demonstrated:

* **Accept** — Confirms that cash was received (in production, often an automated post from a cash drawer or store system).
* **Decline** — Simulates fraud or a failed collection step (in production, usually automated from your risk service).

**Accept and complete the order**

1. In the **[!UICONTROL App Builder]** app, use **Accept** to confirm cash.
1. The app calls a new **[!UICONTROL Commerce]** endpoint that finalizes the cash line. The core order flow (invoicing, and in this build an automated shipment) runs with native **[!UICONTROL Commerce]** behavior. None of that path requires a human in **[!UICONTROL Admin]**; orchestration and the endpoint live in App Builder.

Refresh the same order in **[!UICONTROL Admin]**: status moves to **Complete** when the cash is accepted, with invoice and, where the product is shippable, a shipment to shorten the full lifecycle in the demo.

**Decline and cancel**

1. Open a prebuilt order that is still in the decline scenario.
1. In the **[!UICONTROL App Builder]** app, use **Decline** to simulate a fraud or collection failure.
1. In **[!UICONTROL Admin]**, the order is **Cancelled**. The history shows a declined cash status, comments explain the result, the shopper was not charged the store-credit line as if it were a final sale, and the store-credit invoice is voided with the cancellation. A production system would also notify the customer and release any holds on inventory or service.

## Order total threshold (validation before the order is created)

App Builder or **[!UICONTROL Commerce]** configuration can support validation rules; this build blocks orders over **$100** in cart value (a demo cap you can change). A value check is not the most common business rule—inventory or availability checks are more typical—but it shows that validation can run at **[!UICONTROL Payment]** in **[!UICONTROL Commerce]** before an order is created, while App Builder still handles follow-up automation.

1. Add products until the total is above **$100**.
1. The split **[!UICONTROL UI]** still appears; the over-limit result only surfaces on **Place order**.
1. The shopper sees a generic error such as **Payment could not be processed. Please try again or contact support.**
1. The **[!UICONTROL cart]** is unchanged so the customer can lower the total, pick another method, or contact support.

A risk score, region rule, or similar could plug in here; **[!UICONTROL Commerce]** blocks the order, and **[!UICONTROL App Builder]** can own notifications and case work after the fact.

## Commerce on premises, Commerce in the cloud, and **Adobe Commerce as a Cloud service**

The walkthrough matches **[!UICONTROL Adobe Commerce]** on an infrastructure you manage or **[!UICONTROL Commerce in the cloud]** (PaaS) with a traditional storefront. For **[!UICONTROL Adobe Commerce as a Cloud service]** (SaaS) with a different front end and no in-process module in this shape, the App Builder pieces are largely the same, while storefront and extension work would be different. In all cases, the same principle holds: let **[!UICONTROL Commerce]** do what must happen inside the **[!UICONTROL Commerce]** request, and use **[!UICONTROL App Builder]** for the rest of the experience.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

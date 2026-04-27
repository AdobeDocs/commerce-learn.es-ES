---
title: 'Crear un POC de pagos divididos: Herramientas de App Builder e IA'
description: Obtenga información acerca de la prueba de concepto de pago dividido con App Builder y Commerce PaaS, incluidos los objetivos, la arquitectura y lo que cubre esta primera sesión.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, Integrations
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 259
jira: KT-20791
source-git-commit: 47b35088f2d3139d58791a2f7d327159db8f2175
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# Crear un POC de pagos divididos: Herramientas de App Builder e IA

Este es el primero de un conjunto de tutoriales que le presentan el uso del desarrollo asistido por IA para ayudarle a crear una prueba de concepto de pago dividido. Trabaje con Adobe App Builder y Adobe Commerce en la nube (PaaS) o en línea, y pase de una descripción general en esta sesión a los tutoriales prácticos que le siguen. Espere objetivos, arquitectura de alto nivel y una hoja de ruta para lo que cubre el resto de la serie.

## Vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## Detalles importantes


Este tutorial reduce la brecha entre lo que la mayoría de los equipos de Adobe Commerce están hoy en día, profundamente en PHP, y adónde Adobe quiere que vayan: lógica empresarial basada en eventos y sin servidor que se ejecuta fuera de Commerce en Adobe App Builder. Esto se hace a través de una característica real de trabajo en lugar de un ejemplo de juguete.

### Lo que realmente va a construir

Un sistema de pago dividido en el que los clientes pagan mediante una combinación de Pago contra reembolso y Crédito de tienda. Una vez realizado el pedido, un operador (o sistema ERP) confirma o rechaza el pago en efectivo a través de un panel independiente, sin necesidad de abrir Commerce Admin. Todo el flujo de trabajo de aceptación/rechazo se ejecuta en App Builder.
La lección de arquitectura (esta es la enseñanza básica)
El tutorial muestra un marco de decisión deliberado y repetible:

Qué debe permanecer en PHP: cualquier cosa que se ejecute sincrónicamente en el ciclo de petición Commerce o que llame a las API internas de Commerce sin una superficie externa limpia
Qué se mueve a App Builder: todo lo demás: procesamiento de eventos, flujo de trabajo del operador, integraciones externas y herramientas orientadas al operador

Esto no es &quot;mover todo a App Builder&quot;. Es un punto de partida práctico y honesto para los equipos que necesitan comenzar la transición sin una reescritura.

### Por qué no se incluye el código PHP

El método de petición de datos de IA es en realidad mejor que el código de ejemplo para este caso de uso, ya que captura los contratos de comportamiento y el razonamiento arquitectónico que el código de ejemplo por sí solo no puede transmitir. Un desarrollador que ejecuta el símbolo del sistema y lee el resultado comprende por qué el código tiene la forma que tiene, no solo su aspecto.

### Qué se incluye

Código completo de la aplicación App Builder (coherente en todos los proyectos; utilícelo directamente o como referencia)
Un conjunto completo de peticiones de datos de IA numeradas diseñadas para Cursor y Claude que cubren el módulo de Commerce, App Builder orchestrator, el tablero del operador, las pruebas y la ruta a la producción
Un script de simulación para probar el flujo de aceptación/rechazo en un sitio de Commerce activo sin necesidad de un ERP real.
Documentación de arquitectura que explica cada decisión

### Para quién es esto

Desarrolladores en Adobe Commerce on-premise o Commerce Cloud que inicien su primera integración real con App Builder. No para la implementación totalmente sin encabezado de API, específicamente para equipos en transición que tienen una tienda tradicional y quieren ver cómo descargar lógica empresarial real en App Builder sin abandonar su inversión existente en PHP.

### Llamada de requisitos previos

Adobe Commerce 2.4.5 o posterior, acceso a una organización de Adobe Developer Console con un proyecto de App Builder y eventos de E/S habilitados. Se supone que la interfaz de usuario de cierre de compra tiene un tema de Luma limpio: las temáticas personalizadas necesitarán ajustar la solicitud antes de ejecutarla.

### Pensamientos finales

Esto debería considerarse un debate de nivel inicial sobre el desarrollo asistido por IA. Este tutorial es una demostración del uso de las herramientas de IA en un flujo de trabajo de desarrollo de Commerce, no solo un tutorial sobre pagos divididos.

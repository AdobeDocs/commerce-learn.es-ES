---
title: 'POC de pagos divididos: referencia rápida del tutorial'
description: 'Obtenga información acerca del mapa del archivo POC de pagos divididos: qué página de Experience League coincide con cada solicitud de IA, orden de sección sugerido y notas de autor para el cierre de compra de Luma.'
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '1455'
ht-degree: 0%

---

# POC de pagos divididos: referencia rápida del tutorial

Esta página resume cómo se organiza la serie de tutoriales de prueba de concepto de pago dividido. Asigna cada archivo de solicitud de origen numerado a su página de Experience League publicada, enumera un orden de sección sugerido para los lectores y recopila notas para los autores y administradores.


## Referencia archivo por archivo

### [Crear un POC de pago dividido: herramientas de App Builder e IA](create-a-split-payment-poc-app-builder-and-ai-tools.md)

**etiqueta Source:** `00_TUTORIAL_OVERVIEW.md` (descripción general conceptual; la serie publicada se abre con esta página).

**Propósito:** Introducción y orientación para el tutorial.

**Por qué se necesita:** Ofrece a los desarrolladores el &quot;por qué&quot; antes del &quot;cómo&quot;. Explica qué construirán, quién es la audiencia y, de forma crítica, qué deben personalizar si su sitio difiere de una instalación de Luma limpia. También sirve como tabla de contenido para el resto de la serie.

**Uso del tutorial:** Sección de apertura. Establece el contexto antes de cualquier paso técnico.


### [POC de pago dividido: decisiones de arquitectura y diseño](split-payment-poc-architecture-and-decisions.md)


**Propósito:** explicación detallada de cada decisión arquitectónica en el PoC.

**Por qué es necesario:** El punto de confusión más común al trabajar con este PoC es comprender *por qué* hay algo en PHP en comparación con App Builder. Sin este contexto, los desarrolladores intentan mover cosas que no se pueden mover (como la aplicación de crédito de la tienda) y fallan. Este documento se anticipa a esos fracasos con un razonamiento claro.

**Temas clave cubiertos:**

* La regla &quot;qué debe permanecer en PHP&quot; y por qué
* El patrón de doble aplicación de umbral
* Por qué la solicitud de crédito de tienda no puede ser asincrónica
* Los cinco casos extremos de Commerce administrados por complementos
* El flujo de datos de atributos de extensión del cierre de compra → citar → pedido → evento de E/S → App Builder

**Uso de tutorial:** sección &quot;Arquitectura&quot; o &quot;Cómo funciona&quot;. Los desarrolladores de Commerce experimentados pueden omitirlo, pero es esencial para los recién llegados a App Builder.


### [POC de pago dividido: requisitos previos y configuración del entorno](split-payment-poc-prerequisites-and-setup.md)


**Propósito:** Complete la lista de comprobación previa al vuelo antes de escribir código o de ejecutar mensajes.

**Por qué es necesario:** La fase de instalación tiene la tasa de errores más alta en los tutoriales. Este documento garantiza que el administrador de Commerce esté configurado correctamente (título de COD exacto, crédito de tienda habilitado, cliente de prueba con saldo, integración creada), que el proyecto de App Builder tenga eventos de E/S y que el proveedor de eventos de Commerce esté conectado, y que el entorno local tenga las versiones correctas.

**Elementos de énfasis:**

* El título de la entrega contra reembolso debe ser exactamente `Cash` (esencial: JavaScript hace coincidir cadenas)
* La integración de Commerce debe estar *activada* (no solamente guardada) para que funcionen las credenciales de OAuth
* `entity_id` frente a `increment_id` se explican aquí para evitar confusiones durante

**Uso del tutorial:** Sección &quot;Requisitos previos&quot; o &quot;Introducción&quot;. Debe completarse de forma interactiva, no solo leerse.


### [POC de pago dividido: referencia de variables de entorno](split-payment-poc-env-reference.md)


**Propósito:** todas las variables de entorno para los tres componentes en un solo lugar.

**Por qué es necesario:** Las mismas cuatro credenciales de OAuth aparecen en tres archivos diferentes con nombres de variables diferentes (`COMMERCE_*` en comparación con `COMMERCE_INTEGRATION_*`). Tener una sola referencia evita la depuración de por qué no funciona cuando un conjunto de credenciales tiene un error tipográfico.

**Componentes cubiertos:**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` (IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**Uso del tutorial:** Barra lateral de referencia o sección &quot;Configuración&quot;. También se utiliza como complemento de las indicaciones de compilación.


### [POC de pago dividido: petición de API del módulo Commerce](split-payment-poc-commerce-module-prompt.md)


**Propósito:** Mensaje de IA completo e independiente para generar todo el módulo PHP `Client_SplitPayment`.

**Por qué es necesario:** Este es el artefacto principal de generación de IA para PHP. Está escrito para que un desarrollador sin experiencia PHP pueda entregarlo a Cursor (con Claude) y obtener un módulo de trabajo. Se especifican todos los archivos, se definen todos los contratos de comportamiento, se incluyen notas de implementación críticas y se proporcionan los comandos para habilitar el módulo posteriormente.

**Cobertura:**

* Estructura completa de los archivos (más de 30 archivos)
* Esquema de base de datos, atributos de extensión, extremos REST, configuración de eventos de E/S
* Todas las clases de PHP con especificaciones de comportamiento completas (no solo firmas)
* Especificaciones del componente de cierre de compra KnockoutJS
* Los cinco complementos de casos extremos con explicaciones de por qué existe cada uno
* Llamadas de personalización para temas que no son de Luma

**Uso del tutorial:** sección &quot;Generar el módulo Commerce&quot;. El indicador en sí es el artefacto: los desarrolladores lo copian en su herramienta de IA y lo ejecutan.


### [POC de pago dividido: petición de App Builder orchestrator AI](split-payment-poc-app-builder-orchestrator-prompt.md)


**Propósito:** Mensaje de IA completo e independiente para generar la aplicación de App Builder `split-payment-orchestrator`.

**Por qué es necesario:** Las cuatro acciones de App Builder (`payment-orchestrator`, `payment-accept`, `payment-decline`, `demo-dashboard`) son el núcleo de la historia de &quot;qué se movió de PHP&quot;. Este mensaje los genera todos con especificaciones de comportamiento completas, estructura `app.config.yaml` correcta y la configuración de registro de eventos.

**Cobertura:**

* `app.config.yaml` con las cuatro acciones y el registro de eventos de E/S
* `commerce-client.js` cliente compartido de OAuth 1.0a
* Las cuatro acciones con especificaciones lógicas completas
* El código auxiliar `store-credit.js` está obsoleto (con una explicación de *por qué* no se usa; esto es importante desde el punto de vista pedagógico)
* El panel de demostración con procesamiento de HTML, filtrado de pedidos y seguridad
* `.env.example` con todas las variables

**Uso del tutorial:** sección &quot;Generar la aplicación de App Builder&quot;. Acompañante del mensaje del módulo de Commerce.


### [POC de pago dividido: petición de API de la extensión Experience Cloud UI](split-payment-poc-experience-cloud-ui-prompt.md)


**Propósito:** petición de datos de IA para generar la extensión opcional SDK de la IU de administración de Experience Cloud.

**Por qué es necesario:** El tablero de demostración en el indicador del orquestador es intencionalmente áspero: es prueba de concepto, no producción. Esta sección muestra a los desarrolladores el siguiente paso: incrustar el panel de pago dividido en el Admin Shell de Commerce mediante la IU de administración de SDK. Faltaba por completo en los mensajes originales.

**Cobertura:**

* `ext.config.yaml` para la extensión SDK de la IU de administración
* Componentes de React para el panel de pedidos y los detalles del pedido
* Acciones back-end que utilizan la autenticación IMS para listar y OAuth para aceptar/rechazar
* El script de simulación (también utilizado para pruebas)

**Uso del tutorial:** Sección opcional &quot;Yendo más lejos&quot; o &quot;Ruta de producción&quot;. Se puede omitir si el tutorial se centra únicamente en el PoC.


### [POC de pago dividido: guía de prueba y verificación](split-payment-poc-testing-and-verification.md)


**Propósito:** Guía de pruebas paso a paso que cubre todos los componentes en el orden de verificación correcto.

**Por qué es necesario:** La creación de los componentes es la mitad del trabajo. Los desarrolladores necesitan una ruta de verificación clara que comience desde el nivel más bajo (columnas de base de datos, puntos finales REST) y se genere en el flujo completo de extremo a extremo. Las indicaciones originales tenían una lista de comprobación de configuración, pero no un flujo de prueba explícito.

**Cobertura:**

* Verificación de la instalación del módulo Commerce
* Verificación de configuración de administración
* Pruebas de humo de extremo REST (comandos curl)
* Tutorial de la interfaz de usuario de cierre de compra (incluidos casos de validación)
* Prueba de protección de umbral
* Prueba de colocación de pedido completo
* Uso del script de simulación para aceptar y rechazar
* Uso del tablero de demostración
* Inspección del registro de acciones de App Builder
* Diez modos de error comunes con correcciones específicas

**Uso del tutorial:** Sección &quot;Pruebas&quot; o &quot;Verificación&quot;. También es útil como referencia para la resolución de problemas.


### [POC de pago dividido: pasos siguientes después de la prueba de concepto](split-payment-poc-next-steps.md)


**Propósito:** hoja de ruta para convertir el PoC en patrones listos para la producción.

**Por qué es necesario:** Un tutorial de PoC corre el riesgo de dejar a los desarrolladores con un &quot;¿qué pasa ahora?&quot; sensación. Este documento asigna las progresiones naturales desde la demostración a la producción: reemplazando el panel de demostración con una extensión de Experience Cloud, conectando un ERP real, añadiendo API Mesh, expandiendo el flujo de trabajo de App Builder y la lista de comprobación de implementación de producción.

**Contenido de clave:**

* Diagrama de evolución de la arquitectura (PoC → Phase 2 → Phase 3)
* Patrón de integración de ERP (qué cambia, qué sigue siendo lo mismo)
* Patrón de corredor de malla API
* Lista de comprobación de implementación de producción
* Vínculos de referencia clave

**Uso del tutorial:** Sección de cierre &quot;Pasos siguientes&quot;. También es útil como punto de partida para iniciar conversaciones sobre un proyecto real.


## Secciones del tutorial sugeridas

En función de estos archivos, una estructura típica para los lectores es:

| Sección Tutorial | página de Experience League |
| --- | --- |
| Introducción y objetivos de aprendizaje | [Crear un POC de pago dividido: herramientas de App Builder e IA](create-a-split-payment-poc-app-builder-and-ai-tools.md) |
| Demostración de extremo a extremo (vídeo) | [Crear un POC de pago dividido: demostración completa de App Builder](create-a-split-payment-poc-app-builder-and-ai-tools-full-demo.md) |
| Arquitectura: lo que vive donde | [POC de pago dividido: decisiones de arquitectura y diseño](split-payment-poc-architecture-and-decisions.md) |
| Requisitos previos y configuración | [POC de pago dividido: requisitos previos y configuración del entorno](split-payment-poc-prerequisites-and-setup.md) |
| Variables de entorno | [POC de pago dividido: referencia de variables de entorno](split-payment-poc-env-reference.md) |
| Paso 1: Generación del módulo de Commerce | [POC de pago dividido: petición de API del módulo Commerce](split-payment-poc-commerce-module-prompt.md) |
| Paso 2: Creación de App Builder Orchestrator | [POC de pago dividido: petición de App Builder orchestrator AI](split-payment-poc-app-builder-orchestrator-prompt.md) |
| Paso 3: Prueba del flujo de extremo a extremo | [POC de pago dividido: guía de prueba y verificación](split-payment-poc-testing-and-verification.md) |
| Paso 4 (opcional): Extensión de la IU de administración | [POC de pago dividido: petición de API de la extensión Experience Cloud UI](split-payment-poc-experience-cloud-ui-prompt.md) |
| Pasos siguientes y ruta de producción | [POC de pago dividido: pasos siguientes después de la prueba de concepto](split-payment-poc-next-steps.md) |


## Notas importantes para los autores de tutoriales

**En el supuesto del tema de Luma:** Cada mensaje de compilación genera código para una instalación de Luma limpia. El tutorial debe indicar claramente esto en la parte superior: los desarrolladores con cierres de compra personalizados deberán adaptar las rutas de inyección de `LayoutProcessorPlugin` y posiblemente la configuración de RequireJS. La introducción de series y los indicadores de compilación incluyen llamadas de personalización para esto.

**En el módulo PHP:** El tutorial explícitamente no proporciona el código PHP como una descarga de copiar y pegar. El mensaje de IA *lo genera*. Esto es intencional: enseña el patrón de uso de la IA para crear extensiones de Commerce, no solo copiar y pegar plantillas. Sin embargo, el código generado cuando se le solicita en un entorno limpio debe coincidir exactamente con el código de trabajo de `app/code/Client/SplitPayment/`.

**En el umbral de 100 $:** Este es un valor de PoC codificado. El tutorial debe tener en cuenta que los almacenes reales querrán configurarlo mediante la configuración de administración de Commerce en lugar de una constante de tiempo de compilación.

**Dependencia de crédito de la tienda:** El flujo de pago dividido tal como se creó requiere `Magento_CustomerBalance` (el módulo de crédito de la tienda nativa).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

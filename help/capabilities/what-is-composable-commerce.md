---
title: Cómo crea el Adobe Commerce compositable
description: Obtenga información sobre el comercio componible, priorice un enfoque de API-First e implemente una arquitectura modular y orientada a servicios.
feature: App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-27T00:00:00Z
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
source-git-commit: d578c066f3e51827694c8bf85aa2324035a8b07b
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 0%

---

# Comercio componible

El comercio de composición es un enfoque arquitectónico en el comercio electrónico que implica desacoplar la capa de presentación del front-end de la funcionalidad de comercio del back-end. palo de golf Permite a las empresas seleccionar y combinar los mejores componentes o módulos para crear una solución personalizada. Este enfoque implica desglosar la plataforma monolítica tradicional de comercio electrónico en servicios o microservicios más pequeños e independientes que puedan estar compuestos juntos. El comercio de composición ofrece beneficios como flexibilidad, escalabilidad, personalización, agilidad y la capacidad de integraciones más sencillas con otros sistemas y tecnologías.

Adobe Commerce proporciona muchas funciones y herramientas para ayudar a los comerciantes a adoptar e implementar el comercio componible. Adobe Commerce ofrece una metodología de comercio componible y experiencias front-end híbridas y sin encabezado. Con la extensibilidad fuera de proceso en mente, Adobe ofrece API Mesh para integrar varios servicios y Adobe App Builder para crear microservicios personalizados.

## ¿Por qué es importante el comercio compuesto?

Comprender el comercio de composición es importante para las empresas y las personas que participan en la industria del comercio electrónico por varias razones. Proporciona flexibilidad y personalización, escalabilidad y agilidad, capacidades de integración, futuras pruebas y capacitación para desarrolladores. Composable commerce permite a las empresas adaptar y evolucionar sus plataformas de comercio electrónico, escalar y hacer crecer sus operaciones. Algunos beneficios más impresionantes incluyen la capacidad de integrarse con otros sistemas y tecnologías, mantenerse al día con las expectativas cambiantes de los clientes y aprovechar la experiencia de los desarrolladores.

Ayuda a las empresas a navegar por el cambiante panorama del comercio electrónico, tomar decisiones informadas y aprovechar los beneficios de la flexibilidad, la escalabilidad, la personalización, la agilidad y las capacidades de integración. Además, conocer el comercio de composición es importante para el crecimiento del negocio, la personalización, la agilidad y la innovación, la eficiencia de costes, la integración y la flexibilidad, y para afianzar el futuro. Permite a las empresas la oportunidad de adaptarse rápidamente a las nuevas funciones y responder a las cambiantes condiciones del mercado, creando soluciones altamente personalizadas. Hay más beneficios que incluyen la capacidad de innovar rápidamente, reducir los costes de desarrollo y mantenimiento e integrarlos con servicios y sistemas de terceros.

En general, comprender el comercio componible permite a las empresas tomar decisiones informadas, optimizar su arquitectura y estrategia de comercio electrónico e impulsar el crecimiento y el éxito en el mercado digital.

## ¿Cómo pueden las empresas beneficiarse del comercio compuesto?

Las empresas pueden beneficiarse del comercio compuesto de varias formas:

**Flexibilidad y personalización:** El comercio de composición permite a las empresas seleccionar y combinar los mejores componentes o microservicios para crear una solución de comercio electrónico personalizada que satisfaga sus necesidades específicas. Permite a las empresas adaptar su plataforma para ofrecer experiencias de cliente únicas, implementar funcionalidades especializadas y diferenciarse en el mercado.

**Escalabilidad y agilidad:** Con una arquitectura componible, las empresas pueden escalar diferentes componentes de su plataforma de comercio electrónico de forma independiente. Esto significa que pueden asignar recursos y escalar funcionalidades específicas en función de la demanda, lo que garantiza un rendimiento y una rentabilidad óptimos. El comercio de composición también permite prácticas de desarrollo ágiles, lo que permite a los equipos trabajar en diferentes componentes simultáneamente e implementar cambios de forma independiente, lo que resulta en un tiempo de salida al mercado más rápido.

**Capacidades de integración:** El comercio de composición facilita la integración perfecta con sistemas, servicios y tecnologías de terceros. Las empresas pueden conectar fácilmente su plataforma de comercio electrónico con varias herramientas, como puertas de enlace de pago, sistemas ERP, sistemas CRM, plataformas de automatización de marketing y mucho más. Esta capacidad de integración permite a las empresas aprovechar las mejores soluciones y crear un ecosistema unificado que mejora la eficiencia operativa y la experiencia del cliente.

**Preparado para el futuro:** El comercio compuesto ofrece a las empresas la flexibilidad de adaptar y desarrollar su plataforma de comercio electrónico a medida que cambian la tecnología y las tendencias del mercado. Permite a las empresas añadir o reemplazar fácilmente componentes, integrar nuevas tecnologías y adelantarse a la competencia. Esta capacidad de cara al futuro garantiza que las empresas puedan innovar continuamente y satisfacer las expectativas cambiantes de los clientes.

**Empoderamiento de desarrolladores:** El comercio de composición empodera a los desarrolladores al proporcionarles la flexibilidad para trabajar con diferentes tecnologías, lenguajes y marcos. Permite a los desarrolladores centrarse en componentes o microservicios específicos, lo que permite la especialización y la experiencia. Esta capacitación conduce a una mayor productividad del desarrollador, ciclos de desarrollo más rápidos y la capacidad de aprovechar las herramientas y tecnologías más recientes.

En general, el comercio componible permite a las empresas crear una plataforma de comercio electrónico altamente flexible, escalable y personalizable que se puede adaptar a las cambiantes necesidades comerciales, ofrecer experiencias de cliente excepcionales e impulsar el crecimiento y el éxito en el mercado digital.

## Qué capacidades tiene Adobe Commerce para el comercio componible

Adobe Commerce proporciona varias funciones para ayudar a los comerciantes a adoptar e implementar el comercio componible:

**Metodología de composición de Commerce:** Adobe Commerce ofrece una metodología de composición de comercio que guía a los comerciantes en la comprensión e implementación de los principios de la arquitectura de composición. Esta metodología ayuda a las empresas a aprovechar las ventajas de la flexibilidad, la escalabilidad y la personalización, a la vez que tiene en cuenta factores como la complejidad, la madurez técnica y el tamaño del proyecto.

**Funcionalidad enriquecida con características:** Adobe Commerce proporciona un conjunto completo de características accesibles a través de su GraphQLAPI. Esta funcionalidad enriquecida en funciones permite a los comerciantes minimizar el número de proveedores necesarios en su pila comercial, lo que reduce los desafíos de tiempo de salida al mercado. Permite a las empresas iniciarse más rápido, manteniendo al mismo tiempo la flexibilidad para componer e integrar servicios o capacidades adicionales de terceros a medida que evoluciona su pila comercial.

**Experiencias front-end híbridas:** Adobe Commerce admite experiencias front-end sin encabezado y sin encabezado dentro del mismo ecosistema. Esta flexibilidad permite a los comerciantes elegir el enfoque arquitectónico más adecuado para cada caso de uso front-end específico. Permite una transición gradual a una arquitectura sin encabezado y componible sin necesidad de una migración simultánea de todo el sistema.

**Malla de API:** La malla de API de Adobe Commerce simplifica la integración de varios microservicios, herramientas de terceros y aplicaciones en un extremo de API unificado para desarrolladores de front-end. Permite a los desarrolladores combinar varias fuentes de datos en un único extremo de GraphQL, lo que reduce la complejidad y optimiza el desarrollo y mantenimiento de nuevas funciones y experiencias.

**Adobe App Builder:** Adobe App Builder es una plataforma de extensibilidad sin servidor que permite a los comerciantes crear microservicios personalizados, crear experiencias personalizadas y ampliar soluciones de Adobe. Con App Builder, los comerciantes pueden crear aplicaciones seguras y escalables que amplíen la funcionalidad nativa de Adobe Commerce e integren soluciones de terceros. Esto elimina la necesidad de que los comerciantes creen y mantengan su propia infraestructura para las personalizaciones y los microservicios, lo que reduce la complejidad y el coste total de propiedad.

Estas funciones proporcionadas por Adobe Commerce simplifican la adopción e implementación del comercio de composición, lo que permite a los comerciantes aprovechar las ventajas de la flexibilidad, la escalabilidad, la personalización y las capacidades de integración, a la vez que reducen la complejidad y el esfuerzo de desarrollo.

## Pensamientos finales

El comercio de composición es un enfoque arquitectónico en el comercio electrónico que implica desacoplar la capa de presentación del front-end de la funcionalidad de comercio del back-end. Estas son las lecciones clave aprendidas sobre el comercio componible:

**Arquitectura:** El comercio de composición sigue una arquitectura modular y desacoplada, lo que permite a las empresas seleccionar y combinar los mejores componentes o microservicios para crear una solución personalizada. Esta arquitectura proporciona flexibilidad, escalabilidad y agilidad.

**Ventajas:** El comercio de composición ofrece varias ventajas, entre las que se incluyen la flexibilidad y la personalización, la escalabilidad y la agilidad, las capacidades de integración, la protección del futuro y la habilitación para desarrolladores. Permite a las empresas adaptarse, ampliar, integrar, innovar y aprovechar la experiencia del desarrollador.

**Consideraciones:** Al considerar el comercio componible, se deben evaluar cuidadosamente factores como la complejidad, la madurez técnica interna, el tamaño y la estructura del proyecto, la personalización frente a la estandarización, el costo total de propiedad y la seguridad y la privacidad de datos.

**Adobe Commerce:** Adobe Commerce proporciona funciones y herramientas para ayudar a los comerciantes a adoptar e implementar el comercio componible. Estas incluyen una metodología de comercio componible, funcionalidad enriquecida en funciones, experiencias front-end híbridas, API Mesh para integración y Adobe App Builder para microservicios personalizados.

**Impacto en la empresa:** El comercio compuesto permite a las empresas crear una plataforma de comercio electrónico altamente flexible, escalable y personalizable. Esto les permite ofrecer experiencias de cliente únicas, escalarlas en función de la demanda, integrarlas con otros sistemas, garantizar el futuro de sus operaciones y aprovechar la experiencia del desarrollador.

Comprender el comercio de composición es crucial para que las empresas del sector del comercio electrónico sigan siendo competitivas, se adapten a las cambiantes condiciones del mercado y ofrezcan experiencias de cliente excepcionales. Al adoptar el comercio compuesto, las empresas pueden desbloquear los beneficios de la flexibilidad, escalabilidad, personalización, agilidad y capacidades de integración, impulsando el crecimiento y el éxito en el mercado digital.

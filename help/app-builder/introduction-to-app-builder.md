---
title: Extensibilidad fuera de proceso para Adobe Commerce
description: Obtenga información sobre App Builder de Adobe y por qué es un aspecto importante de la extensibilidad fuera de proceso.
landing-page-description: Descubra qué es App Builder y cómo puede ayudarle con las estrategias de desarrollo de Adobe Commerce.
short-description: Descubra qué es App Builder y cómo puede ayudarle con las estrategias de desarrollo de Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 5%

---

# Introducción al Generador de aplicaciones

Históricamente, el desarrollo de Adobe Commerce ha utilizado la extensibilidad en proceso. El modelo en proceso requiere que cualquier código nuevo sea compatible con las actualizaciones, la versión PHP del servidor y muchas otras aplicaciones y servicios de servidor esenciales que utiliza Commerce. El Generador de aplicaciones de Adobe Developer utiliza la extensibilidad fuera de proceso para evitar estos problemas de compatibilidad.

## Generador de aplicaciones para Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder es una plataforma de extensibilidad sin servidor para integrar y crear experiencias personalizadas para ampliar las soluciones de Adobe y ya está disponible para Adobe Commerce. Con App Builder, puede crear aplicaciones seguras y escalables que amplíen la funcionalidad nativa de Commerce e integren soluciones de terceros. Como desarrollador, ahora puede aprovechar la extensibilidad fuera de proceso con Adobe Commerce y eso, a su vez, proporciona beneficios inmediatos y a largo plazo.

App Builder proporciona un marco de trabajo de extensibilidad unificado de terceros para integrar y crear aplicaciones personalizadas que amplíen [!DNL Adobe Commerce]. Dado que este marco de trabajo de extensibilidad se basa en la infraestructura de Adobe, los desarrolladores pueden crear microservicios personalizados, así como ampliar e integrar [!DNL Adobe Commerce] en otras soluciones de Adobe e integraciones de terceros.

App Builder permite a los clientes ampliar [!DNL Adobe Commerce] en varios casos de uso:

* Extensibilidad de middleware: conecte sistemas externos con aplicaciones de Adobe mediante la creación de conectores personalizados o aproveche un conjunto de integraciones creadas previamente.
* Extensibilidad de los servicios principales: amplíe las funciones de la aplicación principal ampliando el comportamiento predeterminado con funciones personalizadas y lógica empresarial.
* extensibilidad de la experiencia del usuario: amplíe la experiencia principal para satisfacer los requisitos empresariales o cree propiedades digitales, tiendas y aplicaciones de back-office específicas del cliente.

Adobe Developer App Builder es una solución basada en la nube, lo que significa que se adapta automáticamente. Este servicio también está distribuido globalmente para permitir el mejor rendimiento independientemente de su ubicación geográfica.

## ¿Por qué debería obtener más información sobre App Builder?

Dado que Adobe Commerce no es un producto SAAS completo, el código que desarrolle puede añadir complejidad y problemas de actualización. Al utilizar la extensibilidad fuera de proceso, como el Generador de aplicaciones, puede proporcionar una funcionalidad personalizada y única a su tienda de Adobe Commerce sin requerir métodos en proceso.

Otros beneficios incluyen:

* Las funciones disociadas permiten un tiempo de inicio más rápido.
* Las actualizaciones ahora son más sencillas. Las funciones personalizadas están fuera de la base de código de Commerce, lo que evita problemas de compatibilidad al actualizar.
* Mover funciones y lógica fuera de Commerce libera recursos que normalmente utilizan los métodos de desarrollo en proceso.

## Arquitectura {#architecture}

En lugar de una solución predeterminada, Adobe Developer App Builder proporciona una plataforma de desarrollo común, coherente y estandarizada para ampliar las soluciones de Adobe Cloud como Adobe Commerce, que incluye:

* La consola de Adobe Developer se utiliza para el desarrollo personalizado de microservicios y extensiones. Cree y administre proyectos al acceder a todas las herramientas y API necesarias para crear complementos e integraciones.
* Herramientas, SDK y bibliotecas de código abierto para crear extensiones e integraciones personalizadas. Utilice React Spectrum (Kit de herramientas de IU de Adobe) para tener una IU común para todas las aplicaciones de Adobe.
* servicios como I/O Runtime para alojar infraestructura en la plataforma sin servidor de Adobe y Eventos de E/S para integraciones basadas en eventos. El Adobe también es compatible de serie con el almacenamiento de datos y archivos.
* Adobe Experience Cloud donde se envían extensiones e integraciones para publicarlas en la organización de Experience Cloud. Los administradores de sistemas pueden revisar, administrar y aprobar estas extensiones. Una vez publicadas, las extensiones y herramientas personalizadas del App Builder estarán disponibles junto con otras aplicaciones de Adobe Experience Cloud.

El diagrama siguiente ilustra cómo una aplicación estándar creada en el Generador de aplicaciones utiliza estas funcionalidades:

![Arquitectura](/help/assets/app-builder/app-builder-architecture.jpeg)

Para obtener más información sobre la arquitectura del Generador de aplicaciones, consulte la [Descripción general de arquitectura](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Extensión de Sales Channel de Amazon {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>La extensión de Sales Channel de Amazon aún está en desarrollo y no se ha publicado oficialmente.  Estos vídeos y tutoriales están pensados para mostrarle cómo utilizar el Generador de aplicaciones de Adobe Developer para obtener un caso de uso práctico.

Los siguientes tutoriales muestran cómo conectar Adobe Commerce a la Sales Channel de Amazon mediante una extensión de App Builder.

* [resumen técnico App Builder](../app-builder/app-builder-technical-overview.md)
* [marco de extensibilidad](../app-builder/extensibility-framework-commerce-eventing.md)
* [demostración funcional del Generador de aplicaciones](../app-builder/app-builder-functional-demonstration.md)

## Introducción al Generador de aplicaciones {#additional-resources}

Una descripción general de la estrategia de comercio componible, que incluye la configuración inicial se puede encontrar leyendo la siguiente publicación de blog:

[Cómo App Builder ayuda a impulsar la agilidad empresarial en su plataforma de comercio](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Para empezar a usar el Generador de aplicaciones, Adobe ha creado la siguiente documentación:

* [Introducción al Generador de aplicaciones](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Continuar aprendiendo con la documentación {#appbuilder-documentation}

App Builder proporciona vídeos y documentación para desarrolladores de, incluidas guías y documentación de referencia para ayudarle a desarrollar sus propias aplicaciones personalizadas:

* [Documentación del Generador de aplicaciones](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Vídeos del Generador de aplicaciones](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Pruebe una de las aplicaciones de ejemplo {#appbuilder-codesamples}

¿Listo para empezar a desarrollar? El siguiente vínculo contiene aplicaciones de ejemplo para ayudarle a empezar:

* [App Builder Code Labs en el sitio web de Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Asistencia {#support}

Para solicitudes de soporte de desarrolladores, utilice el [foro de Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} para obtener asistencia.

{{$include /help/_includes/app-builder-related-links.md}}

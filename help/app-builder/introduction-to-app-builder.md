---
title: Capacidad de extensión fuera de proceso para Adobe Commerce
description: Obtenga información sobre Adobe de App Builder y por qué es un aspecto importante de la extensibilidad fuera de proceso.
landing-page-description: Descubra qué es App Builder y cómo puede ayudarle con las estrategias de desarrollo de Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-24T00:00:00Z
source-git-commit: 228891b0e4b56bc2f7d6a3b1dc259b67403ddf51
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---


# Capacidad de extensión fuera de proceso

Históricamente, el desarrollo de Adobe Commerce ha utilizado la extensibilidad en proceso, una característica potente, pero el modelo en proceso requiere que cualquier código nuevo sea compatible con las actualizaciones, la versión PHP del servidor y muchas otras aplicaciones y servicios esenciales de servidor que Commerce utiliza. Adobe Developer App Builder utiliza la extensibilidad fuera de proceso para evitar estos problemas de compatibilidad.

## Creador de aplicaciones para Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder es una plataforma de extensibilidad sin servidor para integrar y crear experiencias personalizadas con el fin de ampliar las soluciones de Adobe, y ya está disponible para Adobe Commerce. Con App Builder, puede crear aplicaciones seguras y escalables que amplíen la funcionalidad nativa del comercio e integrarlas con soluciones de terceros. Como desarrollador, ahora puede utilizar la extensibilidad fuera de proceso con Adobe Commerce y que, a su vez, proporciona beneficios inmediatos y a largo plazo.

App Builder proporciona un marco de extensibilidad unificado de terceros para integrar y crear aplicaciones personalizadas que se extienden [!DNL Adobe Commerce]. Dado que este marco de extensibilidad se basa en la infraestructura de Adobe, los desarrolladores pueden crear microservicios personalizados y ampliarlos e integrarlos [!DNL Adobe Commerce] en otras soluciones de Adobe e integraciones de terceros.

App Builder permite a los clientes ampliar [!DNL Adobe Commerce] en varios casos de uso:

* extensibilidad de middleware : conecte sistemas externos con aplicaciones de Adobe mediante la creación de conectores personalizados o aproveche un conjunto de integraciones prediseñadas.
* extensibilidad de los servicios principales : amplíe las capacidades de las aplicaciones principales ampliando el comportamiento predeterminado con funciones personalizadas y lógica empresarial.
* extensibilidad de la experiencia del usuario : amplíe la experiencia principal para satisfacer los requisitos comerciales o genere propiedades digitales, tiendas y aplicaciones de back-office específicas del cliente.

App Builder (anteriormente conocido como Project Firefly) es una solución basada en la nube, lo que significa que se escala automáticamente. Este servicio también se distribuye globalmente para permitir el mejor rendimiento independientemente de su ubicación geográfica.

## Por qué debería obtener más información sobre App Builder

Como Adobe Commerce no es un producto SAAS completo, el código que desarrolle puede añadir complejidad y problemas de actualización. Al utilizar la extensibilidad fuera de proceso, como App Builder, puede proporcionar funcionalidad personalizada y única a su tienda de Adobe Commerce sin necesidad de métodos en proceso.

Otros beneficios incluyen:

* Las funciones desactivadas permiten un inicio más rápido.
* Las actualizaciones ahora son más sencillas. Las funciones personalizadas están fuera de la base de código de comercio, lo que evita problemas de compatibilidad al actualizar.
* Mover las funciones y la lógica fuera de Commerce libera recursos que normalmente utilizan los métodos de desarrollo en proceso.

## Arquitectura {#architecture}

En lugar de una solución lista para usar, Adobe Developer App Builder proporciona una plataforma de desarrollo común, coherente y estandarizada para ampliar las soluciones de Adobe Cloud, como Adobe Commerce, que incluye:

* La consola de Adobe Developer se utiliza para el microservicio personalizado y el desarrollo de extensiones. Cree y administre proyectos mientras accede a todas las herramientas y API necesarias para crear complementos e integraciones.
* Herramientas de código abierto, SDK y bibliotecas para crear integraciones y extensiones personalizadas. Utilice React Spectrum (el kit de herramientas de la interfaz de usuario del Adobe) para tener una IU común para todas las aplicaciones de Adobe.
* servicios como I/O Runtime para alojar la infraestructura en la plataforma sin servidor de Adobe y eventos de E/S para integraciones basadas en eventos. Adobe también proporciona soporte para almacenar datos y archivos de forma predeterminada.
* Adobe Experience Cloud donde envía extensiones e integraciones para publicarlas en su organización de Experience Cloud. Los administradores del sistema pueden revisar, administrar y aprobar estas extensiones. Una vez publicadas, las herramientas y extensiones personalizadas de App Builder están disponibles junto con otras aplicaciones de Adobe Experience Cloud.

El diagrama siguiente ilustra cómo una aplicación estándar creada en App Builder utiliza estas funcionalidades:

![Arquitectura](/help/assets/app-builder/firefly-architecture.jpeg)

Para obtener más información sobre la arquitectura de App Builder, consulte la [Información general sobre la arquitectura](https://developer.adobe.com/app-builder/docs/guides/).

## Extensión de Sales Channel de Amazon {#amazon-sales-channel-extension}

Los siguientes tutoriales muestran cómo conectar Adobe Commerce a la Sales Channel de Amazon mediante una extensión de App Builder.

* [información general técnica App Builder](../app-builder/app-builder-technical-overview.md)
* [marco de extensibilidad](../app-builder/extensibility-framework-commerce-eventing.md)
* [demostración funcional App Builder](../app-builder/app-builder-functional-demonstration.md)

## Introducción a App Builder {#additional-resources}

Para ayudarle a empezar con App Builder, Adobe ha creado la siguiente documentación:

* [Introducción a App Builder](https://developer.adobe.com/app-builder/docs/getting_started/)

## Continuar aprendiendo con la documentación {#appbuilder-documentation}

App Builder proporciona vídeos y documentación para desarrolladores, incluidas guías y documentación de referencia para ayudar a desarrollar sus propias aplicaciones personalizadas:

* [Documentación de App Builder](https://developer.adobe.com/app-builder/docs/overview/)
* [Vídeos de App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## Pruebe una de las aplicaciones de ejemplo {#appbuilder-codesamples}

¿Listo para empezar a desarrollarse? El siguiente vínculo contiene aplicaciones de ejemplo para ayudarle a empezar:

* [Etiquetas de código de App Builder en el sitio web de Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/)

## Asistencia {#support}

Para las solicitudes de asistencia al desarrollador, utilice la variable [Foro de Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) para obtener ayuda.

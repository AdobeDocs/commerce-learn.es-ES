---
title: Extensibilidad fuera de proceso para Adobe Commerce
description: Obtenga información acerca de Adobe Developer App Builder y cómo la extensibilidad fuera de proceso le permite crear extensiones de Commerce seguras y escalables sin problemas de compatibilidad dentro del proceso.
jira: KT-11433
doc-type: Tutorial
duration: 300
last-substantial-update: 2023-02-16T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
TQID: https://experienceleague.adobe.com/c3dl6gZ7Jtje5rZCB9HrwmCbFrcu8bllL0z0B9cl5Jg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: 761
ht-degree: 0%

---

# Introducción a App Builder

Históricamente, el desarrollo de Adobe Commerce ha utilizado la extensibilidad en proceso. El modelo en proceso requiere que cualquier código nuevo sea compatible con las actualizaciones, la versión PHP del servidor y muchas otras aplicaciones y servicios de servidor esenciales que utiliza Commerce. Adobe Developer App Builder utiliza la extensibilidad fuera de proceso para evitar estos problemas de compatibilidad.

## App Builder para Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?learn=on)

Adobe Developer App Builder es una plataforma de extensibilidad sin servidor para integrar y crear experiencias personalizadas para ampliar las soluciones de Adobe y ya está disponible para Adobe Commerce. Con App Builder, puede crear aplicaciones seguras y escalables que amplíen la funcionalidad nativa de Commerce e integren soluciones de terceros. Como desarrollador, ahora puede aprovechar la extensibilidad fuera de proceso con Adobe Commerce y eso, a su vez, proporciona beneficios inmediatos y a largo plazo.

App Builder proporciona un marco de trabajo de extensibilidad unificado de terceros para integrar y crear aplicaciones personalizadas que extienden [!DNL Adobe Commerce]. Dado que este marco de extensibilidad se basa en la infraestructura de Adobe, los desarrolladores pueden generar microservicios personalizados, y extender e integrar [!DNL Adobe Commerce] en otras soluciones de Adobe y en integraciones de terceros.

App Builder proporciona una forma para que los clientes amplíen [!DNL Adobe Commerce] en varios casos de uso:

* Extensibilidad de middleware: conecte sistemas externos con aplicaciones de Adobe mediante la creación de conectores personalizados o aproveche un conjunto de integraciones creadas previamente.
* Extensibilidad de los servicios principales: amplíe las funciones de la aplicación principal ampliando el comportamiento predeterminado con funciones personalizadas y lógica empresarial.
* extensibilidad de la experiencia del usuario: para satisfacer los requisitos comerciales o crear propiedades digitales, tiendas y aplicaciones de back-office específicas del cliente, amplíe la experiencia principal.

Adobe Developer App Builder es una solución basada en la nube, lo que significa que se escala automáticamente. Este servicio también está distribuido globalmente para permitir el mejor rendimiento independientemente de su ubicación geográfica.

## Por qué obtener más información sobre App Builder

Dado que Adobe Commerce no es un producto SAAS completo, el código que desarrolle puede añadir complejidad y consideraciones de actualización. Al utilizar la extensibilidad fuera de proceso, como App Builder, puede proporcionar una funcionalidad personalizada y única a su tienda de Adobe Commerce sin requerir métodos en proceso.

Otros beneficios incluyen:

* Las funciones disociadas permiten un tiempo de inicio más rápido.
* Las actualizaciones ahora son más sencillas. Las funciones personalizadas están fuera del código base de Commerce, lo que evita problemas de compatibilidad al actualizar.
* Mover características y lógica fuera de Commerce libera recursos que los métodos de desarrollo en proceso suelen utilizar.

## Arquitectura {#architecture}

En lugar de una solución preconfigurada, Adobe Developer App Builder proporciona una plataforma de desarrollo común, coherente y estandarizada para ampliar las soluciones de Adobe Cloud como Adobe Commerce, que incluyen:

* Adobe Developer Console se utiliza para el microservicio personalizado y el desarrollo de extensiones. Cree y administre proyectos al acceder a todas las herramientas y API necesarias para crear complementos e integraciones.
* Herramientas, SDK y bibliotecas de código abierto para crear extensiones e integraciones personalizadas. Utilice React Spectrum (Kit de herramientas de IU de Adobe) para tener una IU común para todas las aplicaciones de Adobe.
* servicios como I/O Runtime para alojar infraestructura en la plataforma sin servidor de Adobe y Eventos de E/S para integraciones basadas en eventos. Adobe también proporciona compatibilidad preconfigurada para almacenar datos y archivos.
* Adobe Experience Cloud, donde se envían extensiones e integraciones para publicarlas en su organización de Experience Cloud. Los administradores de sistemas pueden revisar, administrar y aprobar estas extensiones. Una vez publicadas, las extensiones y herramientas personalizadas de App Builder estarán disponibles junto con otras aplicaciones de Adobe Experience Cloud.

El diagrama siguiente ilustra cómo una aplicación estándar creada en App Builder utiliza estas funcionalidades:

![Arquitectura](/help/assets/app-builder/app-builder-architecture.jpeg)

Para obtener más información acerca de la arquitectura de App Builder, consulte [Descripción general de la arquitectura](https://developer.adobe.com/app-builder/docs/guides/app_builder_guides/architecture_overview/architecture-overview){target="_blank"}.

## Introducción a App Builder {#additional-resources}

Una descripción general de la estrategia de comercio componible que incluye la configuración inicial se puede encontrar leyendo la siguiente publicación de blog:

[Cómo App Builder ayuda a impulsar la agilidad empresarial en su plataforma comercial](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Para ayudarle a empezar a usar App Builder, Adobe ha creado la siguiente documentación:

* [Introducción a App Builder](https://developer.adobe.com/app-builder/docs/get_started/app_builder_get_started/first-app){target="_blank"}

## Continuar aprendiendo con la documentación {#appbuilder-documentation}

App Builder proporciona vídeos y documentación para desarrolladores de, incluidas guías y documentación de referencia para ayudarle a desarrollar sus propias aplicaciones personalizadas:

* [Documentación de App Builder](https://developer.adobe.com/app-builder/docs/intro_and_overview){target="_blank"}
* [Vídeos de App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Pruebe una de las aplicaciones de ejemplo {#appbuilder-codesamples}

¿Listo para empezar a desarrollar? El siguiente vínculo contiene aplicaciones de ejemplo para ayudarle a empezar:

* [App Builder Code Labs en el sitio web de Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

{{$include /help/_includes/app-builder-related-links.md}}

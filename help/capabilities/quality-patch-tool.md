---
title: Herramienta Parche de calidad
description: Aprenda a utilizar la herramienta de parches de calidad cuando diagnostique un problema, encuentre una solución y aplique un parche que se encuentre en la lista de parches disponibles.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00.000Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
TQID: https://experienceleague.adobe.com/GpcJqSCn3XqLZtm-QdQ-ka9c-RdkG-C6Hd3FpXrh8-I
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 593
ht-degree: 0%

---

# Herramienta Parche de calidad

Aprenda a utilizar la herramienta de parches de calidad cuando diagnostique un problema, encuentre una solución y aplique un parche que se encuentre en la lista de parches disponibles.

## Qué va a aprender

Aprenda a clasificar un problema y, a continuación, utilice algunas técnicas básicas para encontrar un parche de calidad al que aplicar una corrección.

## Público

* Los desarrolladores aprenden a encontrar problemas y aprovechar esta herramienta para aplicar parches GIT para problemas conocidos

## Contenido de vídeo

La herramienta Parches de calidad es una utilidad de línea de comandos para Adobe Commerce y Magento Open Source. Esto es lo que le permite hacer:

* Vea información general sobre los parches de calidad más recientes.
* Aplique parches de calidad a la instalación.
* Revertir parches aplicados si es necesario

Estos parches han sido desarrollados por desarrolladores de Adobe y la comunidad de Magento Open Source para mejorar la estabilidad y el rendimiento. Tenga en cuenta que no se recomienda para aplicar grandes cantidades de parches, ya que puede complicar futuras actualizaciones.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Por qué utilizar la herramienta parche de calidad

Es posible que desee utilizar la herramienta Parches de calidad para Adobe Commerce o Magento Open Source si desea:

Mejore la estabilidad y el rendimiento: los parches de calidad solucionan los problemas, mejoran la seguridad y optimizan la instalación.
Manténgase al día: la aplicación de parches garantiza que su sistema esté actualizado y protegido.
Revertir cambios: Si un parche causa problemas inesperados, puede revertirlo con la herramienta. Recuerde, es más adecuado para aplicar un número limitado de parches para evitar complicar futuras actualizaciones.  

## Limitaciones o preocupaciones con el uso de la herramienta de parches de calidad

Aunque la herramienta Parches de calidad ofrece ventajas, hay que tener en cuenta algunas consideraciones:

* Compatibilidad: Asegúrese de que los parches sean compatibles con su versión específica de Adobe Commerce o Magento Open Source.
* Prueba: Pruebe siempre los parches en un entorno de ensayo antes de aplicarlos a la producción. Pueden surgir problemas inesperados.
* Dependencias de Parches: Algunos parches pueden depender de otros. Tenga en cuenta cualquier requisito previo.
* Personalizaciones: si ha realizado cambios en el código personalizado, los parches pueden entrar en conflicto. Revise detenidamente los cambios.
* Copia de seguridad: Realice una copia de seguridad de la instalación antes de aplicar los parches para evitar la pérdida de datos.

Aunque la herramienta Parches de calidad es útil para aplicar un número limitado de parches, no se recomienda para manejar un gran volumen de parches. La aplicación de demasiados parches puede complicar las futuras actualizaciones y tareas de mantenimiento. Si tiene que aplicar numerosos parches, considere enfoques alternativos o consúltelo con un especialista de Magento. 

## Resumen

La herramienta Parches de calidad permite a las plataformas de comercio electrónico mejorar la estabilidad y la seguridad mediante la aplicación de parches. Estos parches solucionan problemas, mejoran el rendimiento y optimizan el sistema. Mantener la instalación actualizada garantiza la protección contra vulnerabilidades.

Antes de aplicar parches, es crucial probarlos en un entorno de ensayo. Asegúrese de que sea compatible con su versión específica de Adobe Commerce o Magento Open Source. Algunos parches pueden tener dependencias, por lo que debe revisar cuidadosamente los requisitos previos.

 Realice una copia de seguridad de la instalación antes de aplicar los parches para evitar la pérdida de datos. Si ha realizado cambios en el código personalizado, tenga en cuenta que los parches pueden entrar en conflicto. Siga las prácticas recomendadas y monitorice el impacto de cada parche.

## Artículos y vídeos relacionados

* [Búsqueda de herramientas de parches de calidad](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [Notas de la versión](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github para parches](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Uso de la herramienta de parche de calidad](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Vídeo técnico sobre QPT](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)

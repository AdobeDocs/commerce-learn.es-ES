---
title: Optimización de la reutilización del código con Adobe Commerce
description: Aprenda a optimizar la reutilización del código en Adobe Commerce con patrones de arquitectura de referencia global, mejorando el rendimiento y la conformidad en varias instancias.
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Colaboró Tony Evers, arquitecto técnico senior, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Colaboró Tony Evers"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# Técnicas de implementación de arquitectura de referencia global

Existen varias formas de optimizar la reutilización del código con Adobe Commerce. Estas cuatro técnicas de implementación tienen sus propias ventajas. Los ejemplos de este artículo se ordenan de simples a más complejos. Elija la estrategia que mejor se adapte a su proyecto y a la hoja de ruta futura. La migración de una estrategia a otra puede llevar mucho tiempo.

## Cuándo utilizar la arquitectura de referencia global

La arquitectura de referencia global puede ser útil, según el número de instancias que tenga. Una instancia de es una instalación independiente de Adobe Commerce que utiliza su propia base de datos. Haga un recuento de las bases de datos de producción para saber cuántas instancias tiene. Si mantiene más de una instancia de o si prevé este escenario en el futuro, puede beneficiarse de la arquitectura de referencia global. Cuanta más funcionalidad compartan las instancias, más valor añadirá una arquitectura de referencia global.

En cualquiera de estos casos, es aconsejable explorar con varias instancias de Adobe Commerce.

1. **Diferentes propietarios de tiendas**: Si mantiene código para varios propietarios de tiendas, cada uno con su propio almacén, pueden ser necesarias instancias independientes para mantener sus requisitos individuales de forma eficaz.
2. **Cumplimiento de las regulaciones nacionales**: ciertas regulaciones exigen que los datos de los clientes se almacenen dentro de regiones específicas. En tales casos, es esencial contar con instancias separadas para asegurar el cumplimiento de estas regulaciones.
3. **Desviaciones operativas entre regiones geográficas**: Operar en varias regiones puede significar diferentes calendarios y requisitos de mantenimiento. El uso de instancias independientes permite flexibilidad para administrar estas variaciones de forma eficaz.
4. **Ventas de Flash de alta intensidad**: Las tiendas que realizan ventas flash a gran escala a menudo requieren un rendimiento de servidor optimizado. La infraestructura dedicada proporcionada por instancias independientes garantiza un rendimiento óptimo durante estos períodos de alta demanda.
5. **Diferencias significativas entre marcas o países**: cuando la diferencia entre marcas o países es grande, el uso de una sola instancia genera código que solo se utiliza para algunas marcas o países. Las instancias independientes pueden mejorar el rendimiento y la estabilidad al eliminar el código innecesario para las marcas y los países que no lo necesitan.

## Patrones de arquitectura de referencia global

Junto a &quot;sin patrón GRA&quot; hay cuatro estilos de patrones GRA.

![5 iconos de patrones de GRA: sin GRA, split, bulk, distinct y monorepo.](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### No hay patrón GRA

![Icono que muestra &quot;sin GRA&quot;](/help/assets/global-reference-architecture/no-gra.png){align="center"}

Cuando no se utiliza un patrón GRA, cada instancia de Adobe Commerce es una aplicación única. No se puede reutilizar el código, excepto moviéndolo manualmente de una instancia a otra. Estas copias siempre divergen. La cantidad de esfuerzo para garantizar que cada instancia tenga los mismos cambios pero funcione según lo esperado puede ser abrumadora. En esta situación, tres instancias necesitan el triple de esfuerzo de mantenimiento que una instancia.

![Diagrama que muestra 3 almacenes, donde cada uno es una copia del primero, con un desarrollo único en las 3 copias.](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### El patrón Git GRA dividido

![Un icono que representa el patrón de GRA &quot;dividido&quot;](/help/assets/global-reference-architecture/split-git.png){align="center"}

Este patrón consiste en repositorios Git para desarrollo y un repositorio Git por instancia. Cada archivo de la instancia se mantiene en uno de los repositorios de desarrollo. Se juntan como una trenza formando todo el GRA. Cada línea de código solo existe en un único repositorio de desarrollo y se instala en las instancias mediante la técnica de entrelazado, lo que provoca la reutilización del código.

![Diagrama que muestra dónde se almacena el código en un patrón GRA dividido](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### El patrón GRA de paquetes masivos

![Un icono que representa el patrón GRA &quot;masivo&quot;](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Los módulos principales de Adobe Commerce y de terceros se instalan directamente a través de los repositorios de Composer. Los repositorios Git se pueden usar como repositorios Composer. En este patrón, toda la base de código compartido de GRA se aloja en uno o pocos repositorios Git y se instala a través de Composer. La característica clave es que varios módulos, paquetes de idiomas o temas se alojan en un solo paquete de compositor, lo que simplifica el desarrollo.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de paquetes masivos](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### El patrón GRA de paquetes separados

![Un icono que representa el patrón de GRA &quot;paquetes separados&quot;](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

Cada módulo, paquete de idioma o tema de Adobe Commerce se instala como un paquete de compositor independiente. Cada personalización tiene su propio repositorio de Git. Esto significa la máxima flexibilidad en la composición de las instancias y dispone de una gestión de dependencias del Compositor fiable. Para optimizar el rendimiento, todos los paquetes se duplican en un único repositorio privado de Composer.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de paquetes independiente](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### El patrón de monorepo GRA

![Un icono que representa el patrón de GRA &quot;monorepo&quot;](/help/assets/global-reference-architecture/monorepo.png){align="center"}

Todo el desarrollo tiene lugar en un único repositorio de código. La automatización genera paquetes para las nuevas versiones y los publica en un repositorio del compositor. El patrón combina la baja sobrecarga de desarrollo del enfoque de paquetes masivos con la flexibilidad del enfoque de paquetes separados. El patrón monorepo también es ideal para ejecutar pruebas funcionales automatizadas.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Elección de un patrón GRA

La elección de un patrón GRA se realiza mediante la evaluación de la complejidad del proyecto, la necesidad de flexibilidad y la capacidad del equipo de desarrollo para adaptarse.

Los equipos con poca experiencia en Adobe Commerce deberían empezar de forma sencilla. Sin embargo, si el proyecto exige un patrón de GRA más complejo debido a sus características, no comprometa.

Características comunes del proyecto relacionadas con cada patrón:

1. **Sin patrón GRA**: instancia única de Adobe Commerce sin planes de extensión. Varias instancias de Adobe Commerce con un mínimo de elementos comunes entre ellas.

2. **Patrón Split Git GRA**: Los equipos que desean evitar Composer para sus personalizaciones, en la mayoría de los casos el patrón Bulk packages es el preferido para Split Git.

3. **Patrón GRA de paquete masivo**: base de código de personalización con alta interdependencia. Todas las instancias tienen combinaciones muy similares de paquetes personalizados. No hay promociones ni descensos frecuentes de paquetes individuales. Equipos con poca experiencia en gestión de código y con necesidad de simplicidad.

4. **Patrón GRA de paquetes independientes**: se necesita una administración flexible del ámbito de lanzamiento. Se prevén 50 paquetes personalizados o menos en los próximos 5 años. Capas globales y regionales de código común. No hay planes para migrar a un patrón de Monorepo. El equipo es técnicamente experto y tiene una estricta adherencia al proceso.

5. **Patrón GRA de Monorepo**: todas las características del patrón GRA de paquetes separados. Se prevén más de 50 paquetes en los próximos 5 años. Necesidad de realizar pruebas automatizadas exhaustivas. Soporte para ambientes efímeros. Base de código complejo a escala empresarial que debe ampliarse sin aumentar el coste de mantenimiento a la misma velocidad.

### El coste de una elección incorrecta

La migración de un patrón a otro es posible. El diagrama siguiente muestra el nivel de impacto de pasar de un patrón a otro. Las líneas verdes muestran un bajo impacto, las amarillas y ámbar muestran migraciones de moderadamente complejas a complejas.

![Diagrama que muestra flechas de color entre los 4 patrones de GRA, indicando el nivel de dificultad para moverse de uno a otro.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}

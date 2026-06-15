---
title: Razones por las que existe el modelo de datos de catálogo componible (CCDM)
description: Descubra cómo CCDM mantiene un catálogo unificado mientras las tiendas reciben productos, precios y reglas filtrados mediante vistas de catálogo y servicios de comercialización.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 259
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-18624
source-git-commit: bfe282e4f1ef04985cffb109bce90bc05a70fda0
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# Por qué existe el modelo de datos de catálogo maquetable

Los equipos comerciales modernos suelen vender en **marcas**, **regiones**, **concesionarios** y **canales digitales**. Cuando cada canal conserva su propia copia del catálogo, los equipos invierten más tiempo en reconciliar los SKU, los precios y la disponibilidad que en mejorar la experiencia del comprador. El **Modelo de datos de catálogo compuesto de Adobe (CCDM)** detrás de **Adobe Commerce Optimizer** está diseñado para invertir ese patrón: **un catálogo unificado** en una capa SaaS, con **vistas de catálogo** y **políticas** que determinan lo que cada tienda o integración puede ver.

## ¿Para quién es este vídeo?

* Los arquitectos y desarrolladores de soluciones de Commerce que son nuevos en Adobe Commerce Optimizer y necesitan el contexto empresarial antes de configurar fuentes de catálogo, vistas y directivas

## Contenido de vídeo

* Por qué los catálogos duplicados y específicos de canal crean un riesgo operativo y una innovación más lenta
* Cómo CCDM mantiene unificados los datos de productos al tiempo que admite escenarios de varias marcas y regiones
* Cómo las vistas de catálogo actúan como la &quot;lente&quot; entre un catálogo base compartido y una tienda o audiencia específica
* Cómo consumen esas vistas las API de servicios de comercialización, de modo que las experiencias sin encabezado permanecen alineadas con el catálogo configurado

>[!VIDEO](https://video.tv.adobe.com/v/3491285?learn=on)

## El desafío de los catálogos en silo

Cuando cada sitio de distribución, tienda regional o propiedad de marca mantiene **su propia base de datos de catálogo**, se agravan varios problemas:

* **Duplicación**: el mismo SKU, la descripción y el mismo medio se han introducido muchas veces.
* **Deriva**: las actualizaciones de precios, los nuevos atributos o los artículos interrumpidos aterrizan en un canal, pero no en otros.
* **Lanzamientos más lentos**: cada nuevo punto de contacto repite un trabajo de datos intenso en lugar de reutilizar un único registro de producto.

CCDM existe para que la información del producto pueda estar en **un catálogo maquetable** que otros sistemas enriquecen, mientras que las tiendas siguen recibiendo **surtidos y precios apropiados para el canal**.

## Cambios en el modelo de datos del catálogo maquetable

En Adobe Commerce Optimizer, los datos del producto se **incorporan en un catálogo base unificado** desde una o más **fuentes de catálogo** (por ejemplo, una configuración regional como `en-US` o sistemas ascendentes como PIM o ERP). Esa fuente proporciona los atributos y valores sin procesar.

**Vistas de catálogo** define cómo se **organizan y exponen** esos datos unificados para un contexto empresarial: qué productos pasan sus **políticas**, qué **libros de precios** aplican y qué **fuente de catálogo** respalda la vista. Por lo tanto, los mismos registros subyacentes pueden admitir **muchas proyecciones**, por ejemplo, vistas independientes por distribuidor, región o marca, sin clonar el catálogo completo para cada sitio.

Esta separación (**de dónde provienen los datos** (origen del catálogo) frente a **cómo se presentan** (vista del catálogo)— es la razón principal por la que los equipos adoptan CCDM en lugar de mantener catálogos paralelos por canal.

## Vistas de catálogo como vista de tienda

Como se describe en [Vistas del catálogo para servicios de comercialización](https://experienceleague.adobe.com/es/docs/commerce/optimizer/setup/catalog-view){target="_blank"}, una vista del catálogo se comporta como una **vista**: los compradores solo ven los productos, los precios y las reglas que la vista permite, mientras que el **catálogo base** sigue siendo el sistema de registro compartido. Ese modelo se vincula directamente con **Servicios de comercialización**, de manera que los clientes de API pasan la vista correcta (y los encabezados relacionados) y reciben una respuesta coherente basada en directivas para cada experiencia.

Para ver un tutorial más detallado sobre cómo esas piezas se ajustan a un flujo de extremo a extremo, consulte el tutorial para desarrolladores [Crear un catálogo maquetable para tu tienda](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}.

## Contenido relacionado

* [Más información sobre las Vistas de catálogo](./learn-about-the-ccdm-feature-catalog-views.md)
* [Vistas del catálogo de servicios de comercialización](https://experienceleague.adobe.com/es/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [Crea un catálogo maquetable para tu tienda](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}
* [Guía de [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/es/docs/commerce/optimizer/overview){target="_blank"}
* [Introducción a la API de comercialización](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

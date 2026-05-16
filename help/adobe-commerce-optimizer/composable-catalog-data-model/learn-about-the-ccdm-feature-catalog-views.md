---
title: Obtenga información sobre las vistas de catálogo en el modelo de datos de catálogo maquetable
description: Descubra cómo las vistas de catálogo del Modelo de datos de catálogo compuesto de Adobe (CCDM) asignan un catálogo base a cada tienda con productos, precios y reglas diferentes.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: 96a1356a399fa5cdca9d9befd7c14ebad1b0162f
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Vistas de catálogo en el modelo de datos de catálogo compuesto de Adobe

Las vistas de catálogo son la forma en que se sirve a cada audiencia de forma diferente a un único catálogo maquetable. Este tutorial explica qué es una vista de catálogo, cómo la aplica Adobe Commerce Optimizer a un comprador y por qué un ID de vista único es el anclaje para productos, precios y reglas, utilizando el escenario de demostración de **Carvelo Automobiles**.

## ¿Para quién es este vídeo?

* Los arquitectos y desarrolladores de soluciones de Commerce que modelan catálogos de varias marcas o distribuidores en Adobe Commerce Optimizer

## Contenido de vídeo

* El escenario de Carvelo Automobiles (marcas, concesionarios y acuerdos)
* Qué es una vista de catálogo y la metáfora de &quot;lente&quot; sobre un catálogo base unificado
* Cómo utiliza una tienda una vista de catálogo para filtrar productos y precios (por ejemplo, Celport)
* ID únicos de vista de catálogo y el valor comercial de una única fuente fiable

>[!VIDEO](https://video.tv.adobe.com/v/3491265?captions=spa&learn=on)

## Escenario: Carvelo Automobiles

**Carvelo Automobiles** es una compañía ficticia de autopartes que se usa a menudo en demostraciones, documentación y tutoriales de Adobe Commerce. Carvelo vende piezas en tres marcas: **Aurora**, **Bolt** y **Cruz**, a través de tres concesionarios:

* **Arkbridge** pertenece a West Coast Inc.
* **Kingsbluff** y **Celport** pertenecen a East Coast Inc.

Cada concesionario tiene su propio acuerdo sobre los productos que puede vender. Esto crea una configuración verdaderamente compleja, y aún puede ejecutarse desde **un único catálogo base** de aproximadamente seis millones de SKU. La funcionalidad que lo hace posible es una **vista de catálogo**.

## ¿Qué es una vista de catálogo?

Una **vista de catálogo** es una vista configurada del catálogo para un contexto empresarial específico. Considéralo como un **objetivo**. El catálogo base unificado se encuentra en el centro y contiene todos los SKU de cada marca. Cada concesionario obtiene su propia vista, su propia vista del catálogo.

En el ejemplo:

* El objetivo **Arkbridge** puede mostrar las tres marcas: Aurora, Bolt y Cruz.
* La lente **Celport** muestra solamente un subconjunto de piezas de perno y cruz.

Cada vista de catálogo también puede controlar **price**. Los **datos del catálogo subyacente no cambian**; solo cambian los aspectos visibles (surtido, precio y reglas) para ese contexto.

## Aplicación de una vista de catálogo por Commerce Optimizer

Cuando un comprador visita la tienda **Celport**, Adobe Commerce Optimizer usa la **vista del catálogo de Celport** para determinar exactamente qué productos, precios y reglas se aplican. El comprador solo ve lo que ese lente permite.

Otros productos pueden seguir existiendo en el catálogo, por ejemplo neumáticos Aurora, motores de perno o baterías Cruz, pero la tienda de **Celport nunca los expone** si la vista del catálogo no lo permite.

## ID de vista de catálogo y valor empresarial

Cada vista de catálogo tiene **ID único**. Ese ID es lo que conecta su tienda con la configuración del catálogo. Solo se establece una vez en la configuración de la tienda, y el comportamiento descendente sigue: los productos correctos, los precios correctos y las reglas correctas.

En lugar de mantener catálogos separados para cada distribuidor o marca (y mantenerlos sincronizados), mantienes **un** catálogo maquetable. Las vistas de catálogo le permiten dar forma a **muchas experiencias de tienda diferentes** a partir de esa **única fuente de verdad**.

## Contenido relacionado

* [Guía de [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/es/docs/commerce/optimizer/overview){target="_blank"}
* [Introducción a la API de comercialización](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}

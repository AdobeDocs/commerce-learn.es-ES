---
title: Obtenga información acerca de las directivas de CCDM en el modelo de datos de catálogo maquetable
description: Aprenda cómo las políticas ESTÁTICAS y de DÉCLENCHEUR del modelo de datos de catálogo componible de Adobe controlan la visibilidad del producto en las vistas de catálogo sin reconstruir el catálogo.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 349
last-substantial-update: 2026-05-21T00:00:00Z
jira: KT-21258
source-git-commit: bfe282e4f1ef04985cffb109bce90bc05a70fda0
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Directivas en el modelo de datos de catálogo compuesto de Adobe

Si una **vista de catálogo** es la lente que da forma a lo que los compradores ven desde un catálogo base unificado, las **políticas** son de lo que está hecha esa lente. En este tutorial se explica qué es una directiva, cómo funcionan juntas las directivas **STATIC** y **DÉCLENCHEUR** en el escenario de demostración de **Carvelo Automobiles** y por qué la actualización de una directiva entra en vigor inmediatamente, sin reconstruir el catálogo.

## ¿Para quién es este vídeo?

* Los arquitectos y desarrolladores de soluciones de Commerce que configuran vistas de catálogo y reglas de comercialización en Adobe Commerce Optimizer

## Contenido de vídeo

* Directivas como filtros de acceso a datos en atributos de producto
* Combinación de varias directivas en la vista de catálogo de Celport (marcas y categorías de artículos)
* Políticas STATIC para reglas de negocio permanentes
* Directivas de déclencheur activadas por los encabezados de solicitud de API (por ejemplo, `AC-Policy-Brand`)
* Actualización de directivas en operaciones diarias sin regeneración de catálogos

>[!VIDEO](https://video.tv.adobe.com/v/3491413?learn=on)

Una **directiva** es un **filtro de acceso a datos**. Inspecciona los atributos del producto y aplica reglas que determinan qué productos puede exponer una vista del catálogo. Las directivas se sitúan sobre el catálogo maquetable compartido, no duplican los datos del catálogo.

## Políticas ESTÁTICAS

Una directiva **STATIC** está **siempre activada**. Aplica reglas comerciales permanentes independientemente del comportamiento del comprador o del estado de la sesión.

En el escenario de Celport, las reglas STATIC incluyen:

* Celport solo venderá las marcas **Bolt** y **Cruz**.
* Celport solo mostrará **frenos** y **suspensión** piezas.

Estas reglas nunca se desactivan. Las directivas STATIC son el lugar en el que se aplican los **acuerdos de licencia**, las **restricciones territoriales** y los **permisos de marca**. Configúrelos una vez y Adobe Commerce Optimizer los aplicará automáticamente en cada solicitud.

## políticas de déclencheur

Las directivas con un origen de valor de **DÉCLENCHEUR** a veces se denominan **directivas exclusivas**. La vista de catálogo ejecuta esa directiva **solamente cuando el déclencheur se especifica** en el encabezado de la llamada de API.

Por ejemplo, una tienda de servicios de entrega de experiencias (EDS) puede ofrecer un menú desplegable con **Todas las marcas**, **Aurora**, **Bolt** y **Cruz**:

* En la vista de página inicial no se selecciona nada, por lo que no se envía ningún encabezado de marca adicional.
* Cuando el comprador selecciona **Bolt**, la API subyacente establece el encabezado `AC-Policy-Brand` en `Bolt`.
* Cuando el comprador selecciona **Cruz**, el mismo encabezado se establece en `Cruz`.

La vista de catálogo aplica la directiva de DÉCLENCHEUR solo cuando el encabezado está presente, lo que admite el filtrado interactivo sin cambiar las reglas ESTÁTICAS permanentes.

## ESTÁTICA y DÉCLENCHEUR juntos

Las directivas **STATIC** administran reglas permanentes. Las directivas de **DÉCLENCHEUR** administran directivas interactivas. Utilizados juntos en una sola vista de catálogo, proporcionan **conformidad** y **flexibilidad**, límites de surtido fijos además de refinamiento impulsado por el comprador.

## Actualizar directivas sin volver a crear el catálogo

Para las operaciones diarias, los cambios de directiva son inmediatos. Supongamos que el contrato de licencia de Celport ahora permite **neumáticos**, así como frenos y suspensión:

1. Abra la directiva correspondiente.
1. Agregar **neumáticos** a la lista de valores.
1. Guardar.

Ese cambio es **activo inmediatamente**. No hay reconstrucción del catálogo, reimplementación ni tiempo de espera. La siguiente solicitud a la tienda de Celport refleja la actualización.

Las directivas son filtros ligeros en un **catálogo compartido**, no reglas que se codifican en copias de catálogo independientes. **Cambie la regla, no los datos.**

## Contenido relacionado

* [Por qué existe el modelo de datos de catálogo maquetable](./why-ccdm-exists.md)
* [Más información sobre las Vistas de catálogo](./learn-about-the-ccdm-feature-catalog-views.md)
* [Vistas del catálogo de servicios de comercialización](https://experienceleague.adobe.com/es/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [Guía de [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/es/docs/commerce/optimizer/overview){target="_blank"}
* [Introducción a la API de comercialización](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

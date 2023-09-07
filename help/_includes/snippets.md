---
title: Titulares de edición
description: Se han reutilizado elementos visuales para anotar funciones o páginas que se aplican a una edición específica
source-git-commit: 066e031bd98458c8692f1cb3234ff1ecd1b99e6e
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# Titulares de edición

## Función de solo EE {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Función Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Función exclusiva solo en Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Más información</a>)</td></tr>
</table>

## Función exclusiva de B2B {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Función Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Función exclusiva disponible solo con <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html">B2B para Adobe Commerce</a></td></tr>
</table>

## 400 problemas {#avoid-400-error}

>[!CAUTION]
>
>Al realizar llamadas a la API, asegúrese de que se utiliza algún tipo de searchCriteria. También puede considerar el uso de la paginación. Si el resultado de Adobe Commerce es demasiado grande, es posible que se cumpla la capacidad del Generador de aplicaciones de Adobe Developer y se produzca un final inesperado del archivo. El resultado es un resultado de respuesta mal formado como un error 400.\
> Por ejemplo, supongamos que es necesario solicitar todos los productos actuales a Adobe Commerce. La URL resultante sería similar a `{{base_url}}rest/V1/products?searchCriteria=all`. Según el tamaño del catálogo que devuelva, el json puede ser demasiado grande para que el Generador de aplicaciones lo consuma. En su lugar, utilice la paginación y realice algunas solicitudes para evitar `Response is not valid 'message/http'.`

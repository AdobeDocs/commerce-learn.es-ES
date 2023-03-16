---
title: Banners de edición
description: Se han reutilizado elementos visuales para anotar características o páginas que se aplican a una edición específica
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# Banners de edición

## Función solo EE {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Función Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Función exclusiva solo en Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Más información</a>)</td></tr>
</table>

## Función exclusiva B2B {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Función Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Función exclusiva disponible solo con <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">B2B para Adobe Commerce</a></td></tr>
</table>

## 400 problemas {#avoid-400-error}

>[!CAUTION]
>
>Al realizar llamadas de API, asegúrese de que se utiliza algún tipo de searchCriteria. También puede considerar la posibilidad de utilizar la paginación. Si el resultado de Adobe Commerce es demasiado grande, es posible que se cumpla la capacidad de Adobe Developer App Builder y que se produzca un final inesperado del archivo. El resultado es un resultado de respuesta mal formado como un error 400.\
> Por ejemplo, supongamos que es necesario solicitar todos los productos actuales de Adobe Commerce. La URL resultante se parecería `{{base_url}}rest/V1/products?searchCriteria=all`. Según el tamaño del catálogo que devuelve, el json puede ser demasiado grande para que App Builder lo consuma. En su lugar, utilice la paginación y realice algunas solicitudes para evitar `Response is not valid 'message/http'.`

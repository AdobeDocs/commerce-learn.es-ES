---
title: Utilice Fastly para denegar el acceso a un sitio web completo
description: Restringir el acceso al sitio de Adobe Commerce con ACL de Fastly Edge y una VCL personalizada
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Utilice Fastly para denegar el acceso a todo un sitio web

Obtenga información sobre cómo restringir el acceso a su sitio de Adobe Commerce Cloud mediante ACL de Fastly Edge y fragmentos de VCL personalizados. Esta guía paso a paso le ayuda a proteger su entorno previo al lanzamiento, ya que solo permite direcciones IP específicas, lo que garantiza que el sitio de desarrollo permanezca privado.

## Qué va a aprender

Restrinja el acceso al sitio de Adobe Commerce con ACL de Fastly Edge y VCL personalizado | Entornos seguros antes del inicio

## ¿Para quién es este vídeo?

* Ingeniero de DevOps
* Adobe Commerce Developer
* Ingeniero de confiabilidad del sitio

>[!VIDEO](https://video.tv.adobe.com/v/3464779?learn=on)

## Ejemplo de código

Este es un ejemplo del VCL utilizado

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Documentación relacionada

* [Detectando direcciones IP malintencionadas](https://experienceleague.adobe.com/es/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [VCL personalizado para permitir solicitudes](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [VCL personalizado para bloquear solicitudes](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)

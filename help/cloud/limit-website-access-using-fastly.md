---
title: Utilice Fastly para denegar el acceso a un sitio web completo
description: Restringir el acceso al sitio de Adobe Commerce con ACL de Fastly Edge y una VCL personalizada
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00.000Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
TQID: https://experienceleague.adobe.com/OQlPdH-rAoSoPK1-zemp5OODX6pKWV-dW1C88KVRE6I
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 175
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

* [Detectar direcciones IP malintencionadas](https://experienceleague.adobe.com/es/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [VCL personalizado para permitir solicitudes](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [VCL personalizado para bloquear solicitudes](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)

---
title: Restablecer el URI de administrador mediante la CLI
description: Obtenga información sobre cómo restablecer el URI de administrador en la CLI de Adobe Commerce Cloud. Este método es útil cuando los cambios en la URL de administración causan problemas de acceso.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00.000Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
TQID: https://experienceleague.adobe.com/OgyaTHVhQSRFApJPYwkzodSyAJcBjwk8kIrw4VBXltQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 107
ht-degree: 0%

---

# Restablecer el URI de administrador mediante el cli

Obtenga información sobre cómo restablecer el URI de administrador mediante el comando cli de Adobe Commerce Cloud. Resulta útil si la dirección URL de administración se ha cambiado de administrador, pero se ha producido un error y ya no puede acceder a él.

>[!VIDEO](https://video.tv.adobe.com/v/3435066?learn=on)

## Algunos comandos utilizados en el tutorial

Cambie la configuración para esperar una URL de ruta de administración personalizada en 0:

`$ php bin/magento config:set admin/url/use_custom 0`

Cambie la configuración de la URL de la ruta de administración personalizada a 0

`$ php bin/magento config:set admin/url/use_custom_path 0`

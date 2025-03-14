---
title: Restablecer el URI de administrador mediante la CLI
description: Obtenga información sobre cómo restablecer el URI de administrador en la CLI de Adobe Commerce Cloud. Este método es útil cuando los cambios en la URL de administración causan problemas de acceso.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 123
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 25ee35b730cc6265665a87c9c37d24e88c41b60e
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# Restablecer el URI de administrador mediante el cli

Obtenga información sobre cómo restablecer el URI de administrador mediante el comando cli de Adobe Commerce Cloud. Resulta útil si la dirección URL de administración se ha cambiado de administrador, pero se ha producido un error y ya no puede acceder a él.

>[!VIDEO](https://video.tv.adobe.com/v/3435066/?learn=on)

## Algunos comandos utilizados en el tutorial

Cambie la configuración para esperar una URL de ruta de administración personalizada en 0:

`$ php bin/magento config:set admin/url/use_custom 0`

Cambie la configuración de la URL de la ruta de administración personalizada a 0

`$ php bin/magento config:set admin/url/use_custom_path 0`

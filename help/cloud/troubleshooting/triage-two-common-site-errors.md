---
title: Diagnóstico y corrección de algunos errores comunes de Commerce Cloud
description: Resuelva dos errores comunes del proyecto de Adobe Cloud que impiden que el sitio se cargue.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 297
last-substantial-update: 2024-10-30T00:00:00.000Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
TQID: https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 135
ht-degree: 0%

---

# Diagnose and fix service unavailable and an error occurred

Aprenda a clasificar y resolver dos errores comunes que se ven en los proyectos de Adobe Commerce Cloud.  Comprenda cómo y por qué ocurren estos errores y cuáles son los pasos recomendados para resolverlos.

## Para quién es este vídeo

* Desarrolladores y profesionales de TI
* Administradores del sistema

## Contenido de vídeo

* Diagnosticar y resolver problemas de almacenamiento:
* Administrar modo de mantenimiento
* Efficient Troubleshooting tips

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


## Comandos utilizados en el vídeo

Busque las últimas 5 líneas del registro de excepciones mencionado en el mensaje de respuesta.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

Para comprobar el espacio en disco duro. Preste atención a la línea dev/mapper/xxxx.

```SHELL
df -h
```

Vamos a encontrar los 15 archivos más grandes

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

Mostrar el estado del modo de mantenimiento

```SHELL
php bin/magento maintenance:status
```

Disable the maintenance mode

```SHELL
php bin/magento maintenance:disable 
```

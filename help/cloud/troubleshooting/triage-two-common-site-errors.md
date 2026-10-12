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
TQID: 'https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 938d2364-5176-55ec-80f1-9415253e5e51
    internal-label: Site Management
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%
---
# El servicio de diagnóstico y corrección no está disponible y se produjo un error

Aprenda a clasificar y resolver dos errores comunes que se ven en los proyectos de Adobe Commerce Cloud.  Comprenda cómo y por qué ocurren estos errores y cuáles son los pasos recomendados para resolverlos.

## Para quién es este vídeo

* Desarrolladores y profesionales de TI
* Administradores del sistema

## Contenido de vídeo

* Diagnosticar y resolver problemas de almacenamiento:
* Administrar modo de mantenimiento
* Sugerencias de solución de problemas eficientes

>[!VIDEO](https://video.tv.adobe.com/v/3447696?captions=spa&learn=on)


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

Desactivar el modo de mantenimiento

```SHELL
php bin/magento maintenance:disable 
```

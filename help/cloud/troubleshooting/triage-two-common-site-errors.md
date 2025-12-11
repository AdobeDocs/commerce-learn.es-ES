---
title: Diagnóstico y corrección de algunos errores comunes de Commerce Cloud
description: Resuelva dos errores comunes del proyecto de Adobe Cloud que impiden que el sitio se cargue.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# El servicio de diagnóstico y corrección no está disponible y se produjo un error

Aprenda a clasificar y resolver dos errores comunes que se ven en los proyectos de Adobe Commerce Cloud.  Comprenda cómo y por qué ocurren estos errores y cuáles son los pasos recomendados para resolverlos.

## Para quién es este vídeo

- Desarrolladores y profesionales de TI
- Administradores del sistema

## Contenido de vídeo

- Diagnosticar y resolver problemas de almacenamiento:
- Administrar modo de mantenimiento
- Sugerencias de solución de problemas eficientes

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

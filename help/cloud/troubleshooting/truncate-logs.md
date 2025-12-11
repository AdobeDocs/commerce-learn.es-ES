---
title: Truncar registros
description: Obtenga información sobre cómo seleccionar una implementación fallida debido a un disco duro completo truncando archivos de registro grandes.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
exl-id: 4a36de40-fb55-41ad-afef-35fc18a271ec
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# Truncar registros

Obtenga información sobre cómo realizar una prueba y una implementación fallida debido a un disco duro completo. Obtenga información sobre cómo buscar y qué comandos se pueden ejecutar para liberar espacio en el entorno de Adobe Commerce Cloud.

Si cree que podría necesitar estos archivos de registro, puede `rsync` o utilizar otros métodos para obtener una copia disponible del servidor antes de truncarlos.

## Para quién es este vídeo

- Desarrolladores y profesionales de TI
- Administradores del sistema

## Contenido de vídeo

- Diagnóstico y resolución de una implementación fallida
- Donde se encuentran algunos archivos de registro grandes comunes
- Método rápido para truncar un archivo de registro

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## Comandos utilizados en el vídeo

Para comprobar el espacio en disco duro `df -h`. Preste atención a la línea dev/mapper/xxxx.

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


Mostrar los archivos y sus tamaños en un formato legible en lenguaje natural como kb, mb y gb con el comando `ls -lah`

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## Ejemplos de registro truncado

Después de realizar ssh en el proyecto y entorno correctos, cambie al directorio `var/log`. A continuación, puede truncar un archivo con algo similar a `> some-log-file.log`

```BASH
> support_report.log 
> cron.log 
```

## Documentación relacionada

- [Notificaciones de estado](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}

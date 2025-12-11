---
title: Descubra cómo funciona Percona Toolkit pt-query-digest y por qué se utiliza
description: Analizar consultas MySQL desde archivos de registro lentos, generales y binarios. También puede analizar consultas de &grave;SHOW PROCESSLIST&grave; y datos de protocolo MySQL de tcpdump.
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

Aprenda por qué utiliza el compendio pt-query y algunos ejemplos del mundo real para ayudar a profundizar el razonamiento.

## ¿Para quién es este vídeo?

- Arquitectos
- Desarrolladores
- DevOps

## Contenido de vídeo

- Obtenga información acerca del uso de pt-query-digest
- Conozca los beneficios y las deficiencias de esta función de Percona Toolkit
- Comprenda los resultados y aprenda qué posibles pasos de rendimiento deben tenerse en cuenta

>[!VIDEO](https://video.tv.adobe.com/v/3452294?captions=spa&learn=on)

## Referencias de código

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Recursos útiles

- [Kit de herramientas Percona](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}

---
title: Descubra cómo funciona Percona Toolkit pt-query-digest y por qué se utiliza
description: Analizar consultas MySQL desde archivos de registro lentos, generales y binarios. También puede analizar consultas de `SHOW PROCESSLIST` y datos de protocolo MySQL de tcpdump.
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: fdb908f6085b2f050d52742d22241093b46415ed
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 0%

---

# Más información sobre Percona Toolkit pt-query-digest

Aprenda por qué utiliza el compendio pt-query y algunos ejemplos del mundo real para ayudar a profundizar el razonamiento.

## ¿Para quién es este vídeo?

- Arquitectos
- Desarrolladores
- DevOps

## Contenido de vídeo

- Obtenga información acerca del uso de pt-query-digest
- Conozca los beneficios y las deficiencias de esta función de Percona Toolkit
- Comprenda los resultados y aprenda qué posibles pasos de rendimiento deben tenerse en cuenta

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## Referencias de código

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Recursos útiles

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
- [Interbloqueos en MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}

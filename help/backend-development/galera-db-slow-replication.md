---
title: Diagnosticar replicación de Galera DB en registros de consulta lentos de MySQL
description: Aprenda cómo el diseño de replicación de Galera DB ralentiza las sincronizaciones de bases de datos secundarias, cómo identificar estos eventos en los registros de consultas lentas de MySQL y formas de minimizar el impacto.
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 262
ht-degree: 0%

---

# Obtenga información acerca de la replicación de Galera DB y las consultas lentas de MySQL relacionadas

Los clústeres Galera ayudan a mejorar el rendimiento y la escalabilidad. Cuando se tienen en cuenta las bases de datos de réplica, es importante comprender que la forma en que se produce la replicación de datos es diferente que en la base de datos principal. La base de datos primaria puede realizar operaciones masivas. Cuando la replicación se produce en todas las bases de datos de réplica, realizan las acciones de una en una. Por ejemplo, si tiene 67.000.000 de elementos en una eliminación, en las bases de datos de réplica, cada uno de ellos se produce de uno en uno. Al revisar los registros de consultas lentas de MySQL, encuentra que esta acción puede tomar mucho tiempo. El hecho de que las bases de datos de réplica estén realizando operaciones secuencialmente es una razón para que las cosas no estén sincronizadas y se puedan detectar los impactos en el rendimiento.

Para ayudar a que las bases de datos de réplica se mantengan sincronizadas con la base de datos primaria, realice las operaciones grandes por lotes siempre que sea posible. Al hacer las cosas por lotes, permite que las acciones se ejecuten de manera oportuna y que los impactos en el rendimiento se mantengan al mínimo.

## Destinatarios previstos

* Arquitectos
* Desarrolladores
* DevOps

## Contenido de vídeo

* Replicación de Galera a base de datos de réplica
* Más información sobre el control de flujo
* Búsqueda de números de subproceso en registros de consulta lentos de mysql
* Las ejecuciones masivas solo se producen en la principal. Las replicaciones se producen de una en una
* Para ayudar a la replicación a seguir el ritmo de las confirmaciones principales, agrupe las confirmaciones grandes.

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## Recursos útiles

* [Clúster Galera](https://mariadb.com/products/enterprise/galera-cluster/)

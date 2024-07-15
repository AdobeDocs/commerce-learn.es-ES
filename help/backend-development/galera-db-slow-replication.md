---
title: Aprenda a encontrar consultas lentas en los registros de consultas lentas de mysql y por qué el método de diseño de replicación de Galera DB puede ser la razón
description: Galera DB tiene un método de diseño que hace que la replicación de datos en bases de datos secundarias tome más tiempo que la primaria. Aprenda a encontrar estos eventos en el registro de consultas lentas de mysql y la razón subyacente por la que ve entradas en los registros de consultas lentas y quizás cómo evitarlas en el futuro.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
source-git-commit: 598bff1fd2cefdc449d5ae3431401aec1e796313
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Obtenga información acerca de la replicación de Galera DB y las consultas lentas de MySQL relacionadas

Los clústeres Galera ayudan a mejorar el rendimiento y la escalabilidad. Al considerar las bases de datos secundarias, es importante comprender de qué manera la replicación de datos ocurre de forma diferente a como ocurre en la base de datos primaria. La base de datos primaria puede realizar operaciones masivas. Cuando la replicación se produce en todas las bases de datos secundarias, realizan las acciones de una en una. Por ejemplo, si tiene 67 000 000 de elementos en una eliminación, en las bases de datos secundarias cada uno de ellos se produce de uno en uno. Al revisar los registros de consultas lentas de Mysql, se da cuenta de que esta acción puede tomar mucho tiempo. Dado que las bases de datos secundarias realizan las tareas de una en una, es una razón para que las cosas no estén sincronizadas y se puedan detectar impactos en el rendimiento.

Como solución, si es posible, agrupe las operaciones grandes para ayudar a las bases de datos secundarias a sincronizarse con la principal. Al hacer las cosas por lotes, permite que las acciones se ejecuten de manera oportuna y que los impactos en el rendimiento se mantengan al mínimo.

## ¿Para quién es este vídeo?

- Arquitectos
- Desarrolladores
- DevOps

## Contenido de vídeo

- Replicación de Galera a base de datos secundaria
- Más información sobre el control de flujo
- Búsqueda de números de subproceso en registros de consulta lentos de mysql
- Las ejecuciones masivas solo se producen en la principal. Las replicaciones se producen de una en una
- Realice un lote de las grandes confirmaciones para ayudar a la replicación a seguir el ritmo de las principales

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## Recursos útiles

- [Clúster Galera](https://galeracluster.com/)

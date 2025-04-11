---
title: Detectar direcciones IP
description: Obtenga información sobre cómo detectar direcciones IP para entornos de Adobe Commerce Cloud para mejorar la seguridad y optimizar la comunicación con el servidor
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: 3acec65129773a8ba94eb52c53d15d7633440717
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 0%

---

# Detectar direcciones IP para diferentes entornos

Obtenga información sobre cómo detectar direcciones IP para diferentes entornos en un proyecto de Adobe Commerce Cloud. Mediante una serie de comandos que incluyen Adobe Commerce CLI, sed, xargs, dig, grep y sort -u, los usuarios pueden identificar direcciones IP para entornos de desarrollo, ensayo y producción.

## ¿Para quién es este vídeo?

* Desarrolladores: buscan comprender cómo recopilar las direcciones IP para el proyecto de Adobe Commerce Cloud.
* Equipos de seguridad y DevOps que necesiten restringir el acceso a sistemas de terceros o back-end

## Contenido de vídeo {#video-content}

* Descubra cómo descubrir la dirección IP de cualquier entorno en Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## Comando para obtener la dirección IP

Tenga en cuenta que debe utilizar el ID de proyecto y el nombre del entorno en lugar de la información del marcador de posición.  También puede ser necesario cambiar `{1..3}` para que coincida con el número de nodos en su clúster de Adobe Commerce Cloud, pero 3 es el más común.

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## CLI de Adobe Commerce Cloud

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

La herramienta CLI de Magento en la nube está diseñada para ayudar a los desarrolladores y administradores de sistemas a administrar los proyectos y entornos de Adobe Commerce Cloud de forma eficaz. Amplía la funcionalidad de la consola de Cloud, lo que permite a los usuarios realizar tareas rutinarias y ejecutar la automatización localmente. Las funciones clave incluyen la administración de entornos de integración, la comprobación y combinación de entornos, la lista de variables y el uso de SSH para conectarse a entornos remotos. La herramienta simplifica los flujos de trabajo al permitir que los comandos se ejecuten directamente desde la estación de trabajo local, lo que mejora el proceso general de desarrollo e implementación.

En esta sección inicial del código de ejemplo, `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1`, se solicita la dirección URL del entorno. El valor devuelto se parece a este `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`. De vez en cuando se parece más a este `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Este primer comando es bastante simple, y ahora es el momento de pasar al siguiente comando.

Para obtener más información, lea [Información general sobre la CLI de la nube](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## Usando `sed` para buscar y reemplazar

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

El comando `sed` en UNIX®Linux® significa Editor de secuencias. Se utiliza para realizar transformaciones de texto básicas en una secuencia de entrada (un archivo o entrada desde una canalización). Los usos comunes incluyen buscar, buscar y reemplazar, insertar y eliminar texto. El comando `sed` procesa el texto línea a línea y aplica las operaciones especificadas, lo que lo convierte en una potente herramienta para la manipulación de texto y el script.

Como se mencionó anteriormente, normalmente hay 2 tipos de direcciones URL devueltas desde el cli `magento-cloud`. Hay una variación que contiene `.com.c.c` en el centro. Esta variante es la que debe manipularse. Si se detecta esta estructura, es necesario eliminar todo lo que comience desde el principio de la dirección URL hasta `.com.c.c`.  A continuación, lo que queda es solo la última parte de la dirección URL. Una URL de ejemplo es `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Cuando se detecta este patrón, el objetivo es mantener todo después de `.c.`.  En el código de ejemplo proporcionado, `sed 's/.\.c\.(.)/\1/'` se usa para capturar esta parte e ignorar el resto del valor devuelto original. La parte restante de la dirección URL se parece a `abcikdxbg789.ent.magento.cloud/`.\
Hay dos comandos en ejecución en `sed`. Están separados por un punto y coma. La segunda parte de mi `sed` comando `;s/.$//'` es quitar las barras finales, si existen, para limpiar la dirección URL y que tenga el aspecto de `abcikdxbg789.ent.magento.cloud`.  En este punto, la dirección URL se ha limpiado y está lista para el siguiente comando.

## Xargs con dig

```bash
xargs -I% dig +short {1..3}."%"
```

El comando `xargs` en UNIX®Linux® se usa para generar y ejecutar líneas de comandos desde la entrada estándar. Toma la entrada de una barra vertical o un archivo y la convierte en argumentos para otro comando. Es particularmente útil para manejar grandes cantidades de argumentos que exceden el límite del shell. El comando `xargs` se puede usar para realizar operaciones como mover, copiar o eliminar archivos. Permite un procesamiento por lotes eficiente al pasar varios argumentos a comandos en una sola ejecución.

El comando `dig`, abreviatura de Domain Information Groper, es una herramienta de administración de red que se utiliza para consultar servidores DNS (Sistema de nombres de dominio). Ayuda a recuperar información sobre registros DNS, como registros A, AAAA, MX y CNAME. El comando `dig` se utiliza normalmente para solucionar problemas de DNS, comprobar las configuraciones de DNS y recopilar información detallada acerca de los nombres de dominio y sus direcciones IP asociadas. Mediante el uso de varias opciones e indicadores, los usuarios pueden personalizar el resultado para mostrar detalles específicos o un resumen conciso.

El uso de `xargs` con `dig` hace que sea complicado, pero es necesario. El objetivo es tomar esa URL limpia y guardarla.  Una vez guardada la dirección URL como variable, se inserta en el comando `dig`.

El comando `dig` se creó para recopilar información de DNS. Para reducir la cantidad de datos devueltos, se utiliza el argumento `+short`. Al usar `dig` combinado con `+short`, se devuelven direcciones IP y, a veces, cadenas.

En esa parte del comando, `xargs` guardará esa dirección URL `abcikdxbg789.ent.magento.cloud` y la está insertando en el siguiente comando `dig`. La técnica de guardar la dirección URL combinada con la iteración facilita su uso con el comando `dig`. Tenga en cuenta que mi código de muestra es una manera de lograr el objetivo, siéntase libre de modificar las cosas para satisfacer sus necesidades y expectativas.

En este punto, la dirección URL está lista. A continuación, veamos cómo es posible comprobar cada servidor en el clúster. Para Adobe Commerce Cloud, cada servidor del clúster tiene un número. El identificador de servidor es un prefijo de la dirección URL que se acaba de limpiar y preparar para su uso. Una manera rápida y sencilla de desproteger los servidores es usando `{1..3}`. Utilizando `{1..3}` que notifica al comando `dig` que se va a ejecutar 3 veces. A continuación se muestra una representación de lo que sucede si ve la ejecución en tiempo real.

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

Para ilustrar mejor, esta URL de ejemplo que está usando `dig` sería similar a:

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

Si `{1..3}` se modificara para ser `{1..6}`, tendría este aspecto:

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## El comando `grep`

Hay ocasiones en que se devuelve una cadena como parte del resultado de `dig`. Para este fin, el objetivo son solo las direcciones IP y están formadas por números con puntos. Para asegurarse de que la salida final solo contiene números, es posible ajustar el comando. Una vez completada, la sintaxis final es ` grep '^\d'`.  Al agregar `'^\d'`, el comando `grep` solo mantiene los números y no tiene en cuenta nada más.

## El comando `sort`

Al usar `sort -u`, se eliminan los duplicados de la lista de direcciones IP. Los duplicados solo se producen al buscar en los niveles de desarrollo.

Estos entornos de nivel inferior son de varios inquilinos y comparten servidores subyacentes con muchos otros proyectos. Los entornos de desarrollo son servidores únicos y nunca un clúster. Por lo tanto, cuando el comando dig realiza un bucle en cada iteración, devuelve la misma IP muchas veces. Por lo tanto, al usar el comando `sort -u`, se eliminan todas las direcciones IP duplicadas y solo quedan direcciones IP únicas.



## Documentación relacionada

* [Direcciones IP regionales](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses|https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}

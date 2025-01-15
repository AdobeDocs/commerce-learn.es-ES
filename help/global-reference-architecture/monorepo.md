---
title: Monorepo de arquitectura de referencia global
description: Aprenda a utilizar el enfoque de monorepo para la arquitectura de referencia global a fin de establecer una experiencia comercial escalable y resiliente
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Colaboró Tony Evers, arquitecto técnico senior, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Colaboró Tony Evers"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Intermediate, Advanced
source-git-commit: 421c6de9421d715127d0c892dc05ef22f77149cf
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---


# Patrón de arquitectura de referencia global de Monorepo

Esta guía explica cómo configurar Adobe Commerce con el patrón de arquitectura de referencia global (GRA) de Monorepo.

El patrón Monorepo GRA implica un único repositorio Git para alojar todas las personalizaciones comunes. Este único repositorio de Git se expone a través de Composer como paquetes de compositor independientes.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Ventajas y desventajas de este patrón

Ventajas:

- Ideal para pruebas funcionales
- Reutilización del código a través de repositorios de código compartido
- Flexibilidad total en la instalación de paquetes, cada paquete GRA puede actualizarse, degradarse o retroportarse individualmente
- Compatibilidad total con versiones semánticas
- No se requieren herramientas especiales, infraestructuras complejas ni estrategias de ramificación especiales
- Compatibilidad con todos los tipos de paquetes admitidos por Composer
- Ideal para entornos efímeros, que son opcionales, pero para equipos de entrega de gran volumen son muy útiles

Desventajas:

- Posible implementar combinaciones de paquetes que no se desarrollaron en la misma configuración, necesidad de procedimientos de prueba estrictos
- El patrón de GRA monorepo puede ser complejo al inicio. Asigne un posible cliente que ayude al equipo a trabajar con el sistema

## Configurar Adobe Commerce con el patrón de paquetes separados GRA

### La estructura del directorio

La estructura final de directorios de una instalación completa de Adobe Commerce con el patrón de paquetes separados GRA tiene esta estructura de directorios:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

Un repositorio Git de producción tiene esta estructura de directorio:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

La diferencia es que las instancias de producción se instalan desde Composer, donde el monorepo tiene su propia copia de cada paquete dentro del directorio de paquetes.

### Preparación de un repositorio de producción

Cree un repositorio para la primera instancia de Adobe Commerce, que represente un almacén web para la marca X.

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Instale Adobe Commerce con `bin/magento setup:install`. Confirme los `app/etc/config.php` y los archivos del compositor resultantes. Composer gestiona cualquier otra cosa, por lo que no debería haber nada más en Git.

### Preparación del repositorio de monorepo

El repositorio monorepo comienza con los mismos pasos.

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

Instalar Adobe Commerce con `bin/magento setup:install`, confirmar y enviar.

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### Preparación para el desarrollo de monorepo

Para el desarrollo de monorepo, realice los siguientes cambios en su archivo composer.json:

1. Cambie el nombre y la descripción del paquete para que quede claro que este es su paquete monorepo.
1. Elimine el atributo de versión de composer.json, ya que la versión se administra mediante etiquetas Git para este repositorio.
1. Reemplace la sección requerida por un metapaquete que se cree más adelante.
1. Cambie la estabilidad mínima a dev.
1. Agregue un repositorio de tipo de ruta que señale a `packages/*/*` para hospedar paquetes monorepo, incluidos alias de ramas para cada paquete que contenga
1. Añada un alias de rama para el propio proyecto

La siguiente diferencia de Git muestra la diferencia entre una instalación limpia de Adobe Commerce y los cambios mencionados anteriormente:

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### Uso de metapaquetes

Descargue el código de ejemplo de [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) en GitHub para obtener los metapaquetes y los módulos de ejemplo que se utilizan en este ejemplo.

Los metapaquetes de Composer agrupan varios paquetes de Composer bajo un solo paquete. Cuando se requiere un metapaquete, todos los paquetes que agrupa se instalan automáticamente a través de la sección de requisitos del Compositor del metapaquete.

Para este ejemplo, hay dos metapaquetes:

1. **antonevers/gra-meta-brand-x**: Un metapaquete que contiene todo lo que constituye la &quot;Marca X&quot;
1. **antonevers/gra-meta-foundation**: un metapaquete que contiene todo lo que está siempre instalado en cualquier marca

El metapaquete de marca requiere el metapaquete de base. Cuando se requiere un metapaquete de marca, también se requiere automáticamente el metapaquete de base. Consulte los dos archivos composer.json de los metapaquetes para ver cómo se relacionan:

antonevers/gra-meta-brand-x:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

Los metapaquetes son una buena forma de organizar el código que pertenece a ambos. Utilice metapaquetes para definir grupos de paquetes que sean regionales, globales, específicos de la marca o cualquier agrupación que tenga sentido para usted. Si mantiene varias instalaciones de Adobe Commerce, combina una forma segura y versátil de definir el contexto en el que se espera un paquete.

Hay metapaquetes en el monorepo dentro del directorio `packages`. En este caso, la estructura de directorio de `vendor` se refleja:

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### Añadir y desarrollar módulos

Los módulos del monorepo existen en el directorio `packages`. De este modo, Composer puede encontrarlos a través del repositorio de tipo de ruta.

Descargue el código de ejemplo de [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) en GitHub para obtener los metapaquetes y los módulos de ejemplo que se utilizan en este ejemplo.

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

Si es necesario, puede tener varias áreas de nombres dentro del directorio `packages`.

El desarrollo tiene lugar en el directorio de paquetes. Los enlaces simbólicos a los paquetes dentro del directorio `packages` se crearán en el directorio `vendor` una vez que ejecute `composer update`. De este modo, el código forma parte de la instalación de Adobe Commerce.

Ejecute `bin/magento module:enable --all` o solo para módulos específicos a fin de habilitar los módulos que se agregaron.

Por ahora, debería tener una instalación de Adobe Commerce en funcionamiento con los tres módulos de muestra instalados y en funcionamiento. Puede validar si los módulos están instalados y funcionan ejecutando los comandos:

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### Creación automatizada de paquetes

Existen varias opciones para lograr la creación automatizada de paquetes. Algunas opciones son:

1. [Empaquetador privado](https://packagist.com/)
1. [Simplificar el generador de repositorios](https://github.com/symplify/monorepo-builder)
1. Cree su propia solución

[Packagist privado](https://packagist.com/) automatiza el reconocimiento de paquetes en el monorepo de Git y los expone a través del Compositor. Es compatible con Adobe Commerce, rápido, de bajo mantenimiento y propenso a errores, por lo que esta guía se centra en la opción Private Packagist.

No entra dentro del ámbito de esta guía explicar cómo configurar Private Packagist, consulte los [documentos](https://packagist.com/docs).

Existe la posibilidad de convertir un paquete en un monorepo una vez que haya configurado la sincronización de la organización y los repositorios Git se estén sincronizando automáticamente con Private Packagist.

En primer lugar, vaya a la pestaña paquetes y busque el monorepo:

![Captura de pantalla de Packagist privado con el paquete monorepo visible en la pantalla de paquetes](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

Haga clic en el paquete monorepo y haga clic en &quot;Editar&quot; en la pantalla de detalles, que le lleva a la siguiente página:

![Captura de pantalla de Packagist privado con la página de edición del paquete monorepo](/help/assets/global-reference-architecture/packagist-packages-edit.png)

Debajo del primer campo de entrada hay un vínculo que dice: Crear un repositorio de varios paquetes. Haga clic en este vínculo.

![Captura de pantalla de Packagist privado con la configuración de varios paquetes](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

Defina la ubicación donde los paquetes de composición se pueden encontrar dentro de su monorepo. En el ejemplo, la ubicación es `packages/**/composer.json`. Cambie la ubicación para limitar o ampliar el lugar donde Private Packagist busca los paquetes que desea extraer.

La pestaña packages muestra todos los paquetes encontrados después de guardar, y el monorepo en sí ya no será visible como paquete Composer:

![Captura de pantalla de Packagist privado con todos los paquetes monorepo visibles en la pantalla de paquetes](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

Se crea una versión en Composer para cada paquete dentro del monorepo, para cada etiqueta o rama que se crea en el monorepo en Git.

## Instalación de los paquetes en el entorno de producción

Siga las instrucciones de Private Packagist para agregar Private Packagist como repositorio de compositor. Private Packagist puede y debe usarse como un espejo para todos sus repositorios de Composer y Git, incluyendo packagist.org. De este modo, las credenciales no tienen que compartirse con los desarrolladores y tiene un control completo sobre cada paquete. Este ejemplo no sigue esta práctica recomendada, ya que expondría el código base de Adobe Commerce públicamente.

Descargue [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x) desde GitHub para ver un ejemplo de una tienda de producción.

En el almacén de producción no hay ningún directorio `packages` y todos los paquetes se instalan mediante Composer. El único paquete que se requiere es:

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

Sin embargo, todo Adobe Commerce y todo el GRA se instalan a través de los requisitos de este metapaquete.

## Versiones

Todos los paquetes del monorepo reciben la misma versión que el propio monorepo. Considérelo como una publicación de nuevas versiones de toda la aplicación. En la producción, sin embargo, puede instalar una mezcla de paquetes de diferentes versiones de monorepo.

## Entornos efímeros

Si utiliza entornos efímeros o planea usarlos, el monorepo es una excelente opción. Cada versión y rama del monorepo contiene todos los archivos de módulo de Adobe Commerce, de terceros y personalizados. Con una instalación completa en cada rama, es posible ejecutar todos los tipos de pruebas, incluidas las pruebas funcionales. Con otras configuraciones de GRA, como los paquetes independientes o los paquetes masivos de GRA, primero deberá crear un entorno de Adobe Commerce en funcionamiento antes de poder ejecutar pruebas funcionales. Desde la perspectiva de DevOps, monorepo lo hace mucho más simple.

## Ejemplos de código

Los ejemplos de código de este artículo se han combinado en un conjunto de repositorios Git, que puede utilizar para jugar con la prueba de concepto.

- Un ejemplo de repositorio monorepo: <https://github.com/AntonEvers/gra-monorepo>
- Un almacén de producción de ejemplo: <https://github.com/AntonEvers/gra-monorepo-brand-x>

---
title: Optimización de Adobe Commerce con la arquitectura de referencia global de paquetes masivos
description: Aprenda a configurar Adobe Commerce mediante la arquitectura de referencia global de paquetes en lote para una administración eficiente del código y un control de versiones.
jira: KT-16726
doc-type: tutorial
duration: 391
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Colaboró Tony Evers, arquitecto técnico senior de Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Colaboró Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
TQID: https://experienceleague.adobe.com/q4NzQxc7XJDB-TNv2pU7ghDr6bahliY6soUGPu7fhfg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1188
ht-degree: 0%

---

# Patrón de arquitectura de referencia global de paquetes masivos

{{only-for-on-prem-commerce-cloud}}

En esta guía se explica cómo configurar Adobe Commerce con el patrón de arquitectura de referencia global (GRA) de paquetes en lote.

El patrón GRA de paquetes masivos implica un único repositorio Git para alojar todas las personalizaciones comunes. Este repositorio único de Git se expone a través de Composer como un paquete único composer, que contiene varios módulos Adobe Commerce.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de paquetes masivos](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## Ventajas y desventajas de este patrón

Ventajas:

* Reutilización del código a través de un repositorio de código compartido
* Flexibilidad para instalar diferentes versiones históricas del GRA en diferentes instancias, lo que permite versiones por fases
* Flexibilidad para retroportar y mantener varias versiones principales de GRA
* Compatibilidad con el control de versiones semánticas de GRA
* Simplicidad, los desarrolladores no necesitan más habilidades que en los patrones de desarrollo de una sola tienda regular
* No se requieren herramientas especiales, infraestructuras complejas ni estrategias de ramificación especiales
* La combinación de paquetes de una versión siempre se desarrolla y prueba en conjunto

Desventajas:

* Solo es posible actualizar el GRA completo, incluyendo todos los paquetes contenidos en él.
* El paquete masivo GRA no es compatible con otros paquetes de compositor que no sean módulos de Adobe Commerce, paquetes de idiomas y temáticas, por lo que no hay metapaquetes, paquetes de componentes de Magento2, complementos de Composer y parches

## Configuración de Adobe Commerce con el patrón Split Git GRA

### La estructura del directorio

El GRA de paquetes masivos instala todo el código reutilizable a través de los repositorios de Composer. El código específico de una sola instancia reside dentro del repositorio Git de esa instancia. El código específico de la instancia no se reutiliza en otras instancias de Adobe Commerce.

La estructura final de directorios de una instalación completa de Adobe Commerce con el patrón GRA de paquetes en lote tiene este aspecto:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

Los directorios `app/code`, `app/i18n` y `app/design` se omiten a propósito, porque Composer no evalúa el código de estos directorios. Como resultado, las dependencias que se declaran en los paquetes no se instalan automáticamente. El patrón GRA de paquetes masivos resuelve este problema instalando código personalizado en `packages/` y tratando ese directorio como un repositorio del compositor. El compositor enlaza simbólicamente paquetes dentro de `packages/` a `vendor/`.

### Preparación de los repositorios de Git

Cree dos repositorios Git para el código GRA compartido y para el primer almacén. Comience con el repositorio GRA, que tiene la siguiente estructura de archivos:

El resultado es la siguiente estructura de directorio:

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

Esta estructura de directorio solo menciona el archivo composer.json y el archivo registration.php de los módulos GraOne y GraTwo. En realidad, hay más archivos dentro de estos módulos.

Ejecute estos comandos para iniciar el repositorio Git:

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

El archivo composer.json contiene lo siguiente:

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

Cambie el área de nombres del paquete composer.json a su propia área de nombres.

El archivo src/registration.php contiene:

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

El archivo registration.php busca otros archivos registration.php dentro de los módulos Adobe Commerce y los ejecuta.

Cree los dos módulos de ejemplo con el código de <https://github.com/AntonEvers/gra-bulk-foundation>. Composer no evalúa los archivos composer.json en los módulos de ejemplo. Están ahí como un hábito. Si decide mover los módulos a otro lugar, los archivos composer.json volverán a ser necesarios.

### Configuración del repositorio de almacenamiento

El repositorio de implementación contiene toda la instalación de Adobe Commerce, incluido el código GRA. Cree el repositorio de implementación:

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Instale Adobe Commerce con `bin/magento setup:install`. Instale los módulos de muestra GRA en el repositorio de implementación con Composer:

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

El último comando debería generar el siguiente resultado para probar que el módulo está instalado y en funcionamiento:

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

Puede crear varios paquetes masivos para organizar el código. Por ejemplo, un paquete masivo de terceros para código de terceros que no está disponible a través de Composer. Todo lo que instalaría tradicionalmente en `app/code` debería estar ahora en el directorio `src/` del paquete en bloque. Una excepción a esa regla es el código que solo se utiliza en una sola instancia. Estos paquetes se denominan paquetes locales.

### Instalación de paquetes locales

El repositorio de implementación aloja los paquetes locales. No viven en el paquete masivo GRA. La ubicación de los paquetes locales no es `app/code` sino `packages/local`. Indique al Compositor que trate este directorio como un repositorio:

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

Agregue el módulo de ejemplo que se hospeda en <https://github.com/AntonEvers/module-example-local>:

```bash
mkdir -p packages/local
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

El último comando debería generar el siguiente resultado para probar que el módulo está instalado y en funcionamiento:

```bash
Local module is installed successfully and working!
```

Asigne el módulo local al repositorio de la marca:

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## Información general sobre las ubicaciones de código

Solo si el tercero no ofrece la instalación a través de un repositorio de Composer, puede almacenar módulos de terceros en el directorio `src/` de su repositorio base o en un paquete masivo de terceros dedicado.

* **Adobe Commerce Core**: disponible a través de repo.magento.com.
* **Módulos de terceros**: disponibles a través de Marketplace o del repositorio de Compositor propio de un proveedor.
* **Opción de reserva de módulos de terceros**: almacenada en `src/` de un paquete masivo.
* **código de base GRA**: almacenado en `src/` del paquete base masiva.
* **Código local**: almacenado en el directorio `packages/local` del repositorio de implementación.

## Desarrollo de un módulo GRA

Instale el paquete masivo desde el origen para habilitar Git en el directorio de paquetes masivos:

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

The bulk package has been checked out using Git. When you enter the `vendor/antonevers/gra-bulk-foundation` directory, you are also entering the gra-bulk-foundation Git repository. You can create, checkout and merge branches in this directory.

Add Composer dependencies to the composer.json file at the root of the GRA bulk package, which is the only file in the bulk package that Composer evaluates.

## Include third-party modules to the GRA bulk package

Add third-party packages in the require section of the composer.json at the root of the GRA foundation to add them to your GRA. That way, the packages are always installed in all your instances through composer.

## Deliver your code

To deliver code to the main branch, there are 2 paths. First the local modules, which are merged to the main branch. Run Composer update for those modules. Do not allow developers to update composer.lock in their ticket branches to reduce conflicts. Only update the composer.lock file in staging and production branches, which reduces the risk of conflicts.

Secondly, the GRA bulk packages, which are merged into the main branch of the GRA bulk repository. Then you can add a Git tag to the main branch, versioning the Composer package. Require your new version of the GRA bulk package in the composer.json of the deployment repository to install it.

## Branching strategy

This GRA pattern works with all branching strategies so long as you mirror the branching strategy of the deployment repositories in your GRA bulk repository. For releases, create a release branch with the same name in both repositories. For development, create a ticket branch in both repositories.

In ticket branches, you should almost never have to update the composer.lock file. Just check out the right branches in your development environment for both the store and the GRA foundation repository with Git. The exception is when you update requirements in the GRA foundation composer.json file. Upgrading the GRA foundation in the deployment repository is only done when building the release, or when building a QA branch.

## Code examples

The code examples of this article are available as a set of Git repositories, which you can use to test the proof of concept.

* An example production store: <https://github.com/AntonEvers/gra-bulk-brand-x>
* The GRA code repository: <https://github.com/AntonEvers/gra-bulk-foundation>
* An example local module: <https://github.com/AntonEvers/module-example-local>

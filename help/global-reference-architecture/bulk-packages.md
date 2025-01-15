---
title: Optimización de Adobe Commerce con la arquitectura de referencia global de paquetes masivos
description: Aprenda a configurar Adobe Commerce mediante la arquitectura de referencia global de paquetes en lote para una administración eficiente del código y un control de versiones.
kt: 16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Colaboró Tony Evers, arquitecto técnico senior, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Colaboró Tony Evers"
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# Patrón de arquitectura de referencia global de paquetes masivos

En esta guía se explica cómo configurar Adobe Commerce con el patrón de arquitectura de referencia global (GRA) de paquetes en lote.

El patrón GRA de paquetes masivos implica un único repositorio Git para alojar todas las personalizaciones comunes. Este repositorio único de Git se expone a través de Composer como un paquete único composer, que contiene varios módulos Adobe Commerce.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de paquetes masivos](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## Ventajas y desventajas de este patrón

Ventajas:

- Reutilización del código a través de un repositorio de código compartido
- Flexibilidad para instalar diferentes versiones históricas del GRA en diferentes instancias, lo que permite versiones por fases
- Flexibilidad para retroportar y mantener varias versiones principales de GRA
- Compatibilidad con el control de versiones semánticas de GRA
- Simplicidad, los desarrolladores no necesitan más habilidades que en los patrones de desarrollo de una sola tienda regular
- No se requieren herramientas especiales, infraestructuras complejas ni estrategias de ramificación especiales
- La combinación de paquetes de una versión siempre se desarrolla y prueba en conjunto

Desventajas:

- Solo es posible actualizar el GRA completo, incluyendo todos los paquetes contenidos en él.
- El paquete masivo GRA no es compatible con otros paquetes de compositor que no sean módulos de Adobe Commerce, paquetes de idiomas y temáticas, por lo que no hay metapaquetes, paquetes de componentes de Magento2, complementos de Composer y parches

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

- **Adobe Commerce Core**: disponible a través de repo.magento.com.
- **Módulos de terceros**: disponibles a través de Marketplace o del repositorio de Compositor propio de un proveedor.
- **Opción de reserva de módulos de terceros**: almacenada en `src/` de un paquete masivo.
- **código de base GRA**: almacenado en `src/` del paquete base masiva.
- **Código local**: almacenado en el directorio `packages/local` del repositorio de implementación.

## Desarrollo de un módulo GRA

Instale el paquete masivo desde el origen para habilitar Git en el directorio de paquetes masivos:

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

El paquete masivo se ha extraído mediante Git. Cuando escribe el directorio `vendor/antonevers/gra-bulk-foundation`, también está entrando en el repositorio Git de base masiva. Puede crear, extraer y fusionar ramas en este directorio.

Agregue dependencias de Composer al archivo composer.json en la raíz del paquete masivo de GRA, que es el único archivo del paquete masivo que Composer evalúa.

## Incluir módulos de terceros en el paquete masivo de GRA

Agregue paquetes de terceros en la sección de requisitos del archivo composer.json en la raíz de la base de GRA para agregarlos a su GRA. De este modo, los paquetes siempre se instalan en todas las instancias a través de composer.

## Entrega del código

Para enviar código a la rama principal, hay dos rutas. Primero los módulos locales, que se combinan en la rama principal. Ejecute la actualización del Compositor para esos módulos. No permita que los desarrolladores actualicen composer.lock en sus ramas de tickets para reducir los conflictos. Actualice únicamente el archivo composer.lock en las ramas de ensayo y producción, lo que reduce el riesgo de conflictos.

En segundo lugar, los paquetes GRA a granel, que se combinan en la rama principal del repositorio GRA a granel. A continuación, puede agregar una etiqueta Git a la rama principal, creando versiones del paquete Composer. Requiera su nueva versión del paquete masivo de GRA en el composer.json del repositorio de implementación para instalarlo.

## Estrategia de ramas

Este patrón GRA funciona con todas las estrategias de ramificación, siempre y cuando refleje la estrategia de ramificación de los repositorios de implementación en el repositorio masivo de GRA. Para las versiones, cree una rama de versión con el mismo nombre en ambos repositorios. Para el desarrollo, cree una rama de tickets en ambos repositorios.

En las ramas de tickets, casi nunca debería tener que actualizar el archivo composer.lock. Solo tiene que consultar las ramas adecuadas en su entorno de desarrollo tanto para la tienda como para el repositorio de base GRA con Git. La excepción se produce al actualizar los requisitos en el archivo composer.json de base de GRA. La actualización de la base de GRA en el repositorio de implementación solo se realiza al crear la versión o al crear una rama de control de calidad.

## Ejemplos de código

Los ejemplos de código de este artículo están disponibles como un conjunto de repositorios Git, que puede utilizar para probar la prueba de concepto.

- Un almacén de producción de ejemplo: <https://github.com/AntonEvers/gra-bulk-brand-x>
- El repositorio de código GRA: <https://github.com/AntonEvers/gra-bulk-foundation>
- Ejemplo de módulo local: <https://github.com/AntonEvers/module-example-local>

---
title: Configuración de Adobe Commerce con la arquitectura de referencia global de Split Git
description: Aprenda a configurar Adobe Commerce mediante la arquitectura de referencia global Split Git para una administración eficiente del código y una implementación optimizada. palo de golf
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Colaboró Tony Evers, arquitecto técnico senior de Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Colaboró Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Patrón de arquitectura de referencia global de Split Git

En esta guía se explica cómo configurar Adobe Commerce con el patrón de arquitectura de referencia global (GRA) de Split Git.

El patrón Git GRA dividido implica dos repositorios Git para desarrollo y un repositorio Git por instancia de Adobe Commerce. En los ejemplos, se da por hecho que cada instancia representa una marca única.

![Diagrama que muestra dónde se almacena el código en un patrón GRA dividido](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## Ventajas y desventajas de este patrón

Ventajas:

- Reutilización del código a través de un repositorio de código compartido
- Patrón GRA simple, adecuado incluso para equipos con conocimientos limitados de Compositor
- Además de los módulos, temáticas y paquetes de idiomas de Adobe Commerce, es posible instalar cualquier tipo de paquete Composer a través de este modelo, incluidos composer-plugin, composer-metapackage, magento2-component y parches
- Posibilidad de realizar lanzamientos por fases, planificando lanzamientos a regiones en sus propios períodos de mantenimiento
- Compatibilidad con etiquetas Git con fines de administración, no para el control de implementación
- Garantizar que la combinación de paquetes en una implementación de producción se desarrolle y pruebe en esta configuración exacta.

Desventajas:

- Sin flexibilidad añadida en comparación con otros patrones de GRA
- No es posible actualizar o degradar módulos individuales por instancia, actualizar o degradar siempre el GRA en su conjunto
- En la mayoría de los casos, el patrón de paquetes masivos encaja mejor ya que es igualmente simple, pero más convencional

## Configuración de Adobe Commerce con el patrón Split Git GRA

### La estructura del directorio

El patrón Split Git GRA tiene dos tipos de repositorios: repositorios de desarrollo y repositorios de instalación. Los repositorios de desarrollo solo contienen parte de una instalación completa de Adobe Commerce. Los repositorios de instalación contienen la instalación completa de Adobe Commerce y se utilizan para la implementación, pero no para el desarrollo.

La estructura final de directorios de una instalación completa de Adobe Commerce con el patrón Split Git GRA tiene este aspecto:

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Los directorios `app/code`, `app/i18n` y `app/design` se omiten a propósito, porque Composer no evalúa el código de estos directorios. Como resultado, las dependencias que se declaran en los paquetes no se instalan automáticamente. El patrón Split Git GRA resuelve este problema instalando todo el código personalizado en `packages/` y tratando ese directorio como un repositorio de composición. El compositor enlaza simbólicamente paquetes dentro de `packages/` a `vendor/`.

### Preparación de los repositorios de Git

Cree 3 repositorios Git para:

1. Una instancia de Adobe Commerce
2. Código de terceros que no se instala mediante Composer
3. Sus personalizaciones en forma de módulos, temáticas, paquetes de idiomas y demás; su GRA

En esta guía se utilizan los siguientes nombres para estos repositorios:

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

Todos los repositorios del patrón Split Git GRA se combinan en uno. Para que Git permita la combinación de varios repositorios, los tres repositorios necesitan un historial compartido. Cree un proyecto Git con una sola confirmación e inclúyalo en todos los remotos.

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

Si se inserta el archivo temporal `.gitkeep` en todos los remotos, se crea la misma confirmación inicial con el mismo hash de confirmación, lo que crea un historial compartido. Cada cambio que se crea en un control remoto se puede combinar con los demás.

Desde aquí, los repositorios divergen. El repositorio gra-split-brand-x contiene un código específico de la marca. El repositorio gra-split-3rdparty contiene solo código de terceros. El repositorio gra-split-gra contiene únicamente la base de la arquitectura de referencia global, que consta de todo el código personalizado.

Instale Adobe Commerce en el repositorio gra-split-brand-x.

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

Cree confirmaciones iniciales en los repositorios gra-split-3rd y gra-split-gra. La forma más sencilla es desprotegiendo estos repositorios en directorios separados.

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

Estos dos repositorios almacenan paquetes de terceros y paquetes GRA. Puede haber código que sea exclusivo para cada instancia de Adobe Commerce. Cree un lugar para almacenar estos paquetes locales en el repositorio gra-split-brand-x.

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### Dónde almacenar distintos tipos de código

Adobe Commerce es una aplicación de composición. La forma preferida de realizar la instalación es siempre a través de los repositorios de Composer. Solo si un proveedor de módulos no ofrece la instalación a través de un repositorio de Composer, puede almacenar módulos de terceros en el repositorio de terceros. El lugar preferido para el código personalizado es el repositorio GRA. Cuando un módulo solo lo utiliza una instancia específica, se convierte en código local.

Resumiendo:

- **Adobe Commerce**: almacenado en un repositorio de Composer.
- **Módulos de terceros**: almacenados en un repositorio de Composer.
- **Opción de reserva de módulos de terceros**: almacenada en el repositorio Git de terceros divididos en varias partes.
- **código base GRA**: almacenado en el repositorio Git gra-split-gra.
- **Código local**: almacenado en el repositorio Git gra-split-brand-x.

### Conectar el almacenamiento del paquete a Composer

Composer puede tratar el directorio de paquetes como un repositorio de composer. Informe al Compositor de la ubicación de los paquetes dentro del directorio de paquetes.

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer busca archivos composer.json de dos niveles de profundidad en los tres directorios de almacenamiento. Cree subdirectorios dentro de los tres directorios de almacenamiento de código exactamente como aparecerían en el directorio `vendor/`.

Por ejemplo: si un paquete se instala normalmente en `vendor/example-corp/module-example/`, entonces lo almacena en `packages/3rdparty/example-corp/module-example/`. Compositor enlaza simbólicamente el paquete a `vendor/example-corp/module-example/` cuando lo necesite.

Utilice el espacio de nombres y el nombre del paquete del compositor como estructura de directorios. Por ejemplo: un módulo que existe tradicionalmente en `app/code/MyCorp/MyCustomization/` tiene el nombre `my-corp/module-my-customization` en composer.json. Almacenar este paquete en `packages/gra/my-corp/module-my-customization`.

### Incluir nuevos paquetes en los repositorios de instancias

Combine paquetes de los remotos de terceros y GRA en el repositorio gra-split-brand-x.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

El resultado es la siguiente estructura de directorio:

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Los cambios en el repositorio de base de GRA y de terceros se combinan en los repositorios de marca. De este modo, el código de terceros y el código GRA solo se mantienen en un lugar. Mueva los cambios a las marcas con una combinación Git.

Adobe Commerce no reconoce automáticamente los nuevos módulos. Ejecute el compositor requerido para añadir un nuevo paquete después de una combinación. Ejecute Composer update cada vez que actualice uno de los paquetes después de una combinación.

### Instalación de módulos de ejemplo

Como prueba de concepto, instale módulos de ejemplo para ver cómo funciona el patrón GRA.

Ejecute `composer install` y `bin/magento install` antes de continuar.

Hay 3 módulos de prueba para en GitHub:

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### Instalación de un módulo local

Añadir un módulo al grupo de código local es sencillo. Descargue y extraiga el módulo. Requerirlo con Composer. Habilite la opción con `bin/magento` y confirme los archivos en el repositorio de marca.

```bash
cd gra-split-brand-x
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

Si ve el resultado anterior, puede enviarlo con seguridad al repositorio de marca.

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### Instalación y desarrollo de un módulo de base GRA

Añadir un módulo al repositorio GRA es diferente a instalar módulos locales. De forma predeterminada, las confirmaciones se añaden a origin/main, que es el repositorio de gra-split-brand-x. Los cambios en los módulos GRA deben insertarse en el repositorio gra-split-gra y combinarse posteriormente en el repositorio gra-split-brand-x.

### Creación de un entorno de desarrollo

Cree un entorno de desarrollo con una combinación de todos los grupos de códigos en un solo lugar. Puede insertar código en el repositorio local, GRA y de terceros individualmente mediante enlaces simbólicos. Comience creando un nuevo directorio de desarrollo junto a los directorios de repositorios de marca, GRA y de terceros.

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

El resultado es la siguiente estructura de directorio:

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

Ejecute `composer install` y `bin/magento install` en el directorio de gra-development.

Ahora es posible confirmar cambios directamente desde los directorios `packages/3rdparty`, `packages/gra` y `package/local`. Git confirma los cambios en el repositorio Git al que se vinculan simbólicamente los directorios. Es importante que el comando git commit se emita dentro del directorio `packages/3rdparty`, `packages/gra` o `package/local`. No ejecute la confirmación de Git en la raíz del proyecto.

### Instalación de módulos de ejemplo

Instale los módulos de ejemplo de terceros y GRA en los directorios de paquetes.

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

El último comando debería generar el siguiente resultado para probar que el módulo está instalado y en funcionamiento:

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

Si ve el resultado anterior, puede enviarlo con seguridad al repositorio de marca. Ejecute `git remote -v` para comprobar que está confirmando con el control remoto correcto.

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### Entrega de código a las instancias

Combine los repositorios GRA y de terceros en el repositorio gra-split-brand-x para enviar el código a una instancia de Adobe Commerce. Ejecute `composer require`, `bin/magento module:enable` y confirme el resultado.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## Estrategia de ramas

Este patrón GRA funciona con todas las estrategias de ramificación, si refleja la estrategia de ramificación de los repositorios de almacenamiento en los repositorios de terceros y GRA. Para las versiones, cree una rama de versión con el mismo nombre en los tres repositorios. Combine las ramas de la versión en el repositorio de almacenamiento durante la preparación de la versión.

A veces tiene una rama de tickets que requiere que se modifiquen tanto el código local como el código de terceros o el código GRA. En este caso, es necesario crear las ramas de tickets en todos los repositorios relacionados.

Nunca fusione los compromisos de terceros y GRA en el repositorio de la marca dentro de las ramas de los tickets. En su lugar, consulte las ramas adecuadas en su entorno de desarrollo para cada grupo de códigos. La combinación en el repositorio de marca solo se realiza al maquetar la versión o al maquetar una rama de control de calidad.

## Ejemplos de código

Los ejemplos de código de este artículo están disponibles como un conjunto de repositorios Git, que puede utilizar para probar la prueba de concepto.

- Un almacén de producción de ejemplo: <https://github.com/AntonEvers/gra-split-brand-x>
- El repositorio de código de terceros: <https://github.com/AntonEvers/gra-split-3rdparty>
- El repositorio de código GRA: <https://github.com/AntonEvers/gra-split-gra>
- Ejemplo de módulo local: <https://github.com/AntonEvers/module-example-local>
- Un ejemplo de módulo GRA: <https://github.com/AntonEvers/module-example-gra>
- Un ejemplo de módulo de terceros: <https://github.com/AntonEvers/module-example-3rdparty>

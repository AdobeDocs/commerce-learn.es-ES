---
title: Arquitectura de referencia global de paquetes independientes
description: Optimizar Adobe Commerce con paquetes separados GRA. Conozca la configuración, las ventajas y las prácticas recomendadas para una administración flexible y con versiones de los paquetes.
jira: KT-16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Colaboró Tony Evers, arquitecto técnico senior de Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Colaboró Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: cbddc4a3-602f-4208-85cd-b906d2b81f8b
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '2101'
ht-degree: 0%

---

# Patrón de arquitectura de referencia global de paquetes separados

En esta guía se explica cómo configurar Adobe Commerce con el patrón de arquitectura de referencia global (GRA) de paquetes independientes.

El patrón GRA de paquetes independientes implica un repositorio Git para cada paquete común y un repositorio Git para cada instancia de Adobe Commerce. Los paquetes comunes se exponen a través de Composer con un repositorio privado de Composer.

Este patrón de arquitectura de referencia global se basa completamente en el Compositor y está diseñado para aprovechar al máximo todas las funciones del Compositor.

![Diagrama que muestra dónde se almacena el código en un patrón GRA de paquetes separados](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## Ventajas y desventajas de este patrón

Ventajas:

- Reutilización del código a través de repositorios de código compartido
- Flexibilidad total en la instalación de paquetes, cada paquete GRA puede actualizarse, degradarse o retroportarse individualmente
- Compatibilidad total con versiones semánticas
- No se requieren herramientas especiales, infraestructuras complejas ni estrategias de ramificación especiales
- Compatibilidad con todos los tipos de paquetes admitidos por Composer

Desventajas:

- El desarrollo dentro de este patrón GRA es ligeramente más difícil al principio, hay una pequeña curva de aprendizaje
- Posible implementar combinaciones de paquetes que no se desarrollaron en la misma configuración, necesidad de procedimientos de prueba estrictos

## Configurar Adobe Commerce con el patrón de paquetes separados GRA

### La estructura del directorio

La estructura final de directorios de una instalación completa de Adobe Commerce con el patrón de paquetes separados GRA tiene este aspecto:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

Los directorios `app/code`, `app/i18n` y `app/design` se omiten a propósito. El GRA de Paquetes Separados instala cada paquete desde Composer. Incluso si el paquete solo está instalado en una instancia de Adobe Commerce.

### Preparación del repositorio de almacenamiento

Cree un repositorio para la primera instancia de Adobe Commerce, que represente un almacén web para la marca X.

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Instale Adobe Commerce con `bin/magento setup:install`. Confirme el `app/etc/config.php` resultante.

### Creación de repositorios de paquetes

Cada paquete de este patrón de arquitectura de referencia global tiene su propio repositorio Git. A continuación se muestran paquetes de ejemplo que contienen módulos Adobe Commerce que representan un módulo GRA, un módulo de terceros y un módulo local.

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

Utilice los ejemplos para crear sus propios paquetes.

### Creación de repositorios de metapaquetes

Los metapaquetes controlan el alcance de la base de código común GRA en este patrón GRA. Definen lo que hay en la base: qué combinación de versiones de paquetes se instalan siempre juntas. Un ejemplo:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

El fragmento anterior es composer.json de un metapaquete. Porque los metapaquetes solo contienen un archivo composer.json y ningún otro código. El código anterior también es el metapaquete completo. Alójelo en un repositorio Git y tendrá un repositorio instalable del compositor de metapaquetes. Requiere un módulo GRA de ejemplo y un módulo de terceros, así como el núcleo de Adobe Commerce. También requiere la gra-component-foundation, que se explicará en el capítulo siguiente.

Los metapaquetes son una forma de empaquetar paquetes sin crear dependencias entre ellos. Por lo tanto, incluso cuando no hay dependencia técnica entre paquetes, con un metapaquete puede hacer que se instalen juntos. Si necesita este metapackage en el proyecto, se instalará cualquier paquete o metapackage que requiera el metapackage. Por lo tanto, si crea un proyecto de Composer en blanco y solo necesita este paquete, Composer instala Adobe Commerce, GRA y el módulo de terceros.

De este modo, puede asegurarse de que cada tienda contenga el mismo conjunto de paquetes base.

Puede definir de forma similar un metapaquete que defina la x del almacén. Requiere el metapaquete base, que requiere la base GRA completa, además de un módulo local:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

El metapaquete Brand-X es opcional. También puede omitir el metapaquete de marca y requerir estas dependencias directamente en su proyecto de Compositor de tiendas. La ventaja de crear un metapaquete para módulos locales es que no tiene ramas de funciones ni solicitudes de extracción de funciones en el repositorio de Git de almacenamiento, solo en los repositorios de paquetes. Es una medida de seguridad. Además, puede optar por aplicar el control de versiones semántico en los repositorios de paquetes y utilizar diferentes etiquetas Git en el proyecto principal, por ejemplo, para rastrear versiones con nombre. Depende de ti.

### Archivos de base GRA fuera del directorio de proveedores

A veces, es necesario almacenar los archivos fuera del directorio del proveedor. Como `.gitignore`, archivos que van al directorio `dev/` o archivos de verificación de dominio. El tipo de paquete de componentes magento2 está diseñado para este fin. Mire <https://github.com/AntonEvers/gra-component-foundation>.

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

Este paquete tiene el tipo magento2-component y contiene un directorio src que aloja los archivos que se copian en el directorio raíz de Adobe Commerce. La asignación de este archivo copia `/src/gitignore` a `/.gitignore` en el proyecto Composer principal.

De este modo, también puede incluir archivos fuera del directorio de proveedores en la base de GRA.

### Desarrollar un módulo de base GRA

El desarrollo se produce dentro del directorio de proveedores. Pida al Compositor que instale los paquetes de base desde el origen. Al hacerlo, extrae paquetes de Git en lugar de instalarlos desde un archivo descargado.

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

Con este comando, los paquetes del área de nombres antonevers se han desprotegido mediante Git. Al introducir el directorio provider/antonevers/module-gra, también está introduciendo el repositorio Git module-gra. Ahora puede crear, desproteger y combinar ramas directamente desde el directorio de proveedores, y así desarrollarlas.

### Incluir módulos de terceros en la base de GRA

Agregue paquetes de terceros al metapaquete GRA. Si el código de terceros no está disponible para su instalación en un repositorio de Composer, cree un paquete para él. Cree un repositorio Git, añada el contenido de los paquetes (todo lo que estaría en app/code/Vendor/Package) y asegúrese de que haya un archivo composer.json válido en la raíz del repositorio. Ahora puede instalar este paquete mediante Composer.

## Configuración de un repositorio privado de Composer

Un repositorio privado no es obligatorio en la arquitectura de referencia global. Hace que las implementaciones y la instalación sean más rápidas, reduce la configuración del repositorio en composer.json y aumenta la seguridad. Las credenciales de otros repositorios de Composer y Adobe Commerce Marketplace se almacenan en el repositorio privado. No hay más credenciales confidenciales empaquetadas con el código o en los equipos de los desarrolladores.

Además, algunos repositorios privados ofrecen funcionalidades adicionales, como notificaciones por correo electrónico cuando uno de sus almacenes contiene una vulnerabilidad de seguridad en una de sus dependencias.

El problema de lentitud es lo que ocurre cuando tiene varios repositorios VCS en composer.json. Cada repositorio de Composer debe leerse al realizar actualizaciones y tener 50 repositorios para 50 paquetes tiene al menos 50 veces la sobrecarga de un único repositorio de Composer.

![Diagrama que muestra dónde se produce la lentitud cuando falta un repositorio de composición](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

Incluya un reflejo de Composer en forma de repositorio privado de Composer. La duplicación contiene una copia de todos los paquetes de otros repositorios de Composer, así como todos los paquetes alojados en Git. Con un repositorio privado de Composer, también puede obtener un control de acceso preciso.

Con la sincronización de Git, un repositorio privado de Composer detecta automáticamente nuevos paquetes en sus repositorios Git, así como nuevas versiones de paquetes existentes.

Puede alojar su propio repositorio privado con Satis: <https://composer.github.io/satis/>. Vea un ejemplo de repositorio público en <https://antonevers.github.io/gra-composer-repository/>. Este repositorio se utiliza como repositorio del compositor en los ejemplos de código. Se necesitan medidas adicionales para hacer privado un repositorio Satis.

Hay soluciones que puede configurar y olvidar: Private Packagist <https://packagist.com/>, creado por las mismas personas que escribieron Composer o JFrog Artifactory <https://jfrog.com/artifactory/>.

## Código de envío

Con los metapaquetes, hay 3 pasos para entregar el código.

1. Combine los cambios en paquetes y cree una versión de los paquetes modificados.
2. (Opcional, solo si se agregan nuevos paquetes) Requiere los nuevos paquetes en los metapaquetes y la versión de los metapaquetes.
3. (Opcional, solo si se agregan paquetes nuevos) Requiera los nuevos metapaquetes en Adobe Commerce e implemente.

El ámbito de implementación se controla con las versiones de los paquetes. La creación de una versión estable de un paquete significa que este está listo para la implementación de producción.

Para generar una nueva versión, ejecute composer update en el proyecto principal de Composer, que contiene la instalación completa del almacén. Se instalan todas las versiones más recientes de los paquetes.

## Versiones

El control de versiones en un GRA de paquetes separados es un sinónimo de etiquetado de módulos en Git. Las etiquetas Git crean versiones numeradas de los paquetes que instala Composer.
El método de versiones adecuado permite que los paquetes fluyan automáticamente, sin dejar de ser seguros.

Dos ejemplos:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

Este ejemplo muestra una definición estricta de dependencias. Se requieren 3 paquetes en versiones exactas. La actualización del Compositor con este metapaquete en su instalación no hace nada. Siempre instala estos 3 paquetes en estas versiones exactas, incluso si hay una versión más reciente disponible.

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

Este ejemplo muestra una definición flexible de dependencias. Con `~1.0`, cualquier versión de estos paquetes se puede instalar si es mayor o igual que `1.0.0` y menor que `2.0.0`, con preferencia por la versión más alta disponible. Puede obtener más información acerca de cómo definir dependencias de versión en <https://getcomposer.org/doc/articles/versions.md>:

> El operador ~ se explica mejor con un ejemplo: `~1.2` equivale a `>=1.2 <2.0.0`, mientras que `~1.2.3` equivale a `>=1.2.3 <1.3.0`.

Tan pronto como publique una nueva versión de cualquiera de los paquetes mencionados, se instalará automáticamente con Composer update.

Aplicar versiones semánticas. Puede aprender todo sobre las versiones semánticas en <https://semver.org/>. En especial, las preguntas frecuentes son de lectura obligatoria. Con las versiones semánticas, los números de &quot;1.0.0&quot; se denominan MAJOR.MINOR.PATCH. Las versiones menores y de parches de un paquete deben introducirse con seguridad sin romper la aplicación.
Puede incluir parches automáticamente y elegir actualizaciones menores de forma manual. Tenga en cuenta que, al hacerlo, se generan costes generales adicionales al seleccionar manualmente cada cambio menor:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

Por supuesto, todo esto solo funciona si aplica el control de versiones semántico de forma consistente, siempre. Y no solo en los metapaquetes, sino también en los requisitos de los paquetes normales, debe definir las dependencias de forma flexible. Si tiene una dependencia estricta en el sistema, ese paquete se limita a la definición estricta.

Encuentre estas dependencias estrictas escribiendo composer depends \&lt;nombre del paquete\>. Consulte <https://getcomposer.org/doc/03-cli.md#depends-why> para obtener más información.

## Estrategia de ramas

Puede utilizar varias estrategias de ramificación para admitir este patrón de estrategia de referencia global, siempre que la rama principal sea la única en la que realice la versión de los paquetes. Si realiza la versión en varias ramas, existe el riesgo de perder la funcionalidad aleatoriamente entre versiones. Cree solo versiones estables en la rama principal.

Cree únicamente ramas de funciones en los repositorios de paquetes. No en los repositorios de instalación de la tienda. Sigue siendo capaz de introducir cualquier cambio en tu tienda con solo utilizar Composer. Evite la necesidad de combinar Git en el repositorio de implementación.

Tipos de ramas que son comunes en las estrategias de ramificación y repositorios en los que deberían existir:

**Ramas de características**: existen en sus repositorios de paquetes, en ningún otro lugar.

**Ramas de versiones**: se crean en cualquier repositorio: paquetes, metapaquetes, almacenen repositorios de instalación. Cuando planifique una versión, agrupe los cambios en las ramas de versiones de los paquetes antes de realizar el control de versiones. Imagine que está preparando una versión con el nombre en código &quot;Unicornio&quot;. Puede crear una rama release-unicorn en paquetes con cambios. Combine cualquier cosa allí y luego requiera &quot;dev-release-unicorn as 1.4.0&quot; en su metapaquete. Obtenga más información acerca de los alias para ver lo que está sucediendo allí: <https://getcomposer.org/doc/articles/aliases.md>.

**ramas de control de calidad/desarrollo**: similares a las ramas de versiones.

**Rama principal**: debe existir en todos los repositorios y siempre debe ser la rama que represente el estado de producción o listo para la producción. La rama principal es donde se etiqueta el código para publicar versiones.
Asegúrese de elegir una estrategia de ramificación con poca sobrecarga de mantenimiento. Por ejemplo, volver a combinar la rama principal en las ramas QA, UAT, release o dev después de una versión de revisión es una tarea de mantenimiento general. Cuantos más paquetes, más repositorios y tareas generales más repetitivas.

Use una herramienta como mixu/gr para realizar operaciones rutinarias en varios repositorios Git en un lote: <https://github.com/mixu/gr>

## Convenciones de nomenclatura

Con el patrón GRA de Paquetes separados, los paquetes son parte de la base GRA si el metapaquete de base los requiere. Agregar o quitar paquetes del metapaquete para moverlos dentro y fuera de la base.

Los metapaquetes proporcionan flexibilidad al ámbito de instalación de los paquetes. Es de suma importancia que los nombres de los envases no contengan ninguna palabra que se refiera al uso previsto del envase. El nombre antonevers/module-gra-store-locator puede resultar confuso cuando decide sacar ese paquete de la base de GRA. Evite el ámbito (GRA, base, local). Evite la región (EMEA, España, Global). Evite definitivamente el nombre de la tienda para la que está creado un paquete. Elija nombres que solo estén relacionados con la funcionalidad agregada en el paquete. De esta forma podrás reutilizarlos donde quieras, también en escenarios futuros imprevistos. El nombre antonevers/module-store-locator sería excelente.

Asegúrese de que los paquetes relacionados se muestren juntos en las descripciones generales. Generar nombres de genéricos a específicos. Entonces, antonevers/module-b2b-tax-exento en lugar de antonevers/tax-exento-module-b2b.

## Ejemplos de código

Los ejemplos de código de esta publicación de blog se han combinado en un conjunto de repositorios Git, que puede utilizar para jugar con la prueba de concepto.

- Un almacén de producción de ejemplo: <https://github.com/AntonEvers/gra-separate-brand-x>
- Un ejemplo de módulo base: <https://github.com/AntonEvers/module-example-gra>
- Un ejemplo de módulo de terceros: <https://github.com/AntonEvers/module-example-3rdparty>
- Ejemplo de módulo local: <https://github.com/AntonEvers/module-example-local>
- Un ejemplo de metapaquete base: <https://github.com/AntonEvers/gra-meta-foundation>
- Un ejemplo de metapaquete local (opcional): <https://github.com/AntonEvers/gra-meta-brand-x>
- Un ejemplo de repositorio del Compositor: <https://github.com/AntonEvers/gra-composer-repository>

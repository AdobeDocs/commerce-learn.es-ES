---
title: Creación de un módulo
description: Cree una página que devuelva json con un parámetro.
topic: Development
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: fb2cb5b844c4156802e8627e71fc18f8e7fa981d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Creación de un módulo

El módulo es un elemento estructural de [!DNL Commerce] - todo el sistema está construido sobre módulos. Normalmente, el primer paso para crear una personalización es crear un módulo.

## ¿Para quién es este vídeo?

- Desarrolladores

## Pasos para añadir un módulo

- Cree la carpeta del módulo.
- Cree el archivo etc/module.xml.
- Cree el archivo registration.php.
- Ejecute la configuración de bin/magento.
- Actualice el script para instalar el nuevo módulo.
- Compruebe que el módulo funciona.

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## Recursos útiles

- [Guía de referencia del módulo](https://developer.adobe.com/commerce/php/module-reference/)

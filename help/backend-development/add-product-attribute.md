---
title: Crear un atributo de producto
description: Cree una página que devuelva json con un parámetro.
kt: 14131
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, User
level: Beginner, Intermediate
exl-id: 98257e62-b23d-4fa9-a0eb-42e045c53195
source-git-commit: d6aeac0c4c66bd8117cc9ef1e0186bbb19cf23e9
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# Crear un atributo de producto

Agregar un atributo de producto es una de las operaciones más populares en [!DNL Commerce]. Los atributos son una forma eficaz de resolver muchas tareas prácticas relacionadas con un producto. Hay un proceso sencillo para añadir un atributo de tipo desplegable a un producto.

En este vídeo:

- Agregue un atributo llamado ropa_material con los valores posibles: Algodón, Cuero, Seda, Vaquero, Piel y Lana
- Haga que este atributo esté visible en la página de vista del producto con el texto en negrita
- Asígnelo al conjunto de atributos predeterminado y agregue una restricción
- Añadir el nuevo atributo

## ¿Para quién es este vídeo?

- Desarrolladores nuevos en el comercio que necesitan aprender a crear un atributo de producto mediante programación

## Contenido de vídeo

>[!VIDEO](https://video.tv.adobe.com/v/3412438?quality=12&learn=on&captions=spa)

## Ejemplo de código

Primero cree las carpetas, archivos xml y PHP que sean necesarios:

- app/code/Learning/ClothingMaterial/registration.php
- app/code/Learning/ClothingMaterial/etc/module.xml
- app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php
- app/code/Learning/ClothingMaterial/Setup/InstallData.php

### app/code/Learning/ClothingMaterial/registration.php

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Learning_ClothingMaterial',
    __DIR__);
```

### app/code/Learning/ClothingMaterial/etc/module.xml

>[!NOTE]
>
>Si el módulo utiliza un esquema declarativo y la mayoría lo ha hecho desde la versión 2.3.0, debe omitir setup_version. Sin embargo, si tiene algunos proyectos heredados, puede ver este método utilizado.  Consulte [developer.adobe.com](https://developer.adobe.com/commerce/php/development/build/component-name/#add-a-modulexml-file){target="_blank"} para obtener más información.\
>POR FAVOR NOTA: para que este código de ejemplo funcione, usted necesita incluir la setup_version de lo contrario el InstallData.php no se ejecuta.



```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Learning_ClothingMaterial" setup_version="0.0.1"/>
</config>
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php

>[!NOTE]
>
>Asegúrese de utilizar el ID del conjunto de atributos que se encuentra en el proyecto; en este ejemplo, es el número 9.

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Backend;

use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\Backend\AbstractBackend;
use Magento\Framework\Exception\LocalizedException;

class Material extends AbstractBackend
{
    /**
     * @param Product @object
     * @throws LocalizedException
     */
    public function validate($object)
    {
        $value =$object->getData($this->getAttribute()->getAttributeCode());
        // Be sure to validate that your ID number it is likely to be different

        if (($object->getAttributeSetId() == 9) && ($value == 'fur')) {
            throw new LocalizedException(__('Bottoms cannot be fur'));
        }
    }
}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Frontend;

use Magento\Eav\Model\Entity\Attribute\Frontend\AbstractFrontend;
use Magento\Framework\DataObject;

class Material extends AbstractFrontend
{

    public function getValue(DataObject $object): string
    {
        $value = $object->getData($this->getAttribute()->getAttributeCode());
        return "<b>$value</b>";
    }

}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Source;

use Magento\Eav\Model\Entity\Attribute\Source\AbstractSource;

class Material extends AbstractSource
{
    /**
     * Get all options
     *
     * @retrun array
     */
    public function getAllOptions(): array
    {
       if(!$this->_options){
           $this->_options = [
               ['label'    => __('Cotton'),     'value' => 'cotton'],
               ['label'    => __('Leather'),    'value' => 'leather'],
               ['label'    => __('Silk'),       'value' => 'silk'],
               ['label'    => __('Fur'),        'value' => 'fur'],
               ['label'    => __('Wool'),       'value' => 'wool'],
           ];
       }
       return $this->_options;
    }
}
```

### app/code/Learning/ClothingMaterial/Setup/InstallData.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Setup;

use Learning\ClothingMaterial\Model\Attribute\Frontend\Material as Frontend;
use Learning\ClothingMaterial\Model\Attribute\Source\Material as Source;
use Learning\ClothingMaterial\Model\Attribute\Backend\Material as Backend;
use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface;
use Magento\Framework\Setup\InstallDataInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class InstallData implements InstallDataInterface
{
    protected $eavSetupFactory;

    public function __construct(
        \Magento\Eav\Setup\EavSetupFactory $eavSetupFactory
    )
    {
        $this->eavSetupFactory = $eavSetupFactory;
    }

    /**
     * @param ModuleDataSetupInterface $setup
     * @param ModuleContextInterface $context
     * {@inheritDoc}
     *
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     * @suppressWarnings(PHPMD.ExessiveMethodLength)
     * @SuppressWarnings(PHPMD.NPathComplexity)
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $eavSetup = $this->eavSetupFactory->create();
        $eavSetup->addAttribute(
            Product::ENTITY,
            'clothing_material',
            [
                'group'         => 'Product Details',
                'type'          => 'varchar',
                'label'         => 'Clothing Material',
                'input'         => 'select',
                'source'        => Source::class,
                'frontend'      => Frontend::class,
                'backend'       => Backend::class,
                'required'      => false,
                'sort_order'    => 50,
                'global'        => ScopedAttributeInterface::SCOPE_GLOBAL,
                'is_used_in_grid'               => false,
                'is_visible_in_grid'            => false,
                'is_filterable_in_grid'         => false,
                'visible'                       => true,
                'is_html_allowed_on_frontend'   => true,
                'visible_on_front'              => true,
            ]
        );
    }
}
```

## Recursos útiles

[Agregar un atributo de campo de texto personalizado](https://developer.adobe.com/commerce/php/tutorials/admin/custom-text-field-attribute/)

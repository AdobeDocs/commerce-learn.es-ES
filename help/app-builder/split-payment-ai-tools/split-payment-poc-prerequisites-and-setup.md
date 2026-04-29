---
title: 'POC de pagos divididos: requisitos previos y configuración del entorno'
description: Obtenga información sobre cómo configurar Commerce, Admin para COD y crédito de tienda, integración de OAuth, eventos de I/O, App Builder y CLI de aio antes de que la compilación de pago dividido solicite.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# POC de pagos divididos: requisitos previos y configuración del entorno

Complete todos los pasos de este tutorial antes de ejecutar cualquiera de las indicaciones de generación. La falta de un solo paso es la razón más común por la que el flujo se interrumpe a mitad del tutorial.

## &#x200B;1. Requisitos de Adobe Commerce

* Adobe Commerce **2.4.5 o posterior** (local o Commerce Cloud)
* Acceso de Git al proyecto de Commerce (agrega un módulo en `app/code/`)
* Acceso a **[!UICONTROL Commerce Admin]**

### Solo Commerce Cloud: habilitar eventos de E/S

Agregue lo siguiente a `.magento.env.yaml` e implemente antes de agregar el módulo:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **Advertencia:** Esta implementación debe finalizar correctamente para que se pueda resolver la dependencia del módulo de eventos de E/S.


## &#x200B;2. Configuración de administración de Commerce

Haz estos pasos antes que nada. El JavaScript de cierre de compra depende de coincidencias de cadena exactas.

### 2 bis. Habilitar Pago contra reembolso con el título exacto

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]**: **Sí**
* **[!UICONTROL Title]**: **`Cash`** (esta cadena exacta es lo que coincide con el JavaScript de cierre de compra)

> **Nota:** Si tu tienda usa una implementación o un título de Pago contra reembolso (COD) diferente, ajusta `payment-method-helper.js` en el módulo de Commerce.

### 2 ter. Habilitar crédito de tienda

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]**: **Sí**

### 2 quater. Añadir crédito de la tienda al cliente de prueba

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[su cliente de prueba]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

Agregar al menos **$50** en el crédito de la tienda. Realizará pruebas con un pedido total inferior a 100 $.

### 2d. Creación de la integración de Commerce

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**: `Split Payment App Builder` (o cualquier nombre que prefiera)
* En la ficha **[!UICONTROL API]**, conceder como mínimo:
   * `Magento_Sales::actions` (requerido para el extremo `cash-received`)
   * `Magento_Sales::cancel` (requerido para el extremo `cash-decline`)
   * Administración de presupuestos o carros de compras (completo o un subconjunto relevante)
   * **[!UICONTROL Customer balance]** (completo o un subconjunto relevante)

Seleccione **[!UICONTROL Save]**, luego **[!UICONTROL Activate]**.

**Copie los cuatro valores; solo se muestran una vez:**

| Valor | Variable de entorno |
| --- | --- |
| Clave de consumidor | `COMMERCE_CONSUMER_KEY` |
| Secreto del consumidor | `COMMERCE_CONSUMER_SECRET` |
| Token de acceso | `COMMERCE_ACCESS_TOKEN` |
| Secreto de token de acceso | `COMMERCE_ACCESS_TOKEN_SECRET` |

Guarde estos valores de forma segura. Los necesita en cada archivo de App Builder `.env`.


## &#x200B;3. ADOBE DEVELOPER CONSOLE y APP BUILDER

* Acceso a una organización de Adobe Developer Console
* Un **proyecto App Builder** (nuevo o que usted reutiliza)
* Espacio de trabajo configurado; los mensajes asumen **[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]** se agregó como servicio al área de trabajo
* **Commerce** se conectó como proveedor de eventos para el espacio de trabajo

El código de evento utilizado en la prueba de concepto es: `com.adobe.commerce.observer.sales_order_place_before`

Si no ha conectado Commerce como proveedor de eventos, consulte [Configuración de Commerce para que emita eventos a Adobe I/O](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"} en la guía de extensibilidad de Commerce.


## &#x200B;4. Entorno de desarrollo local

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## &#x200B;5. Dos directorios de proyecto para conocer

Este tutorial utiliza dos raíces de directorio independientes. Manténgalos separados.

**raíz de proyecto de Commerce** (su repositorio de Git de Magento):

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**Raíz de proyecto de App Builder** (la carpeta `split-payment-orchestrator` del paquete de origen o un nuevo proyecto que cree):

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_id comparado con increment_id

> **Use siempre `entity_id` (el identificador numérico de base de datos), no `increment_id` (por ejemplo `000000042`), en las llamadas REST.**

Buscar `entity_id` de la dirección URL del pedido **[!UICONTROL Commerce Admin]**:

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

O desde el script de simulación:

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. El umbral de 100 dólares

La interfaz de usuario de pago dividido y el objetivo de protección de umbral solicitan **$100.00 o menos**. El flujo se comporta de la siguiente manera:

* **A 100 $ o menos:** aparece la interfaz de usuario de pago dividido; el cliente puede establecer una división de efectivo más crédito de la tienda
* **Por encima de 100 $:** `CheckoutPlugin` genera un error en la etapa de pago

Para probar, genere un carro de compras cuyo subtotal, gastos de envío e impuestos sean **menores o iguales a $100** (por ejemplo, un producto menor de $90 por lo que los gastos de envío e impuestos aún caben debajo del límite).

El umbral se almacena en:

* Configuración de Commerce: `split_payment/general/threshold` (predeterminado: `100` en `etc/config.xml`)
* Entorno App Builder: `PAYMENT_THRESHOLD=100` (debe coincidir con Commerce)


## &#x200B;8. Rápidamente (solo Commerce Cloud)

Las acciones de App Builder llaman a Commerce sobre REST (`/rest/V1/split-payment/orders/...`). Si el proyecto de Commerce Cloud utiliza Fastly con inclusión en la lista de permitidos IP, las direcciones IP de salida de tiempo de ejecución de App Builder deben estar incluidas en la lista de permitidos.

**Comprobación:** Ejecute primero el script de simulación (curl directo con firma OAuth). Si esto funciona pero la acción App Builder devuelve `403`, es probable que Fastly esté bloqueando la solicitud.

Utilice la documentación actual de Adobe para los intervalos de IP de salida de App Builder y añádalos a la configuración de Fastly según sea necesario.


## Lista de comprobación de verificación

Antes de iniciar las peticiones de datos de la versión, confirme lo siguiente:

* [ ] versión de Commerce 2.4.5 o posterior
* [ ] eventos de E/S habilitados (Commerce Cloud) e implementados
* [ ] La entrega contra reembolso está habilitada con el título establecido exactamente en `Cash`
* [ ] El crédito de la tienda está habilitado
* [ ]: el cliente de prueba tiene un crédito de tienda de al menos 50 dólares.
* [ ]: se crea y activa la integración de Commerce y se guardan los cuatro valores de OAuth
* [ ]: el proyecto de App Builder tiene configurados el servicio Eventos de E/S y el proveedor de eventos de Commerce
* [ ] `aio login` se ha completado y el área de trabajo correcta se ha seleccionado con `aio app use`
* [ ] Node.js 18 o posterior está instalado y la CLI `aio` está instalada
* [ ] `.env` archivos se preparan por [POC de pago dividido: referencia de variables de entorno](split-payment-poc-env-reference.md) (y su paquete de origen, si lo usa)


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}

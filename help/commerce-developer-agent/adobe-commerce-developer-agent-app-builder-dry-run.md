---
title: Ejecución en seco de Adobe Commerce Developer Agent App Builder
description: Obtenga información sobre cómo crear, implementar y probar tres casos de uso de extensibilidad de Commerce con Adobe Commerce Developer Agent en esta ejecución práctica de App Builder.
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 92af5355fa31c1ce9e627679b0a1bb92cce0e1d8
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---

# Ejecución en seco de Adobe Commerce Developer Agent App Builder

Un tutorial práctico para crear, implementar y probar casos de uso de extensibilidad de Commerce con Adobe Commerce Developer Agent (CDA). Esta ejecución en seco cubre tres casos de uso: un webhook de límite de cantidad de carritos, una retención de pedidos de alto valor y un archivado controlado por evento para pedidos retenidos, desde el modelo hasta las pruebas funcionales.

## Primeros pasos

### Cómo informar sobre problemas y comentarios

A lo largo de la ejecución en seco, se encuentran con bordes ásperos, lo que se espera mientras se trabaja con una nueva función. Capture y comparta cualquier problema con el contacto del programa de Adobe mediante la plantilla de comentarios proporcionada durante la incorporación.

>[!TIP]
>
> Al informar de un problema:
>
> * Incluya `projectId` (visible en la dirección URL del explorador).
> * Incluya capturas de pantalla siempre que sea relevante.

### Requisitos previos

**Cuentas y acceso**

* Al menos el rol de **Desarrollador** en su organización IMS de acceso anticipado.
* Acceso de administrador a una instancia de Adobe Commerce as a Cloud Service (ACCS) dentro de esa organización, disponible en experience.adobe.com en **Instancias de Cloud Service**.
* Una cuenta de GitHub.

**Herramientas**

Se requiere una tienda de Edge Delivery Services (EDS) para la validación funcional. Necesitará lo siguiente:

* Node.js 22+
* CLI de Adobe I/O: `npm install -g @adobe/aio-cli`
* Complemento Commerce de CLI de AIO: `aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

Instale la plantilla de la tienda en una carpeta vacía y seleccione la instancia de ACCS cuando se le solicite:

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

Inicia la tienda:

```bash
cd storefront
npm run start
```

## Abrir Commerce Developer Agent

1. Vaya a Commerce Developer Agent en experience.adobe.com, en **Agente para desarrolladores**.
1. Inicie sesión con sus credenciales de organización de IMS de acceso anticipado.

## Caso de uso 1: gancho web de unidades máximas del carro de compras

Este caso de uso valida los límites de cantidad del carro de compras antes de agregar un producto, mediante un webhook sincrónico de Commerce.

### Fase de modelo

Escriba el siguiente mensaje y haga clic en **Generar modelo**:

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> Busque lo siguiente:
>
> * Se crea un modelo (v1) que captura los requisitos.
> * Se crean tareas para guiar la implementación.

Refine el modelo introduciendo detalles en el cuadro de chat o haciendo clic en una de las píldoras encima del cuadro de chat (*Suposiciones de desafío*, *Encuentre lagunas de diseño*, etc.). Una vez que esté satisfecho, haga clic en **Aprobar plan** para continuar.

### Fase de desarrollo

El agente pasa a la fase de desarrollo y comienza a aprovisionar el espacio de trabajo.

>[!NOTE]
>
> Busque estos archivos en el panel Explorador:
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

Una vez aprovisionado, el agente muestra una lista de tareas de implementación y comienza a crear.

>[!NOTE]
>
> Busque lo siguiente:
>
> * El código generado coincide con los requisitos de.
> * La pantalla de transmisión `Validate` muestra el progreso en la validación del área de trabajo (`aio app build`).
> * El agente autocorrige el código generado si la validación falla.

Una vez que esté satisfecho con el código, haga clic en la ficha **Integraciones** para continuar.

### Configuración de integraciones

**Conectar o crear un espacio de trabajo de App Builder**

Para crear o conectar un proyecto de App Builder, siga las instrucciones que aparecen en pantalla.

Si se conecta a un espacio de trabajo existente, asegúrese de que tenga:

* Se agregó el servicio `Runtime`.
* Se agregaron las siguientes API: Adobe Commerce as a Cloud Service, API de administración de E/S, servicios de datos de App Builder, eventos de E/S, Adobe I/O Events para Adobe Commerce.

Si crea un nuevo espacio de trabajo, agregue manualmente la API **Adobe Commerce as a Cloud Service**.

>[!IMPORTANT]
>
> Una vez que se haya conectado a un proyecto existente de App Builder, expanda **Configuración avanzada** y pegue el espacio de trabajo JSON; a continuación, haga clic en **Volver a comprobar el estado** para confirmar que todas las API requeridas están instaladas.

Haga clic en **Siguiente** para continuar.

**Conectarse a Commerce**

Seleccione la instancia de ACCS de la lista o escriba la dirección URL en el campo **URL base de REST de Commerce** y, a continuación, haga clic en **Conectar la instancia de Commerce**. Haga clic en **Siguiente** para continuar.

**Conectarse a GitHub**

Conecte el espacio de trabajo a un repositorio de GitHub introduciendo la URL del repositorio y utilizando la aplicación de GitHub o un token de acceso personal. Haga clic en **Siguiente** para continuar.

**Configurar variables de entorno**

Complete las variables de entorno que requiera el proyecto.

### Implementar

Haga clic en **Desarrollo** para regresar a la fase de desarrollo y, a continuación, solicite al agente que realice el despliegue en el campo de solicitud.

>[!NOTE]
>
> Busque el mensaje &quot;Confirm deployment&quot; que muestra el área de nombres Organization, Project, Workspace y Runtime.

Confirme la implementación.

>[!NOTE]
>
> Busque:.
>
> * La pantalla de streaming `Validate` muestra el progreso de validación previo a la implementación.
> * El agente corrige automáticamente el código si la validación falla.
> * La pantalla de transmisión `Deploy` muestra el progreso de implementación (`aio app deploy`).
> * El agente corrige automáticamente el código si falla el despliegue.

### Asociar la aplicación en la administración de aplicaciones

1. Vaya a la URL de administración de la instancia ACCS e inicie sesión.
1. Seleccione **Aplicaciones** en el menú de la izquierda, luego **Administración de aplicaciones**.
1. Haga clic en **+ Asociar aplicación** (parte superior derecha).
1. Seleccione el proyecto y el Workspace en los que CDA implementó y, a continuación, haga clic en **Asociar**.

>[!NOTE]
>
> Busque una tarjeta que muestre el nombre y la versión de la aplicación, y las capacidades implementadas (Configuración empresarial, Webhooks, Eventos, etc.).

### Instalación y configuración en Administración de aplicaciones

1. En la fila de la aplicación, haz clic en **Instalar** y luego en **Cerrar**.
1. En la misma fila, haga clic en **Configurar** para rellenar los valores de configuración empresarial y, a continuación, **Cerrar**.

>[!NOTE]
>
> Busque un formulario que muestre todos los campos de configuración especificados por el modelo, rellenados previamente con los valores predeterminados especificados.

### Pruebas funcionales

1. En la configuración de la aplicación App Management, establezca **Unidades máximas del carro de compras** en 3 (un valor bajo para una prueba rápida).
1. En la tienda, comienza con un carro vacío.
1. Añadir productos desde la página de detalles del producto (PDP) hasta que la cantidad total supere 3 (se produce un error en la última adición).
1. En PDP, verá: *&quot;Ha alcanzado la cantidad máxima de elementos.&quot;*
1. Por debajo del límite, las adiciones siguen teniendo éxito.

>[!NOTE]
>
> Desde la página de lista de productos (PLP), un anuncio bloqueado falla silenciosamente sin mensaje: se trata de un comportamiento de tienda, no de un error de gancho web. Prefiera el PDP para la verificación.

## Caso de uso 2: retención de pedido de alto valor y código de verificación

Vuelva a la fase **Modelo** para comenzar este caso de uso.

### Fase de modelo

Escriba el siguiente mensaje y haga clic en **Generar modelo**:

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> Busque lo siguiente:
>
> * Se crea un modelo (v2) que captura los requisitos.
> * Las tareas del plan original se conservan.
> * Se han añadido nuevas tareas correspondientes a los nuevos requisitos.

Refine el modelo según sea necesario y luego haga clic en **Aprobar plan** para continuar.

### Desarrollar, implementar, asociar e instalar

Siga el mismo proceso utilizado en el Caso de uso 1 para pasar de los requisitos a una aplicación instalada; no es necesario volver a configurar las integraciones.

>[!IMPORTANT]
>
> Para aplicar cambios a una aplicación que ya está asociada, debes **Desasociar** y **Asociar** de nuevo en la administración de aplicaciones.

### Pruebas funcionales

1. En la configuración de la aplicación App Management, establezca **Umbral de retención de pedidos (USD)** en 50 (fácil de superar en un carro de pruebas).
1. Confirme que existe el atributo personalizado order (predeterminado `lab_verification_code`).
1. Realice un pedido con un total de más de 50 $.
1. Espere ~30 segundos (los eventos son asíncronos; la entrega sin prioridad puede tardar hasta ~59s).
1. En Administración de Commerce → Pedidos de → de ventas, abra el pedido. El estado es **En espera** (`holded`); los atributos personalizados incluyen `lab_verification_code` con un valor aleatorio.
1. Opcional: primero haga un pedido inferior a 50 $; este controlador no lo pone en espera.

## Caso de uso 3: archivado basado en eventos para pedidos retenidos

Vuelva a la fase **Modelo** para comenzar este caso de uso.

### Fase de modelo

Escriba el siguiente mensaje y haga clic en **Generar modelo**:

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> Busque lo siguiente:
>
> * Se crea un modelo (v3) que captura los requisitos.
> * Las tareas del plan original se conservan.
> * Se han añadido nuevas tareas correspondientes a los nuevos requisitos.

Refine el modelo según sea necesario y luego haga clic en **Aprobar plan** para continuar.

### Desarrollar, implementar, asociar e instalar

Siga el mismo proceso utilizado en los casos de uso anteriores para pasar de los requisitos a una aplicación instalada; no es necesario volver a configurar las integraciones.

>[!IMPORTANT]
>
> Para aplicar cambios a una aplicación que ya está asociada, debes **Desasociar** y **Asociar** de nuevo en la administración de aplicaciones.

### Pruebas funcionales

1. Asegúrese de que el umbral del Caso de uso 2 sea lo suficientemente bajo para realizar pruebas (por ejemplo, 50 $ en la configuración de la aplicación).
1. Realice un pedido por encima de ese umbral para que el Caso de uso 2 lo ponga en espera (~30 segundos).
1. En Adobe Developer Console → su proyecto → Ensayo → eventos, abra el registro para el evento de archivado por orden de retención (añadido o actualizado en la instalación).
1. Confirme que se ha entregado un evento a ese registro después de que el pedido se haya movido a la espera. Use el seguimiento de eventos o la supervisión del evento de Commerce vinculado a `order-archive/archive-held-order`.

>[!NOTE]
>
> Los eventos son asincrónicos: espere entre 30 y 59 segundos tras poner el pedido en espera.

## Resolución de problemas

Si la aplicación generada por CDA no se comporta como se espera o produce errores, pídale al agente que solucione los problemas desde la fase de desarrollo.

>[!NOTE]
>
> El análisis multidispositivo no tiene visibilidad de los pasos que se producen fuera de él. Las pruebas Asociar, Instalar, Configurar y Funcional se ejecutan en el Administrador de Commerce, en la Administración de aplicaciones o en la tienda, no en CDA. Si un problema aparece en una de esas áreas, el agente no puede verlo ocurrir, así que asígnele lo que falta:
>
> * Qué ha hecho y dónde (por ejemplo, &quot;ha hecho clic en Instalar en la administración de aplicaciones&quot;).
> * Lo que esperabas que pasara.
> * ¿Qué pasó?
> * El texto o mensaje de error exacto que se muestra en la pantalla.
> * Cualquier error relevante de la consola del explorador o de los registros de App Builder de Adobe Developer Console y los seguimientos de depuración del registro de eventos.

Cuanto más concreto sea el informe, mejor podrá el agente diagnosticar el problema.

## Pasos opcionales

**Descargar el código**

Para seguir refinando o editando en su IDE favorito, descargue el código generado por CDA haciendo clic en el icono de descarga en la barra de herramientas del Explorador de fase de desarrollo. Seleccione una carpeta de destino, haga clic en **Guardar** y luego descomprima el paquete del espacio de trabajo.

>[!NOTE]
>
> Busque:.
>
> * Todos los archivos mostrados en el Explorador de fase de desarrollo están presentes en la carpeta descomprimida.
> * No hay errores de &quot;compilación&quot; al generar el proyecto con `aio app build`.

Para utilizar las mismas habilidades de agente que utiliza CDA, instálelas en la carpeta del proyecto:

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

A continuación, inicie el IDE o CLI y comience a solicitar ayuda.

**Adjuntar contexto mediante archivo o vínculo**

En lugar de preguntar directamente en las fases de modelo o desarrollo, puede adjuntar un contexto mediante un archivo de texto o un vínculo:

1. Haga clic en el icono de datos adjuntos en el cuadro de diálogo.
1. Haga clic en **Agregar archivo** para cargar un archivo de texto local, o bien escriba una dirección URL y haga clic en **Agregar vínculo** para agregar contexto a través de un archivo remoto.
1. Haga clic en **Listo** e introduzca un mensaje para empujar al agente.

>[!NOTE]
>
> Busque el agente que incorpore el contexto de sus archivos adjuntos en su siguiente turno.

## Problemas y soluciones conocidos

**La fase de modelo no genera tareas**

Para desbloquearlo y continuar, empuje al agente a generar tareas.

**Los botones para insertar y extraer de GitHub no funcionan**

En su lugar, descargue el archivo ZIP del proyecto desde la fase Desarrollo.

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

## Recursos adicionales

* [Información general de Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/)
* [Introducción a Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/getting-started)
* [Sugerencias para solicitar Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/prompting)
* [Asistencia y comentarios sobre Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/support)

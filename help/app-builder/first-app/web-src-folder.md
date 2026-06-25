---
title: La carpeta web-src
description: Obtenga información sobre la estructura de carpetas web-src, sus archivos JavaScript y carpetas anidadas, y cómo esta carpeta admite la interfaz de usuario en la aplicación de ejemplo de App Builder.
jira: KT-21683
doc-type: Tutorial
duration: 285
last-substantial-update: 2023-03-13T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 67bbb464-1c2e-493e-9d7f-1051dfeec4ee
TQID: https://experienceleague.adobe.com/SavAwkQQnKIE64PlRHPKPU9RBT37dWsLm8W7FXYm9Mc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 179
ht-degree: 0%

---

# Descubra el propósito de la carpeta web-src {#web-src-folder}

La carpeta web-src de esta aplicación de ejemplo contiene muchos archivos y carpetas de JavaScript. Esta carpeta se utiliza para aplicaciones que tienen una interfaz de usuario. No todas las aplicaciones utilizan esta característica. Por ejemplo, una integración de Commerce con un sistema de administración de inventario externo no requiere una interfaz de front-end y código.

## ¿Para quién es este vídeo?

* Desarrolladores nuevos en Adobe Commerce con experiencia limitada que utilizan Adobe App Builder y que están aprendiendo acerca de la carpeta `web-src` y su contenido.

## Contenido de vídeo

* ¿Cuál es el propósito principal de la carpeta `web-src`?
* Archivos y carpetas incluidos normalmente
* Uso de la carpeta `web-src` y del contenido de la aplicación de ejemplo

>[!VIDEO](https://video.tv.adobe.com/v/3416665?learn=on)

## Muestras de código

web-src/src/components/Orders.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
import {
    Content,
    Heading,
    IllustratedMessage,
    TableView,
    TableHeader,
    TableBody,
    Column,
    Row,
    Cell,
    View,
    Flex,
    ProgressCircle
} from '@adobe/react-spectrum'
import {useCommerceOrders} from '../hooks/useCommerceOrders'

export const Orders = props => {

    const {isLoadingCommerceOrders, commerceOrders} = useCommerceOrders(props)

    const ordersColumns = [
        {name: 'Order Id', uid: 'increment_id'},
        {name: 'Status', uid: 'status'},
        {name: 'Store Name', uid: 'store_name'},
        {name: 'Total Item Count', uid: 'total_item_count'},
        {name: 'Total Quantity', uid: 'total_qty_ordered'},
        {name: 'Total Due', uid: 'total_due'},
        {name: 'Tax', uid: 'tax_amount'},
        {name: 'Created At', uid: 'created_at'}
    ]

    function renderEmptyState() {
        return (
            <IllustratedMessage>
                <Content>No data available</Content>
            </IllustratedMessage>
        )
    }

    return (

        <View>
            {isLoadingCommerceOrders ? (
                <Flex alignItems="center" justifyContent="center" height="100vh">
                    <ProgressCircle size="L" aria-label="Loading…" isIndeterminate/>
                </Flex>
            ) : (
                <View margin={10}>
                    <Heading level={1}>Fetched orders from Adobe Commerce</Heading>
                    <TableView
                        overflowMode="wrap"
                        aria-label="orders table"
                        flex
                        renderEmptyState={renderEmptyState}
                        height="static-size-1000"
                    >
                        <TableHeader columns={ordersColumns}>
                            {column => <Column key={column.uid}>{column.name}</Column>}
                        </TableHeader>
                        <TableBody items={commerceOrders}>
                            {order => (
                                <Row key={order['increment_id']}>{columnKey => <Cell>{order[columnKey]}</Cell>}</Row>
                            )}
                        </TableBody>
                    </TableView>
                </View>
            )}
        </View>
    )
}
```

web-src/src/hooks/useCommerceOrders.js

{{avoid-400-error}}

En el ejemplo siguiente, el ejemplo de código es `not` que limita la solicitud. Para evitar un error 400, reduzca el tamaño de la respuesta usando `searchCriteria`.

`?searchCriteria[filter_groups][0][filters][0][field]=created_at&searchCriteria[filter_groups][0][filters][0][value]=2022-12-01&searchCriteria[filter_groups][0][filters][0][condition_type]=gt`

```javascript {line-numbers="true" start-line="1" highlight="25"}
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
import { useEffect, useState } from 'react'
import { callAction } from '../utils'

export const useCommerceOrders = props => {
    const [isLoadingCommerceOrders, setIsLoadingCommerceOrders] = useState(true)
    const [commerceOrders, setCommerceOrders] = useState([])

    const fetchCommerceOrders = async () => {
        const commerceOrdersResponse = await callAction(
            props,
            'commerce-rest-get',
            'orders?searchCriteria=all'
        )
        setCommerceOrders(commerceOrdersResponse.error ? [] : commerceOrdersResponse.items)
    }

    useEffect(() => {
        fetchCommerceOrders().then(() => setIsLoadingCommerceOrders(false))
    }, [])

    return { isLoadingCommerceOrders, commerceOrders }
}
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}


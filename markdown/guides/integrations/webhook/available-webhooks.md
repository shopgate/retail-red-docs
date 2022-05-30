# Available Events

## Content

- [About this Document](#about-this-document)
- [Events](#events)
  - [cartReminderTimeReached](#cartremindertimereached)
  - [customerDeleted](#customerdeleted)
  - [fulfillmentOrderAdded](#fulfillmentorderadded)
  - [fulfillmentOrderStatusUpdated](#fulfillmentorderstatusupdated)
  - [fulfillmentOrderUpdated](#fulfillmentorderupdated)
  - [importCompleted](#importcompleted)
  - [inventoryReservationDeleted](#inventoryreservationdeleted)
  - [inventoryReservationSettled](#inventoryreservationsettled)
  - [locationCreated](#locationcreated)
  - [locationDeleted](#locationdeleted)
  - [locationUpdated](#locationupdated)
  - [orderNotPickedUp](#ordernotpickedup)
  - [productCreated](#productcreated)
  - [productUpdated](#productupdated)
  - [salesOrderAdded](#salesorderadded)
  - [salesOrderFulfillmentAdded](#salesorderfulfillmentadded)
  - [salesOrderStatusUpdated](#salesorderstatusupdated)
  - [schedulePickTimeReached](#schedulepicktimereached)
  - [schedulePickupReminderTimeReached](#schedulepickupremindertimereached)

## About this Document

This document contains the payload of the events supported by the Webhook Service. To see the complete request body structure see [the HTTP Call](./overview.md#body-structure).

## Events

### cartReminderTimeReached

Triggers when a cart is older than a certain timespan. The duration of the timespan can be set in the merchant settings (key: cartReminderTriggerAfterSeconds). This setting is also setable in the admin Marketing > Abandoned Cart Reminder Time setting.

```json
{
  "customerId": "5292bee1-c2e6-4816-a99d-f88834c35775", 
  "deviceCode": "device2", 
  "cartId": "57de7a67-bd83-46e9-8276-c1206c301380", 
  "shopCode": "shop1"
}
```

### customerDeleted

Triggers after a customer was deleted.

```json
{
  "customerId": "5292bee1-c2e6-4816-a99d-f88834c35775",
  "externalCustomerNumber": "500",
  "isAnonymous": false,
  "emailAddress": "test@test.com",
  "requestUser": "5",
  "requestIp": "127.0.0.1"
}
```

### fulfillmentOrderAdded

Triggers after a sales order got routed to a location.

```json
{
  "locationCode": "1",
  "fulfillmentOrderNumber": "1500-0001",
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "salesOrderNumber": "1500",
  "sequenceId": 1,
  "user": "6" //userId
}
```

### fulfillmentOrderStatusUpdated

Triggers on status transition of a fulfillment order.

```json
{
  "locationCode": "1",
  "fulfillmentOrderNumber": "1500-0001",
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "newStatus": "fulfilled",
  "oldStatus": "accepted",
  "user": "6"
}
```

### fulfillmentOrderUpdated

Triggers on any update of a fulfillment order.

```json
{
  "locationCode": "1",
  "fulfillmentOrderNumber": "1500-0001",
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "newProperties": { "status": "fulfilled" },
  "oldProperties": { "status": "accepted" },
  "user": "6"
}
```

```json
{
  "locationCode": "1",
  "fulfillmentOrderNumber": "1500-0001",
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "newProperties": { "status": "canceled", "cancellationReason": "declined" },
  "oldProperties": { "status": "accepted", "cancellationReason": null },
  "user": "6"
}
```

```json
{
  "locationCode": "1",
  "fulfillmentOrderNumber": "1500-0001",
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "newProperties": { "lineItems": [{ "id": 789, "status": "ready"}] },
  "oldProperties": { "lineItems": [{ "id": 789, "status": "accepted"}] },
  "user": "6"
}
```

### importCompleted

Triggers when an import reached its endstate (either succeeded or failed).

```json
{
  "ref": "12345678-1234-5678-1234-567812345678",
  "status": "succeeded|failed"
}
```

### inventoryReservationDeleted

Triggers after a reservation got deleted, most likely after orders got canceled/rejected.

```json
{
  "code": "0cb848b4-d239-4223-8b91-1ddbf04f25b6",
  "locationCode": "1",
  "salesOrderNumber": "1339",
  "fulfillmentOrderNumber": "1339-0001",
  "salesOrderLineItemCode": "line item 1",
  "productCode": "test1",
  "quantity": 3,
  "bin": "1004",
  "binLocation": "AA21004",
  "sku": "sku-1"
}
```

### inventoryReservationSettled

Triggers after a fulfillment order changed into fulfilled.

```json
{
  "code": "0cb848b4-d239-4223-8b91-1ddbf04f25b6",
  "locationCode": "1",
  "salesOrderNumber": "1339",
  "fulfillmentOrderNumber": "1339-0001",
  "salesOrderLineItemCode": "line item 1",
  "productCode": "test1",
  "quantity": 3,
  "bin": "1004",
  "binLocation": "AA21004",
  "sku": "sku-1"
}
```

### locationCreated

Triggers after a location got created.

```json
{
  "locationCode": "ABC",
  "user": "6"
}
```

### locationDeleted

Triggers after a location got deleted.

```json
{
  "locationCode": "ABC",
  "user": "6"
}
```

### locationUpdated

Triggers after a location got updated.

```json
{
  "locationCode": "ABC",
  "newProperties": { "status": "inactive" },
  "oldProperties": { "status": "active" },
  "user": "6"
}
```

### orderNotPickedUp

Triggers after a fulfillment order is in ready state for a specific timespan. The timespan can be set by the pickupReminderTriggerAfterSeconds property via the merchant service api or in the Shopgate admin Settings > General > Fulfillmennt Handling > Pickup Reminder Time.

```json
{
  "locationCode": "1",
  "fulfillmentOrderNumber": "1500-0001",
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "user": "6"
}
```

### productCreated

Triggers after a product was created.

```json
{
  "catalogCode": "DefaultCatalog",
  "productCode": "t-shirt-grey",
  "user": "6"
}
```

### productUpdated

Triggers after a product got updated.

```json
{
  "catalogCode": "DefaultCatalog",
  "productCode": "t-shirt-grey",
  "user": "6"
}
```

### salesOrderAdded

Triggers after a order was created. Typically when a cart turns into an order on checkout.
merchantCode, customerId, salesOrderNumber, salesOrderId, imported, eventDate

```json
{
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "salesOrderId": 5,
  "salesOrderNumber": "0001",
  "imported": false
}
```

### salesOrderFulfillmentAdded

Triggers after a fulfillment order went into ready state.

```json
{
  "salesOrderNumber": "0001",
  "sOFulfillmentGroupId": 2,
  "fulfillmentId": "312",
  "fulfillmentMethod": "directShip",
  "fulfillmentLocationCode": "warehouse",
  "carrier": "DHL",
  "tracking": "JJD000390007882823450",
  "serviceLevel": "sameDay",
  "status": "inProgress"
}
```

### salesOrderStatusUpdated

Triggers after the status of a salesorder changed.

```json
{
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "salesOrderId": 5,
  "salesOrderNumber": "0001",
  "oldStatus": "new",
  "newStatus": "accepted",
  "user": "6" //userId
}
```

### schedulePickTimeReached

Triggers after a fulfillment order is in new state for a specific timespan. The timespan can be set by the scheduledFulfillmentPickTime property via the location service api or in the Shopgate admin.

```json
{ 
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "fulfillmentOrderNumber": "1500-0001",
  "locationCode": "loc1"
}
```

### schedulePickupReminderTimeReached

Triggers after a fulfillment order is in ready state for a specific timespan. The timespan can be set by the scheduledPickupReminderBeforePickupSeconds property via the merchant service api or in the Shopgate admin Settings > General > Fulfillmennt Handling > Scheduled Pickup Reminder Time Before Pickup Time.

```json
{ 
  "customerId": "7b196c58-a689-49fe-b056-91dd902694d2",
  "fulfillmentOrderNumber": "1500-0001",
  "locationCode": "loc1"
}
```

<!-- 
 
[ ] Ship-From-Store concept
[x]     Routing & Fulfillment (can be used separately)
[ ]     Flows (big overview diagram, where later we will dig into detail)
[x]     Different locations that can fulfill
[x]         Warehouses (no fulfillment via app probably)
[x]         Stores
[x]         Dropshippers (no fulfillment via app probably)
[x]     Get Inventory & product data into Shopgate
[x]     Configure Locations & Routes (see support portal)
[x]     Use cumulated inventory in ECP
[x]         Either do themselves, or get aggregated from Shopgate
[ ]     Import new orders to Shopgate & how to handle order update in ECP/OMS
[ ]         Import new orders from ECP / OMS to order API
[ ]         What to do if an order is edited in ECP/OMS?
[ ]             Shipping address changed? 
[ ]             Line items added/removed?
[ ]             Status updates (FO Status/SO Status?)
[ ]     Sync order updates / routing results back (e.g. when order is shipped)
[ ]         Fulfillment done via retail.red or only routing?
[ ]         Both ways:
[ ]             FO status changed in Shopgate (e.g. store associate marked order as “shipped”,
[ ]             FO status changed in external system (e.g. warehouse management system of dropshipper marked order as shipped)

---

[ ]     Special flows
[ ]         Editing order (in Shopgate / in external system)
[ ]         Canceling order (in Shopgate / in external system)
[ ]         Backorders / Preorders

-->

# Overview

## Content

- [Overview](#overview)
  - [Content](#content)
  - [About this Guide](#about-this-guide)
  - [Key Features](#key-features)
    - [Routing](#routing)
    - [Fulfillment](#fulfillment)
    - [Different Ways Of Implementing Ship-From-Store](#different-ways-of-implementing-ship-from-store)
  - [Entities Needed For The Ship-From-Store Implementation](#entities-needed-for-the-ship-from-store-implementation)
    - [Sales Orders / Fulfillment Orders](#sales-orders--fulfillment-orders)
    - [Locations](#locations)
    - [Products](#products)
    - [Inventory](#inventory)
    - [Routes](#routes)
  - [Usage of Cumulated Inventory and Ship-From-Store](#usage-of-cumulated-inventory-and-ship-from-store)
  - [How To Get Basic Data Into The Shopgate Platform](#how-to-get-basic-data-into-the-shopgate-platform)
    - [Locations](#locations-1)
    - [Products](#products-1)
    - [Inventories](#inventories)
  - [Syncing Between Your Platform And Shopgate](#syncing-between-your-platform-and-shopgate)
  - [Configurations](#configurations)
    - [Locations](#locations-2)
    - [Routes](#routes-1)

## About this Guide

The following guide should show you what Ship-From-Store is, how it works, and what you need to set it up within the Shopgate Platform - integrated with your system.

## Key Features

The key features of Ship-From-Store are **Order Routing** and the **Fulfillment Process**. In the upcoming text they will be referenced in their short forms as routing and fulfillment.

Utilizing these features you are able automatically route/split orders to different locations using a customizable rule set as well as fulfilling the orders on site from picking to shipping.

### Routing

The Order Routing describes the creation of [fulfillment orders](../order/overview.md#fulfillment-order) at locations that fit certain criteria like: closest to the customer, or location that have the most stock or other rule sets that you can freely configure within the Shopgate Admin. Not only that - the routing engine is able to split one [sales order](../order/overview.md#sales-order) into multiple fulfillment orders it sees as best fit. Of course this is also configurable. In case a fulfillment order can't get handled on it's assigned location, the Order Routing can reroute the fulfillment order to another location that fits.

In order to be able to decide if an order needs to be routed to a special location or if an order needs to be split to different locations we introduced rule sets called **routes**. You can also set up rules that orders from a certain set of states are only handled by certain locations. You can define how many order splits are allowed and many other conditions and settings via our Admin.

There can be multiple routes, each set with a certain priority. If an order does not fit a route, next route will be checked.

### Fulfillment

The fulfillment describes the process of handling fulfillment orders at a location. The process steps depend on the fulfillment method chosen on the fulfillment order. For example: there will be no packing and shipping on BOPIS or ROPIS orders. The steps of a Ship-From-Store / direct ship fulfillment are:

- Accepting the fulfillment order at the location (otherwise after a certain time span it will be rerouted)
- Processing the fulfillment order
  - Picking the items
  - Packing the items (can be multiple packages)
  - Creating shipments by using for example our build-in shipping providers
- Shipping the fulfillment orders

Handling this process can be done by using the Shopgate Admin or In-StoreApp. By performing status transitions on the fulfillment order like 'packed' to 'shipped' the system can trigger events, which can both trigger a notification to the customer or a webhook call to your platform. 

The Fulfillment can be processed on various types of locations like warehouses, dropshippers or stores. While warehouses and dropshippers may not use Shopgates In-StoreApp or the Admin, fulfillment- and sales order updates can still be passed via our API to enable either webhook calls or customer notifications.

### Different Ways Of Implementing Ship-From-Store

There are three major ways of integrating Ship-From-Store:

- Complete Shopgate Integration: 
  - Shopgate get the orders, 
  - Shopgate does the routing and
  - Shopgate manages the fulfillment
  - all order status updates will get synched back to merchant
- Shopgate Routing Integration:
  - Shopgate gets the orders,
  - Shopgate does the routing and
  - Fulfillment gets done by the merchant
  - Status updates have to be synched from both sides
- Shopgate Fulfillment Integration:
  - Shopgate gets the fulfillment orders per location and handles the fulfillment
  - Order status has to be synched back to the merchant system

## Entities Needed For The Ship-From-Store Implementation

The following section shows the major entities you need to know for getting started with Ship-From-Store.

### Sales Orders / Fulfillment Orders

Sales Orders are the commitment to purchase certain items by a customer. Fulfillment Orders are a subset of the Sales Order, bound to a location and a request to the shop worker to fulfill a part of the Sales Order. To learn more about these entities see the [order integration guide](../order/overview.md#concepts-and-entities).

### Locations

Locations are physical / abstract entities where a fulfillment can happen like:

- **stores**, where a customer also can walk in and purchase items,
- **warehouses**, that send out packages of items directly to the customers
- **dropshippers**, that hold stock for you and do their own fulfillment.

To all these location types can be routed.

### Products

A Product a representation of a purchasable item with a price. To learn more about this entity see [Catalog Integration Guide: Product](../catalog/concepts-and-entities.md#product).

Line items of an order object are products of a certain quantity. 

### Inventory

Inventory describe the available stock of a product. The Shopgate Platform holds information of inventory on three different abstraction levels:

- the most granular regular inventory,
- location inventory and,
- cumulated inventory.

**Inventory, Reservations and General Reservations**

A Inventory record is defined as an on-hand quantity of a product with as special sku in a single location in a single bin location in a single bin. It is possible to define a safety stock quantity for this record. Obviously as there is the concept of sku, bins/bin locations so there can be multiple inventory records per location each with a unique sku / bin /bin location combination. With this kind of granularity you can keep track where your products are if you need to. Of course it is also possible to not use sku / bin / bin location granularity.

<!-- TODO: show and explain inventory model -->

In addition to the inventory, we also have the concept of reservations. Reservations can be done on distinct inventory rows. These reservations are attached to Sales Orders and Fulfillment Orders. When requesting inventory, the reserved amount will decrease the shown inventory amount. Removing the reservation because of order cancellation will increase the inventory amount.

<!-- TODO: show and explain inventory reservation model -->

**Product Location Inventory**

The Product Location Inventory is the inventory of a product per location. It will be computed by summing up all inventories and reservations of a product in one location. Every interaction with regular inventory or reservations will update the Product Location Inventory.

<!-- TODO: link to the get endpoint -->

**Cumulated Inventory and Cumulated Inventory Reservations**

<!-- TODO: link to the inventory modes -->

Cumulated Inventory is the overall inventory / availability of a product over all locations that have directShip / Ship-From-Store available. Based on location settings like the inventory modes 'blind', 'blindWithAssortment', or 'integrated', we can tell the exact inventory counts or whether inventory is available or not. Also cumulated inventory can be reserved via Cumulated Inventory Reservations.

Our exact inventory reservations on the location and sku, bin and binLocation will be done during routing and rerouting. For the complete process of the order flow - from creation to completion - we need the reservation on the Cumulated Inventory because the routing and rerouting is a processes that can happen on different points in time. The Cumulated Inventory Reservations aim to hold the Cumulated Inventory counts accurate over the time of the order process.

### Routes

See [Routing](#routing)

## Usage of Cumulated Inventory and Ship-From-Store

The Cumulated Inventory can be used on the merchants storefront view (Magento, Shopify, self build, ...) to show the stock or the availability of a product over all location that have Ship-From-Store available. As a merchant you can get the Cumulated Inventory counts for a product via the Shopgate API.

Also some ECP may have their own cumulated inventory. This should be work as well out of the box with our system in case the inventory is properly synched.

<!-- TODO: Cumulated Inventory in a generic Storefront -->

## How To Get Basic Data Into The Shopgate Platform

<!-- 
TODO: Move this to "Import new orders to Shopgate & how to handle order update in ECP/OMS"
Sales Orders

API Order
Our Storefront / Storefront API / Storefront Library
-->

### Locations

Locations are typically set up once in the [Shopgate Admin](https://next.admin.shopgate.com). Within the admin you can manage your locations like set them active or inactive, enable fulfillment methods like ROPIS or direct ship and so on.

### Products

Products can be imported in multiple ways:

- The [Shopgate Catalog API](/docs/retail-red/YXBpOjM1NjU0NzYz-catalog)
- The simplified [FTP CSV Product Import](../catalog/csv-import.md#product-csv)
- The regular [JSON Import](../catalog/bulk-file-import.md)

### Inventories

Like products, inventory can be imported in multiple ways as well:

- The [Shopgate Catalog API](/docs/retail-red/YXBpOjM1NjU0NzYz-catalog)
- The simplified [FTP CSV Inventory Import](../catalog/csv-import.md#inventory-csv)
- The regular [JSON Import](../catalog/bulk-file-import.md)

> **Note**: Inventory may need to imported more frequently than other entities in order to keep everything in sync. Please contact our Support to get a suggestion of the rate for your specific case.

## Syncing Between Your Platform And Shopgate

<!-- Depends on integration / how deep -->
<!-- Scenario -->

## Configurations

### Locations

In order to be able to route to a location you need to enable the `Location Settings` >> `Fulfillment Settings` >> `Direct Ship` on a location (`Settings` >> `Locations`). When this is set, the location is now in a pool of locations that can be selected for routing.

### Routes

You need to create at least one route to make the routing work. To create one navigate to `Settings` >> `Ship-From-Store`. Here you can make some general settings like:
- the time to accept a fulfillment order on a location, before it gets rerouted
- the routing strategy
- the count reroute attempts and more ...

To Add a new route, press the **Add new Route** - button. The easiest route would be a something like, route everything to all orders from  country A to all stores in country A. Of course you can also do more complex routing with conditions like: if the Product has Property A OR Property B AND NOT Property C, route it to locations in State Y.

Shopgate provides a easy to use but powerful rule builder to configure such complex routings.

Multiple routes can apply for an order. Therefore Routes get a priority when they get tested against the order. So if the first one does not fi and not order splitting is possible, the next route will be used.

If no route applied, a Catch-All route can be configured, that for example routes the orders to a default warehouse.

In case there is no catch-all route configured, Orders will be marked as action-required and can be routed manually.
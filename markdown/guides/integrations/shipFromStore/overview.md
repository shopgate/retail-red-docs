<!-- 
 
[ ] Ship-From-Store concept
[x]     Routing & Fulfillment (can be used separately)
[ ]     Flows (big overview diagram, where later we will dig into detail)
[x]     Different locations that can fulfill
[x]         Warehouses (no fulfillment via app probably)
[x]         Stores
[x]         Dropshippers (no fulfillment via app probably)
[ ]     Get Inventory & product data into shopgate
[ ]     Configure Locations & Routes (see support portal)
[ ]     Use cumulated inventory in ECP
[ ]         Either do themselves, or get aggregated from shopgate
[ ]     Import new orders to shopgate & how to handle order update in ECP/OMS
[ ]         Import new orders from ECP / OMS to order API
[ ]         What to do if an order is edited in ECP/OMS?
[ ]             Shipping address changed? 
[ ]             Line items added/removed?
[ ]             Status updates (FO Status/SO Status?)
[ ]     Sync order updates / routing results back (e.g. when order is shipped)
[ ]         Fulfillment done via retail.red or only routing?
[ ]         Both ways:
[ ]             FO status changed in shopgate (e.g. store associate marked order as “shipped”,
[ ]             FO status changed in external system (e.g. warehouse management system of dropshipper marked order as shipped)
[ ]     Special flows
[ ]         Editing order (in shopgate / in external system)
[ ]         Canceling order (in shopgate / in external system)
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
  - [How to get data into the Shopgate platform](#how-to-get-data-into-the-shopgate-platform)
  - [Syncing between your Platform and Shopgate](#syncing-between-your-platform-and-shopgate)
  - [Using cumulated inventory](#using-cumulated-inventory)
    - [The three levels of inventory](#the-three-levels-of-inventory)

## About this Guide

The following guide should show you what Ship-From-Store is, how it works, and what you need to set it up within the Shopgate Platform integrated with your system.

## Key Features

The key features of Ship-From-Store are Order Routing and the Fulfillment Process. In the upcoming text they will be referenced in their short forms as routing and fulfillment.

Utilizing these features you are able automatically route/split orders to different locations using a customizable ruleset as well as fulfilling the orders on site from picking to shipping.

### Routing

The Order Routing describes the creation of fulfillment orders<!-- TODO link FO --> at locations that fit certain criteria like: closest to the customer, or location that have the most stock or other rule sets that you can freely configure within the Shopgate system. Not only that - the routing engine is able to split one sales order<!-- TODO link SO --> into multiple fulfillment orders it sees as best fit. Of course this is also configurable. In case a fulfillment order can't get handled on it's assigned location, the Order Routing can reroute the fulfillment order to another location that fits.

<!-- TODO order splitting by fulfillment method or next best fit when it can't get routed to a single location -->

<!-- TODO: explain Routes -->

### Fulfillment

The fulfillment describes the process of handling fulfillment orders at a location. The process steps depend on the fulfillment method chosen on the fulfillment order. For example: there will be no packing and shipping on BOPIS or ROPIS orders. The steps of a Ship-From-Store / direct ship fulfillment are:

- accepting the fulfillment order at the location (otherwise after a certain timespan it will be rerouted)
- processing the fulfillment order
  - picking the items
  - packing the items (can be multiple packages)
  - creating shipments by using for example our build-in shipping providers
- shipping the fulfillment orders

Handling this process can be done by using the Shopgate Admin or In-StoreApp. By performing status transitions on the fulfillment order like 'packed' -> 'shipped' the system can trigger events, which can both trigger a notification to the customer or a webhook call to your platform. 

The Fulfillment can be processed on various types of locations like warehouses, dropshippers or stores. While warehouses and dropshippers may not use Shopgates In-StoreApp or the Admin, fulfillment- and sales order updates can still be passed via our API to enable either webhook calls or customer notifications.

### Different Ways Of Implementing Ship-From-Store

<!-- Complete Implementation: We get the orders, we do the routing, we manage the fulfillment -->
<!-- Routing Implementation: We get the orders, we do the routing, you manage the fulfillment -->
<!-- Fulfillment Implementation: We get the Fulfillment Orders on the Location and manage the fulfillment without routing -->

## Entities Needed For The Ship-From-Store Implementation

Sales Orders
Locations
Products
Inventories / LocationInventory / Cumulated Inventory
Reservations / Cumulated Inventory Reservations
Routes

## How to get data into the Shopgate platform

<!-- Max example -->
Sales Orders
Locations
Products
Inventories
Configured Routes

## Syncing between your Platform and Shopgate

<!-- Depends on integration / how deep -->
<!-- Scenario -->

## Using cumulated inventory

### The three levels of inventory

**Inventory, Reservations and General Reservations**

A Inventory record is defined as a on-hand quantity of a product with as special sku in a single location in a single bin location in a single bin. It is possible to define a safety stock quantity for this record. Obviously as there is the concept of sku, bins/bin locations so there can be multiple inventory records per location each with a unique sku / bin /bin location combination. With this kind of granularity you can keep track where your products are if you need to. Of course it is also possible to not use sku / bin / bin location granularity.

<!-- TODO: show and explain inventory model -->

In addition to the inventory, we also have the concept of reservations. Reservations can be done on distinct inventory rows. These reservations are attached to Sales Orders and Fulfillment Orders. When requesting inventory, the reserved amount will decrease the shown inventory amount.

<!-- TODO: show and explain inventory reservation model -->

**Product Location Inventory**

**Cumulated Inventory and Cumulated Inventory Reservations**

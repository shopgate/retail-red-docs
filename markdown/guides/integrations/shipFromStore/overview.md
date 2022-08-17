<!-- 
Ship from store concept
    Routing & Fulfillment (can be used separately)
    Flows (big overview diagram, where later we will dig into detail)
    Different locations that can fulfill
        Warehouses (no fulfillment via app probably)
        Stores
        Dropshippers (no fulfillment via app probably)
    Get Inventory & product data into shopgate
    Configure Locations & Routes (see support portal)
    Use aggregated inventory in ECP
        Either do themselves, or get aggregated from shopgate
    Import new orders to shopgate & how to handle order update in ECP/OMS
        Import new orders from ECP / OMS to order API
        What to do if an order is edited in ECP/OMS?
            Shipping address changed? 
            Line items added/removed?
            Status updates (FO Status/SO Status?)
    Sync order updates / routing results back (e.g. when order is shipped)
        Fulfillment done via retail.red or only routing?
        Both ways:
            FO status changed in shopgate (e.g. store associate marked order as “shipped”,
            FO status changed in external system (e.g. warehouse management system of dropshipper marked order as shipped)
    Special flows
        Editing order (in shopgate / in external system)
        Canceling order (in shopgate / in external system)
        Backorders / Preorders
-->

# Overview

## Content

1. [About this Guide](#about-this-guide)
2. [Key Features](#key-features)

## About this Guide

The following guide should show you what Ship-From-Store is, how it works, and what you need to set it up within the Shopgate Platform integrated with your system.

## Key Features

The key features of Ship-From-Store are Order Routing and the Fulfillment Process. In the upcoming text they will be referenced in their short forms as routing and fulfillment.

<!-- TODO benefits ? -->

### Routing

The routing describes the creation of fulfillment orders<!-- TODO link FO --> at locations that fit certain criteria like, closest to the customer, or location that have the most stock or other rule sets that you can freely configure within the Shopgate system. Not only that - the routing engine is able to split one sales order<!-- TODO link SO --> into multiple fulfillment orders it sees best fit. Of course this is also configurable. In case a fulfillment order can't get handled on it's assigned location, the routing engine can reroute the fulfillment order to another location that fits best.

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

Within this process it is possible to make adjustments to both sales order and fulfillment order. To handle the process you can either use our In-StoreApp, the Shopgate Admin <!-- TODO: link admin --> or implement the transitions in your system via the Shopgate API <!-- TODO link api docs order svc -->.

Fulfillments can be processed on various types of locations like warehouses, dropshippers and stores.

## How to get data into the Shopgate platform

<!-- Max example -->
Locations
Products
Inventories // Cumulated Inventory
Sales Orders
Fulfillment Orders
Configured Routes

<!-- Min example -->
Sales Orders
Fulfillment Orders

## Syncing between your Platform and Shopgate

<!-- Depends on integration / how deep -->
<!-- Scenario -->
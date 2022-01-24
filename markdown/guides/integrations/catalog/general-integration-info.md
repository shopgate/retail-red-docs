# Integration Overview

## About this guide

Goal of this guide is to give you an overview of the catalog structure for retail.red, the different options how catalog data can be imported from an external system into retail.red and how it is kept up-to-date.

## Overview

The catalog data consists of multiple entities:

- Products
- Product Attributes (e.g. sizes, colors)
- Categories
- Inventory (for direct ship fulfillment as well as retail store availability)
- Catalogs (only relevant in case of multi-store / multi-catalog setups)

For the detailed specification of each entity, please check the [retail.red Catalog API](/docs/retail-red/YXBpOjM1Nzg0Nzgz-catalog).

The goal of the catalog integration is to transfer the catalog data to retail.red and keep it up to date. There are different options how catalog information can be transferred:

- [Bulk file imports in CSV format](./csv-import.md) via FTP or manual upload in [retail.red Admin](https://next.admin.shopgate.com/)
- [Bulk file imports in JSON format](./bulk-file-import.md) via [retail.red Import API](/docs/retail-red/YXBpOjM1NjU0NzY0-import)
- [Near-real-time updates](./near-realtime-import.md) in JSON format via the [retail.red Catalog API](/docs/retail-red/YXBpOjM1NjU0NzYz-catalog)

## Comparison

| CSV Import                                 | JSON Import                                        | Catalog API Integration                          |
| ------------------------------------------ | -------------------------------------------------- | ------------------------------------------------ |
| Fast to implement / Setup for a quickstart | Fairly easy to implement but more complex then CSV | Close to realtime updates / synched with the ecp |
| Easy to use (drag-and-drop or ftp upload)  | Supports partial and full imports                  | Highest effort to implement                      |

The CSV Import is the best way to get started. For the best integration experience, we recommend using the JSON Import and additionally if required the Catalog API Integration.

<!--
Start with bulk imports in JSON format, for the initial import and recurring updates. Depending on the number of records per file, bulk imports can take a few minutes to be applied. Bulk imports support full imports as well as partial imports (only importing products that have changed).
If near-real-time updates are required, you can update specific products directly via the retail.red API, and changes will be applied in a matter of seconds.
-->

> **NOTE:** You can always switch between all the options or even mix them without the need of resetting your data.

## Arrangement of Entities

For the import of catalog entities it is important to know some special relationships between entites. These relationships have implications on how (in which order) the entities need to be imported.

General rule of thumb here is:

- Attributes, Inventory and, Catalogs come first
- After Catalogs have been imported the Categories can be imported
- Products last, as they depend on Attributes, Inventory and Categories

> **For CSV Import:** The CSV Import lets you upload categories, products and inventory. Attributes get created implicitly via the product entries. Everything gets imported into a default catalog. The category CSV file should get imported before the product CSV file.

> **For JSON Import:** If all entities get uploaded into the same import via the JSON [Import Service](/docs/retail-red/YXBpOjM1NjU0NzY0-import), the process will automatically take care of the correct logical order.

If you want to learn more about the entity relationships see [Concepts and Entities](./concepts-and-entities.md#entity-relationships).

## Import Strategies

The retail.red platform supports two import strategies for CSV and JSON imports.

### Full Import

The uploaded file should contain all records for this entity (e.g. all products). New records are automatically created, existing records updated and records that exist at retail.red, but are missing in the uploaded file, will be removed from retail.red.

> **NOTE:** One special case is the full import for Inventory. Before uploading the file, the locations this file applies to can be specified. This way, inventory records of other locations remain untouched. This can be helpful when different locations or location groups use different ways to manage their inventory.

### Partial Import

New records from the file will be created and existing records updated, but if records are missing from the file they will not be deleted from retail.red. To delete records via a partial import, an explicit entry has to be added to the file with the field importType and value “D”.
Example to delete the product 123:

- `JSON`: {“productCode”:”123”, “importType”:”D”}
- `CSV`: Set column "ImportType" to "D"

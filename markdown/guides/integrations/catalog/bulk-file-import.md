# Bulk imports in JSON format

## Introduction

For bulk updates, all entities that should get updated are collected in one json file per entity type (e.g. one file for products, one for categories etc.). This file is then uploaded using the [retail.red Import Service](/docs/retail-red/YXBpOjM1NjU0NzY0-import) and the Import is triggered.

You can also upload multiple files within one bulk import, e.g. one file for products, one for product attributes and one for categories. The Bulk Import Service will take care of the relationships between the different entities and make sure that for example all product attributes have finished importing before starting the product import.

For each file that is uploaded you can specify if this file should be handled as a full import or partial import via the importStrategy parameter.

## File Format

The to be uploaded JSON file has to contain an array consisting of POST models of the entity you want to import. See [Products example](/docs/retail-red/c2NoOjM1Nzg0ODU2-product-create) for post model.

For partial imports: In addition to the model, you can add `importType` as an model top level property in order to specify if you want to delete or add/change the entity - see [Partial Import](./general-integration-info.md#partial-import).

[Example products upload file](https://data.shopgate.com/example_files/products_example.json)

## Process

The flow to do a bulk import via the Bulk Import Service API is the following:

1. Call [POST /merchants/{merchantCode}/imports](/docs/retail-red/b3A6MzU2NTUwODE-create-import) to create a new import. Take note of the importRef from the response for the next calls.
2. Call [POST /merchants/{merchantCode}/imports/{importRef}/urls](/docs/retail-red/b3A6MzU2NTUwODU-get-signed-url) to generate a new upload URL for the desired entity type. Use the `importStrategy` parameter to define if this should be a [full import or partial import](./general-integration-info#import-strategies) (default is full import).
3. Upload the json file to this URL (PUT Method; Body: Binary)
4. Optional: create more URLS for other entity types and upload other files
5. Call [POST /merchants/{merchantCode}/imports/{importRef}](/docs/retail-red/b3A6MzU2NTUwODI-trigger-import) to trigger the import
6. Optional: Check the status of the import via [GET /merchants/{merchantCode}/imports](/docs/retail-red/b3A6MzU2NTUwODA-get-imports) and pass a filter for the current importRef

You can also see a list of the bulk imports in the Shogate Admin. Log in to your admin and go to Settings → Imports to see a list of current and previous imports.

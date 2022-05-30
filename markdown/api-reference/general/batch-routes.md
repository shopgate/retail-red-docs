# Batch Routes

As a convention the Shopgate API uses batch routes for creating entities. The purpose of these routes is to reduce API calls as it is possible to create multiple entities at once.

To be able to do this there is a common structure for both request and response bodies.

Also the error handling with these routes work a little different as the single entity HTTP route. If there is no general hard error like a the correspondent merchant to a call was not found (404) or the permission was not granted (403) the endpoint will always return a 20x. Even if no entity of the incoming list has been processed properly. See [Response](#response) on how to check the errors in this case.

## Request

The incomming request body is always an object containing a collection property like this:

```json
{
  "products": [
    { "code": "product1", ... },
    { "code": "product2", ... }
  ]
}
```

## Response

In case no 4xx/5xx HTTP error occured the response will generally look like this:

```json
{
  "errors": [
    {
      "code": "400",
      "entity": "product",
      "entityId": "product1",
      "entityIndex": 0,
      "reason": "category does not exist",
      "subentityPath": ["categories", 0]
    }
  ]
}
```

The example shows the default error. In some cases (where it is necessay) there may be additional properties describing the issues further.

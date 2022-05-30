# Service URLs

## Service Subdomains

Every Shopgate service is reachable under its own subdomain, using the following format: `https://{serviceName}.shopgate.io`.

**Examples**

- `https://location.shopgate.io`
- `https://order.shopgate.io`

## Service Paths

All service paths follow the REST pattern and are scoped by entities.
All entities are scoped by merchant, and therefore start with the merchant entity:

```
/v${version}/merchants/{merchantCode}/${subentityName}/[${subentityID}]
```

_Examples:_

- `https://location.shopgate.io/v1/merchants/TEST/locations`
- `https://catalog.shopgate.io/v1/merchants/TEST/products/awesomeProductCode`

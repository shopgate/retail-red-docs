# Sorting, Filtering and Fields

## Content

1. [Sorting](#sorting)
2. [Filtering](#filtering)
3. [Fields](#fields)

## Sorting

In case the endpoint has a sortable result set it supports the `sort` query parameter. The value of this parameter needs to be a comma separated list of fields.
One filter consists out of the field name and the sort order:

`{field name}:{ASC|DESC}`

Example:

`?sort=price:asc,name:desc`

The sort order parameter is required and has to be set on every request.

## Filter

All retail.red APIs share the same filter syntax. It is a JSON-based syntax, which is passed via the query parameter `filters`.

### Available Filters

- `$eq` - equals
- `$ne` - not equals
- `$gt` - greater than
- `$gte` - greater than or equals
- `$lt` - lower than
- `$lte` - lower than or equals
- `$ct` - contains substring
- `$nct` - not contains substring
- `$exists` - exists
- `$nExists` - not exists
- `$in` - in a list of values
- `$nin` - not in a list of values

### Available Logical Operators

- `$and` - Implicitly, all filters of a list are joined with an "and" operation, unless declared otherwise.
- `$or` - All filters of a list within the `$or` property of a filter are joined with an "or" operation.

Example:

```json
{
 "locationCode": "DERetail001",
 "$or": [
   { "orderNumber": { "$ct": "10074" } },
   { "name": { "$ct": "john" } },
   {
     "$and": [
       { "contact.id": { "$exists":  true } },
       { "contact.defaultBillingAddress": { "$eq": true } }
     ]
   }
 ]
}
```

## Fields

Some endpoints support the fields query parameter. With this paramter you can specify additional fields the endpoint supports.

The format needs to be a comma separated list.

Example:

`?fields=shortDescription,longDescription`

If no list is passed via query parameter, a default set of fields will be shown.

> **NOTE**: The catalog getProduct(s) endpoints work a  different. If the a list via the fields parameter is specified, only these fields will be returned. Its exclusive instead of additive in this case.
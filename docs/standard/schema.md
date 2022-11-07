# Schema

`network-schema.json` is the single source of truth for defining OFDS's objects and fields. It is documented using an extended version of [JSON Schema 2020-12](https://json-schema.org/specification-links.html#2020-12).

The source for `network-schema.json` and other schema files is in the [`schema` directory](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/tree/0.1-dev/schema) of the standard repository.

## JSON Schema usage

The tables below list the validation keywords, types and formats used in the schema. For each keyword, type and format, the 'Test data' column contains a JSON pointer to a field in [`network-package-invalid.json`]() that fails validation.

### Keywords

| Keyword | Test Data | 
| ------- | --------- |
| [`prefixItems`](https://json-schema.org/understanding-json-schema/reference/array.html#tuple-validation) |`/networks/0/links/0/` | 
| [`const`](https://json-schema.org/understanding-json-schema/reference/generic.html#constant-values) | `/networks/0/crs/name` | 
| [`minItems`](https://json-schema.org/understanding-json-schema/reference/array.html#length) | `/networks/0/nodes` | 
| [`uniqueItems`](https://json-schema.org/understanding-json-schema/reference/array.html#uniqueness) | `/networks/0/phases` | 
| [`pattern` (`propertyNames`)](https://json-schema.org/understanding-json-schema/reference/object.html#property-names) | `/networks/0/spans/0/route` | 
| [`pattern` (`string`)](https://json-schema.org/understanding-json-schema/reference/string.html#regular-expressions) | `/networks/0/links/1/rel` | 
| [`minLength`](https://json-schema.org/understanding-json-schema/reference/string.html#length) | `/networks/0/name` | 
| [`enum`](https://json-schema.org/understanding-json-schema/reference/generic.html#enumerated-values) | `/networks/0/spans/0/status` | 
| [`required`](https://json-schema.org/understanding-json-schema/reference/object.html#required-properties) | `/networks/0/spans/0/id` | 
| [`minProperties`](https://json-schema.org/understanding-json-schema/reference/object.html#size) | `/networks/0/organisations/0/` | 

### Types

Types are validated using the [`type`](https://json-schema.org/understanding-json-schema/reference/type.html) keyword.

| Type | Test Data | 
| ---- | --------- |
| [`boolean`](https://json-schema.org/understanding-json-schema/reference/boolean.html) | `/networks/0/spans/0/directed` | 
| [`integer`](https://json-schema.org/understanding-json-schema/reference/numeric.html#integer) | `/networks/0/spans/0/fibreCount` | 
| [`number`](https://json-schema.org/understanding-json-schema/reference/numeric.html#number) | `/networks/0/accuracy` | 
| [`string`](https://json-schema.org/understanding-json-schema/reference/string.html) | `/networks/0/accuracyDetails` | 
| [`object`](https://json-schema.org/understanding-json-schema/reference/object.html) | `/networks/0/publisher` | 
| [`array`](https://json-schema.org/understanding-json-schema/reference/array.html) | `/networks/0/contracts` | 

### Formats

Formats are validated using the [`format`](https://json-schema.org/understanding-json-schema/reference/string.html#format) keyword.

| Format | Test Data | 
| ------ | --------- |
| [`date`](https://json-schema.org/understanding-json-schema/reference/string.html#dates-and-times) | `/networks/0/publicationDate` | 
| [`iri`](https://json-schema.org/understanding-json-schema/reference/string.html#resource-identifiers) | `/networks/0/website` | 
| [`uri`](https://json-schema.org/understanding-json-schema/reference/string.html#resource-identifiers) | `/networks/0/crs/uri` | 
| [`uuid`](https://json-schema.org/understanding-json-schema/reference/string.html#resource-identifiers) | `/networks/0/id` | 
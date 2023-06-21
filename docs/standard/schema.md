# Schema

`network-schema.json` is the single source of truth for defining OFDS's objects and fields. It is documented using an extended version of [JSON Schema 2020-12](https://json-schema.org/specification-links.html#2020-12).

The source for `network-schema.json` and other schema files is in the [`schema` directory](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/tree/0.1-dev/schema) of the standard repository.

## JSON Schema usage

The tables below list the validation keywords, types and formats used in the schema. For each keyword, type and format, the 'Test data' column contains a JSON pointer to a field in [`network-package-invalid.json`](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/examples/json/network-package-invalid.json) that fails validation.

### Keywords

| Keyword | Test Data | 
| ------- | --------- |
| [`prefixItems`](https://json-schema.org/understanding-json-schema/reference/array.html#tuple-validation) |`/networks/0/links/0/` | 
| [`const`](https://json-schema.org/understanding-json-schema/reference/generic.html#constant-values) | `/networks/0/crs/name` | 
| [`minItems`](https://json-schema.org/understanding-json-schema/reference/array.html#length) | `/networks/0/nodes` | 
| [`uniqueItems`](https://json-schema.org/understanding-json-schema/reference/array.html#uniqueness) | `/networks/0/phases` | 
| [`additionalProperties](https://json-schema.org/understanding-json-schema/reference/object.html#additional-properties) | `/networks/0/spans/0/route` | 
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

## Other normative rules

The table below lists normative rules specified in field descriptions. For each rule, the 'Test data' column contains JSON pointers to the fields in [`network-package-additional-checks.json`](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/examples/json/network-package-additional-checks.json) that does not conform to the rule.

| Rule | Specified in | Test data |
| -- | -- | -- |
| Node reference is resolvable | [`Span.start`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Span,start), [`Span.end`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Span,end) | `networks/0/spans/0/start`, `networks/0/spans/0/end` |
| Node location type is 'Point' | [`Geometry.type`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Geometry,type) | `networks/0/nodes/0/location/type` |
| Node location coordinates format is a single position | [`Geometry.coordinates`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Geometry,coordinates) | `networks/0/nodes/0/location/coordinates` |
| Span route geometry is 'LineString' | [`Geometry.type`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Geometry,type) | `networks/0/spans/0/route/type` |
| Span route coordinates is an array of positions | [`Geometry.coordinates`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Geometry,coordinates) | `networks/0/spans/0/route/coordinates` |
| Phase reference is resolvable | [`PhaseReference.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/PhaseReference,id) | `networks/0/nodes/0/phase/id`, `networks/0/spans/0/phase/id`, `networks/0/contracts/0/relatedPhases/0/id` |
| Phase name is consistent | [`PhaseReference.name`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/PhaseReference,name) | `networks/0/contracts/0/relatedPhases/1/name` |
| Organisation reference is resolvable | [`OrganisationReference.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/OrganisationReference,id) | `networks/0/spans/0/physicalInfrastructureProvider/id`, `networks/0/spans/0/networkProvider/id`, `networks/0/spans/0/supplier/id`, `networks/0/nodes/0/physicalInfrastructureProvider`, `networks/0/nodes/0/networkProvider`, `networks/0/phases/0/funders/0/id` |
| Organisation name is consistent | [`OrganisationReference.name`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/OrganisationReference,name) | `networks/0/phases/0/funders/1/name` |
| Node international connections country is present | [`Node.internationalConnections`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Node,internationalConnections) | `networks/0/nodes/0/internationalConnections/0/` |
| Identifier is unique | [`Node.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Node,id), [`Span.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Span,id), [`Phase.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Phase,id), [`Organisation.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Organisation,id), [`Contract.id`](https://open-fibre-data-standard.readthedocs.io/en/0.1-dev/reference/schema.html#network-schema.json,/definitions/Contract,id) | `networks/0/spans/1/id`, `networks/0/nodes/1/id`, `networks/0/phases/1/id`, `networks/0/contracts/1/id`, `networks/0/organisations/1/id` |

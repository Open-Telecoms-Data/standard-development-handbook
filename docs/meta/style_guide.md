# Common conventions

This style guide covers our conventions when writing:

* OFDS schema and standard documentation
* GitHub issues
* Resources and guidance for publishers and users
* Training materials
* Associated presentations and documents

## Readability

We strive to write concise, clear documentation. To improve your writing, please learn how to [write for the web](https://www.usa.gov/style-guide/writing-for-web) (with many more resources at [Nielsen Norman Group](https://www.nngroup.com/topic/writing-web/), and consider using these tools:

* [Hemingway Editor](http://www.hemingwayapp.com/)
* [Grammarly](https://www.grammarly.com/)
* Readability statistics in Microsoft Word or [online tools](https://www.webfx.com/tools/read-able/flesch-kincaid.html)

## Spelling

Use British English (e.g. "organisation" rather than "organization") unless aligning a field name with an existing standard that uses alternative spellings. Notably, this means being careful about using "s" instead of "z" in many words of Latin origin.

## Text formatting

* Do not use constructions like "node(s)" or "the node (or nodes)". The plural is fine, like "nodes".
* Do not use smart quotation marks like `“ ” ‘ ’`. Use simple quotation marks like `" '`.
* When referring to a **field** or **codelist**, use the camelCase version of the field/codelist name, and enclose it in backticks so it is displayed in a montotype font as follows: `camelCase`
* When referring to a **building block**, use the capitalized CamelCase version of the building block name, and enclose it in backticks so it is displayed in a montotype font as follows: `CamelCase`
* When referring to a **code** from a codelist, enclose the value in single quotes, e.g. "We have added an 'aerial' code to the `deploymentType` codelist". Note that `true` and `false` are not codes; they are boolean values.
* When pluralizing a **field** or **building block**, treat the field/building block name as a proper noun, and add a `'s` instead of an `s` to the end, or treat it as a mass noun and add nothing to the end
* When referring to a field in JSON Schema, use dot notation, like `tender.id`. (Slash notation is reserved for [JSON Pointer](https://tools.ietf.org/html/rfc6901). For example, the JSON Pointer for `tender.id` is `/definitions/Tender/properties/id`.)
* When referring to a field in OFDS data, use a JSON Pointer, like `/nodes/id`.

## Word choice

* "codelist" not "code-list" or "code list"
* "changelog" not "change-log" or "change log"
* "metadata" not "meta-data" or "meta data"
* "hyphen" not "dash", to describe the "-" character

When describing data:

* "publication" for the data source that persists across time
* "collection" for the publication's data at a specific point in time
* "JSON data" not "JSON document", to avoid confusion with the `documents` field

When describing JSON Schema:

* "field" to refer to OFDS fields, like `links.deployment`
* "property" to refer to JSON Schema metadata properties, like `enum`
* "array" not "list"
* "object" not "block"

When referring to a field, prefer the notation for the path in the data, like `links.transmissionMedium`, rather than the notation for the path in the schema, like `Link.transmissionMedium`.

These regular expressions can be used to find breaches of the style guide, accounting for false positives.

property
:  ```(?<!(`minLength| `required|geStrategy)` )propert(y|ies)```

data path notation
:  `\b[A-Z][a-zA-Z]+\.(?!(aspx|db|html|md|org|xml|zip)\b)[a-zA-Z]{2,}`

## JSON example filenames

* Name the JSON example with a descriptive, lower-case filename, with underscores between words.
* Store the example in the `docs/examples` directory in the standard's repository. Create a sub-directory to group related examples, if one doesn't exists already, rather than using a common prefix to the filename.
* If you need to make a file downloadable, don't place it in `docs/_static/`, instead use the download role, e.g.:

```
   {download} [link text](../../examples/file)
```

Images

* Create the image, preferably using easily accessible collaborative tools like [Google Drawings](https://docs.google.com/drawings/).
* Store the editable version in the [this Google Drive folder](https://drive.google.com/drive/folders/1BKh5_fV2diqLNlHOgSuxzeSKMyL0SvDZ).
* Export the image in PNG format.
* Use a descriptive, lower-case filename, with underscores between words.
* Store the exported version in the `docs/_static/png` directory in the standard's repository. Create a sub-directory to group related images, as needed, rather than using a common prefix to the filename.

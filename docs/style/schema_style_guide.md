# Schema style guide

## Guidelines

* Avoid multiple representations of the same fact where possible. A user should, ideally, have a single way of answering a given question. Having multiple representations of the same fact also introduces the possibility of inconsistent values. Some exceptions:
  * The fact is external to the contracting process but serves a common need. For example, an organisation's ID from a company register is sufficient to identify it. However, it is a common need to get organisation names. As such, we include a field for an organisations name, so that users can avoid the extra step of performing a lookup against the corporate register.
  * The fact is calculated from details that are unpublished.
* Avoid adding fields for facts that can be calculated from other fields. This introduces the possibility of inconsistent values. Some exceptions:
  * The fact can't be calculated in all cases. For example, because the fields needed to calculate it may not be published until a later date.
  * The fact is calculated from details that are unpublished.

## Normative statements

* Normative statements should be constructed using the keywords defined in [RFC2119](https://tools.ietf.org/html/rfc2119).
* Non-normative statements should not use the keywords defined in RFC2119, see this [Internet-Draft](https://tools.ietf.org/html/draft-hansen-nonkeywords-non2119-04) for appropriate synonyms.
* Normative statements should not use constructions such as "should always", "should only" or "where possible … must". The appropriate normative keyword should be used instead, e.g. "must" in place of "should always".
* Normative statements must be consistent with the schema, e.g. if a field `x` is a required field in the schema, then:
  * "`x` must be provided" is consistent.
  * "`x` should be provided" is inconsistent.

## Schema structure

### Definitions

The top level of the schema is split between `properties` and `definitions`. The latter contains objects that may be re-used, by reference, in multiple locations across the schema. Each of these can be thought of as a "Class", and its name is capitalized accordingly. Whenever you consider that an object or structure might be re-used in a different area of the standard, it should be included in `definitions`.

### ID field

Objects can include an `id` field to support cross-referencing, or to disclose the object's real-world identifier.

If an object is used as an item in an array and has, or may have in the future, a child property that is itself an array of objects, then the object must include an `id` field so that it can be referenced by the objects in the child array. This approach is [necessary for compatibility with Flatten Tool](https://flatten-tool.readthedocs.io/en/latest/unflatten/#relationships-using-identifiers) and useful for relational representations of the data in general.

### Details field

A codelist field can be paired with a `xDetails` string field, which can be used for:

* Free text details on the codelist value
* A more detailed set of classifications from a publisher's systems

Use of `xDetails` fields can help increase acceptance of a closed codelist.

### Entered field

A field can be paired with a `xEntered` field, which can be used when:

* The data is sometimes entered manually
* The data has known quality issues

For example:

* A data entry form might allow users to select an identifier scheme from a dropdown, or to enter its name manually if it is not in the list. The `scheme` field would be used for dropdown entries and a `schemeEntered` field for manual entries.
* The publication system can detect unreasonable amounts (e.g. missing decimal separators due to human error). The `amount` field would be used for reasonable amounts and an `amountEntered` field for unreasonable amounts.

With this pattern, users can more easily analyze values in the original field, without needing to clean, filter or deduplicate them.

That said, the semantics of the two fields are very close, with the only distinctions being the data collection method and/or the data quality. As such, **only use this pattern if the field supports important use cases**.

### Additional array

An object field can be paired with a `additionalX` array field, which can be used when:

* A data owner has one or more values for a field
* One of those values can be considered in some way 'primary'
* A number of use cases can be met by looking only at the primary value

For example, a source system might record the company registration number and VAT identifier of a company. If we had a single `parties.identifier` object, the data owner would have to pick which identifier to use, and would be omitting data that could help some users to identify an organization. If we only had an array of `parties.identifiers`, then the data structure for the simple case (only one identifier) becomes more complex, and it is not possible to indicate any priority between the identifiers.

## Validation keywords

* Date fields must use `"format": "date-time"`.
* URL fields must use `"format": "uri"`.
* Number fields should use `minimum`, `maximum` and/or `exclusiveMinimum`, if appropriate.
* The `default` keyword shouldn't be used, because consumers aren't expected to fill in defaults.
* The following keywords aren't used and might require code changes: `additionalItems`, `additionalProperties`, `dependencies`, `exclusiveMaximum`, `maxItems`, `maxLength`, `maxProperties`, `multipleOf`, `allOf`, `anyOf`, `not`.

The following keywords are added by [ocdskit schema-strict](https://ocdskit.readthedocs.io/en/latest/cli/schema.html#schema-strict):

* Array fields should set `"uniqueItems": true`.
* Required array fields must use `"minItems": 1`.
* Required object fields must use `"minProperties": 1`.
* Required string fields must use `"minLength": 1`, unless `enum`, `format` or `pattern` is used.

### Types and null

`null` should not be included in a field's types. Where the data needed to publish a field is not available, the field should be omitted in its entirety (key and value).

## Field and code names

* Check [other standards](https://lov.linkeddata.es/dataset/lov) for preferred terms.
* Use lower [camelCase](https://en.wikipedia.org/wiki/Camel_case) for field names, e.g. `equippedCapacity`.
* Use upper [CamelCase](https://en.wikipedia.org/wiki/Camel_case) for `definitions` entries, e.g. `Node`.
* Put the qualifier *before* the concept, e.g. `fibreType` rather than `typeOfFibre`.
* Don't abbreviate words, e.g. `transmissionMedium` not `txMedium`.
* Use singular for fields pointing to an object or literal value.
* Use plural for fields pointing to an array of values.
* Field names should not include their parent's name, e.g. `Link.deployment` not `Link.linkDeployment`.

.. note::
   The choice of a term is cosmetic; it's not semantic. A field's description, not its name, is semantic.

## Field and code descriptions

* The first sentence:
  * Must be distinct between fields.
  * Should be a noun phrase, not a sentence. For example, for `Node`:
    * Good: "An access (entry or exit) point […]"
    * Bad: "A node is […]"
  * Should be written in a neutral voice, rather than addressing a particular audience. For example, for `Link.endpoints`:
    * "The nodes that this link connects." is written in a neutral voice.
    * "Specify the nodes that this link connects" addresses publishers rather than users.
* Subsequent sentences may provide information or guidance to assist publishers to use the field effectively or users to interpret the field effectively. Guidance sentences should be grounded in clear user needs and implementation experience of common pitfalls or errors.
* Descriptions for similar fields or codes should be consistent with each other where possible, without discarding information relevant to a specific field.
* Descriptions of core concepts should be compared to authoratitive sources.
* For fields or codes whose names and titles use complex or specialist language, consider providing an example to aid non-expert users.

Descriptions should:

* Balance the needs of expert users, for whom the description serves to assure that use of the field or code is appropriate, and non-expert users, for whom the description of the code serves to help them understand how the field or code is used and whether it is likely to contain the information they are looking for.
* Be concise and avoid using exhaustive lists.

Descriptions should **not**:

* Link to definitions provided on external websites.
* Explicitly state whether a field is required or optional.
* Simply restate the title or name of a field or code.
* Declare the type of the field: for example, "A list", "A true/false field", etc.

The following examples can be used to inform descriptions for common types of fields in the schema. Additional information, specific to a particular field, should be provided in a separate sentence after the primary description of the field.

### Articles

Assuming the rest of the guidance is followed, it is recommended to start the description with:

* "Whether", for a boolean field.
* "Information about", for a high-level sub-schema. For example:
  * "Information about the nodes […]" for `nodes`.
  * "The address of the node. […]" for `Node.address`. `Address` is a low-level sub-schema.
* "The" with a plural noun phrase, for the description of an array of strings.
* "A" or "An", for the description of a sub-schema that is used in the context of an array.

In other cases, start with "The", though this guidance may be updated with additional cases.

### Codelists

```
   <semantics>, using the <open/closed> <name> codelist. See also the <xDetails> field.
```

**Example:**

   The transmission medium of this link, using the transmission medium codelist

### Identifiers

For the `id` field of items in arrays:

```
   A locally unique identifier for this <object_name>. The identifier must be unique within the scope of the <parent> this <object_name> belongs to.
```

**Example:**

   A locally unique identifier for this link. The identifier must be unique within the scope of the network this link belongs to.

### Names

For the `name` field of an object:

```
   A name for this <object_name>.
```

### Descriptions

For the `description` field of an object:

```
   A description of this <object_name>. Structured information should be provided in <related_fields>.
```

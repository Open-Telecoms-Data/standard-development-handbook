# Schema style guide

See [schema conventions](../../standard/conventions) for more guidance on structuring schema.

## Field names

* We use lower [camelCase](https://en.wikipedia.org/wiki/Camel_case) for field names, e.g. `awardCriteriaDetails`.
* We use upper [CamelCase](https://en.wikipedia.org/wiki/Camel_case) for `definitions` entries, e.g. `Award`.
* We put the qualifier *before* the concept, e.g. `enquiryPeriod` rather than `periodOfEnquiry`.
* We use singular for fields pointing to an object or literal value.
* We use plural for fields pointing to an array of values.
* Field names should not include their parent's name, e.g. `title` not `tenderTitle`, `description` not `awardDescription`, etc.

## Building blocks

* The `period` object should be used in place of `year` or `month` fields.
* The `identifier` object should be used when referencing identifiers from external sources, e.g. `{"id": "12345678", "scheme": "IBAN"}` rather than `"ibanID": "12345678"`.

## Validations

* Date fields should use the `"format": "date-time"` key to enforce use of ISO8601.

# Normative content style guide

```eval_rst
  .. note::
    The current version of the OCDS schema and documentation (1.1.3) does not comply with these recommendations.
```

## Statements

* Normative statements should be constructed using the keywords defined in [RFC2119](https://tools.ietf.org/html/rfc2119).
* Normative keywords should be capitalised where used, per [RFC8174](https://tools.ietf.org/html/rfc8174).
* Non-normative statements should not use the keywords defined in RFC2119, see this [Internet-Draft](https://tools.ietf.org/html/draft-hansen-nonkeywords-non2119-04) for appropriate synonyms.
* Normative statements should not use constructions such as "should always", "should only" or "where possible ... must". The appropriate normative keyword should be used instead, e.g. MUST in place of "should always".
* Normative statements must be consistent with the OCDS schema, e.g. `ocid` is a required field in the schema so:
  * "the `ocid` field MUST be provided" is consistent.
  * "the `ocid` field SHOULD be provided" is inconsistent.
* When referring to extensions it is not necessary to explicitly state that they are optional.

## Field and code descriptions

* The first sentence of a description should be descriptive of the field and written in a neutral voice, rather than addressing a particular audience, e.g. for `tender/submissionMethod`.
  * "One or more values from the submissionMethod codelist indicating the method(s) by which bids can be submitted" uses a neutral voice.
  * "Specify the method(s) by which bids can be submitted" addresses publishers rather than users.
* Descriptions should balance the needs of expert users, for whom the description serves to assure that use of the field or code is appropriate, and non-expert users, for whom the description of the code serves to help them understand how the field or code is used and whether it is likely to contain the information they are looking for.
* Subsequent sentences may provide information or guidance to assist publishers to use the field effectively or users to interpret the field effectively.
* Guidance sentences should be grounded in clear user needs and implementation experience of common pitfalls or errors.
* For fields or codes whose names and titles use complex or specialist language, consider providing an example to aid non-expert users, e.g.

```eval_rst
================= ===================================================== ===========
code              title                                                 Description
================= ===================================================== ===========
guaranteeReports  Fiscal commitments and contingent liabilities reports Reports detailing the fiscal commitments of the public authority to the PPP, for example known payments that must be made if the PPP proceeds or payment commitments whose occurrence, timing and magnitude depend on some uncertain future event, outside the control of the public authority.
================= ===================================================== ===========
```

* Descriptions should not link to definitions provided on external websites.
* Descriptions should be concise and avoid using exhaustive lists.
* Descriptions should not explicitly state whether a field is required or optional.
* Descriptions should not simply restate the title or name of a field or code.

### Examples

Descriptions for similar fields or codes should be consistent with each other where possible, without discarding information relevant to a specific field.

The following examples can be used to inform descriptions for common types of fields in the schema. Additional information, specific to a particular field, should be provided in a separate sentence after the primary description of the field.

#### Codelists

For single values:

    A value from the <codelist_name> codelist indicating <purpose of codelist>. Further information may be provided in the <codelistDetails_name> field.

For multiple values:

    One or more values from the <codelist_name> codelist indicating <purpose of codelist>. Further information may be provided in the <codelistDetails_name> field.

**Example:**

> One or more values from the submissionMethod codelist indicating the method(s) by which bids can be submitted. Further information may be provided in the submissionMethodDetails field.

#### Identifiers

For the `id` field of items in arrays:

    A locally unique identifier for this <object_name>. Used to track changes to this <object_name> and to [merge](https://standard.open-contracting.org/latest/en/schema/merging/#merging) multiple releases to create a record.

**Example:**

> A locally unique identifier for this document. Used to track changes to this document and to [merge](https://standard.open-contracting.org/latest/en/schema/merging/#merging) multiple releases to create a record.

#### Titles

For the `title` field of an object:

    A title for this <object_name>.

#### Descriptions

For the `description` field of an object:

    A description of this <object_name>. Structured information should be provided in <related_fields>.

**Examples:**

> A description of this tender. Structured information should be provided in the items array. Descriptions should be short and easy to read. Avoid using ALL CAPS.

> A description of this document. Descriptions should not exceed 250 words. In the event the document is not accessible online, the description field may be used to describe arrangements for obtaining a copy of the document.

#### Documents

For the `documents` field of an object:

    All documents and attachments related to this <object_name>, including any official notices.

#### Milestones

For the `milestones` field of an object:

    A list of important dates or events associated with this <object_name>.
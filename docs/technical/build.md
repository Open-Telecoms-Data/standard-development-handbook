# Build the documentation and run tests

This page describes how to build the Sphinx documentation site and run tests against the schema and codelists. It also covers how to fix build errors and resolve test failures.

```{note}
Before running the commands on this page, you need to [set up your development environment](setup).

```

## Build the documentation

Sphinx, which builds the documentation, doesn’t watch directories for changes. To regenerate the documentation, start an HTML server, and refresh the browser whenever changes are made, run:

```bash
cd docs
make autobuild
```

Alternatively, build the documentation and view it using a local web server:

```bash
cd docs
make html
python -m http.server --directory ../_readthedocs/html
```

For guidance on resolving build errors, see [resolve build errors](#resolve-build-errors).

Then go to http://localhost:8000/ in a browser.

## Resolve build errors

If Sphinx, which builds the documentation encounters an error, it provides a detailed log in the terminal. Follow these steps to diagnose an issue:

1.  **Locate the Error:** Look for lines starting with `WARNING:` or `ERROR:`.
2.  **Check the Path:** Sphinx reports the file and line number (e.g., `docs/how-to.md:42`).
3.  **Read the Message:** Identify if it is a missing file, a syntax error, or a broken reference.

### Common MyST/Sphinx Errors

| Error Message | Cause | Resolution |
| :--- | :--- | :--- |
| `myst anchor '...' not found` | You linked to a heading that was renamed or deleted. | Update the link to match the new heading text (converted to a slug). |
| `unknown document: '...'` | A relative path to another `.md` file is incorrect. | Verify the relative path from the current file to the target (e.g., `../about/file`). |
| `Document isn't included in any toctree` | A Markdown file exists but isn't linked in any `index.md`. | Add the filename to the `toctree` directive in the parent `index.md` file. |
| `Unknown directive type` | A block starting with ` {target} ` is misspelled. | Ensure directives like `note` or `tip` use the correct `{}` syntax. |
| `Non-consecutive header level` | You skipped a header level (e.g., `##` to `####`). | Ensure headers follow a logical hierarchy (`#`, `##`, `###`). |
| `Duplicate target name` | Two internal link anchors share the same name. | Ensure all custom cross-reference targets are unique project-wide. |

### Handling Heading & Relative References

Since OFDS uses auto-generated anchors, follow these rules to avoid build failures:

* **Slugs are Sensitive:** If you change a heading from `## Span` to `## Span data`, any link to `#span` will break. You must find and update those references.
* **Check Relative Levels:** When linking between folders, remember that relative paths are relative to the *source file*. 
    * Linking from `docs/guides/test.md` to `docs/about/intro.md` requires `../about/intro.md`.
* **Avoid Special Characters in Headings:** Symbols like `?` or `:` in a heading can make the auto-generated slug unpredictable. Stick to alphanumeric characters and hyphens.

### Generic Troubleshooting Tips

* **Clear the Cache:** If errors persist after a fix, run `make clean` in the `docs` directory to wipe the previous build and [build the documentation](#build-the-documentation) again.
* **Check Blank Lines:** MyST-parser requires blank lines to separate elements like lists, code blocks, and directives.
* **Indent Correctly:** Content inside a directive (like a `{note}`) must be indented consistently.

## Run tests

Python tests perform a variety of checks on the schema, codelists and other files.

Use the following command to run the tests:

```bash
pytest tests
```

For information on how to resolve test failures, see [resolve test failures](#resolve-test-failures).

## Resolve test failures

This section describes how to resolve common failures reported when running `pytest tests`:

### test_json.py::test_empty

Review the warnings summary to identify the empty JSON files and remove the files.

For example, a empty JSON file at `schema/test.json` would be reported as follows, including the command you need to run to remove it:

```
================================================================ warnings summary =================================================================
tests/test_json.py::test_empty
  ERROR: schema/test.json is empty, run: rm schema/test.json
```

### test_json.py::test_indent

Review the warnings summary to identify the misindented files and run following command to indent the file:

```bash
ocdskit indent path/to/file.json
```

For example, a misindented schema would be reported as follows:

```
================================================================ warnings summary =================================================================
tests/test_json.py::test_indent
  ERROR: schema/network-schema.json is not indented as expected
```

To indent the file, run:

```bash
ocdskit indent schema/network-schema.json
```

If there are multiple misindented files, run the following command to recursively indent all JSON files in the repository:

```bash
ocdskit indent -r .
```

### test_json.py::test_invalid_json

Review the warnings summary to identify the invalid JSON files, and correct the errors.

For example, a JSON file with the following content would be invalid because the `,` is missing after the first key-value pair:

```json
{
    "key_1": "value_1"
    "key_2": "value_2"
}
```

The failure would be reported as follows:

```
================================================================ warnings summary =================================================================
tests/test_json.py::test_invalid_json
  ERROR: schema/test.json is not valid JSON: Expecting ',' delimiter: line 3 column 5 (char 19)
```

To resolve the issue, you would open `schema/test.json` and use the reported line and column numbers to identify the location of the error in the file.

### test_json.py::other tests

`test_json.py` performs a variety of structural and quality checks on the OFDS schema to ensure it is valid, consistent, and follows certain design patterns. These tests utilize the [JSCC testing library](https://jscc.readthedocs.io/en/latest/index.html) to enforce standards like proper metadata presence and correct property definitions.

### Understanding the Error Output

When a schema test fails, the terminal output will provide a **JSON Pointer** and a specific error message. Use these to identify the cause of the issue:

* **The Pointer:** A path like `schema/network-schema.json/properties/id` tells you exactly where in the schema the error was found.
* **The Message:** Describes the logic violation, such as a missing description, an invalid type, or a missing property.

### Common Resolution Steps

Most failures in this suite can be resolved by following these general principles:

* **Check for Missing Metadata:** OFDS requires most fields to have a `title` and `description`. Ensure these are present and not empty for any new properties.
* **Verify Property Definitions:** If a property is listed as `required`, it must be defined in the `properties` section of that object, or inherited via `allOf`.
* **Enforce Strict Types:** The OFDS schema generally does not allow `null` types (e.g., `["string", "null"]`). Use a single specific type and make the field optional by omitting it from the `required` list instead.
* **Synchronize Codelists and Enums:** The `validate_codelist_enum` check ensures that the schema's `enum` values match the corresponding CSV codelist. 
    * If `openCodelist` is set to `false`, an `enum` must be defined and match the CSV.
    * If `openCodelist` is `true`, an `enum` should not be present.
* **Object Identifiers:** Ensure that objects inside arrays have an `id` field, to support stable data merging.

```{tip}
 For more information on the underlying logic of each check, refer to the [JSCC documentation](https://jscc.readthedocs.io/en/latest/api/testing/checks.html).
```
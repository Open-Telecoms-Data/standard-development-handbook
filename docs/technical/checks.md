# Checks and tests

The standard repository uses [GitHub Actions](https://docs.github.com/en/actions) to run automated checks each time you push a commit to GitHub.

With the exception of [Link Check](#link-check), all checks must pass successfully before you can merge a pull request.

Checks are defined by the YAML files in the [`.github/workflows` directory](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/tree/0.1-dev/.github/workflows).

This page describes the purpose of each check, how to run it locally and what to do about failures.

Before running checks locally, you need to follow the [getting started instructions](build.md#get-started).

## Link Check

Check the integrity of all external links.

To run locally:

```bash
cd docs
make linkcheck
```

To correct failures, review the output and fix broken links, where possible. This check may report false positives, for example, when linking to anchors in Markdown files hosted on GitHub. Therefore, failures do not block merging.

## MD Format

Enforce a consistent style in Markdown files.

To run locally:

```bash
mdformat --check docs
```

To correct failures, run the following command:

```bash
./manage.py pre-commit
```

## Sphinx Build

Build the documentation and warn about all references where the target cannot be found.

To run locally:

```bash
sphinx-build -nW ./docs/ ./temp_build
```

To correct failures, review the output and fix broken references.

## Run all tests

Run the tests defined in the [`/tests` directory](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/tree/0.1-dev/tests).

Tests are written using the [pytest framework](https://docs.pytest.org/en/7.2.x/) and use methods from [JSON Schema and CSV Codelists](https://jscc.readthedocs.io/en/latest/).

There are two test files:

* `test_csv.py` - Ensure that all CSV files are valid: no empty rows or columns, no leading or trailing whitespace in cells, same number of cells in each row.
* `test_json.py`- Ensure that:
  * All JSON files are non-empty, correctly indented and valid.
  * Codes in codelist CSV files match enums in the schema.
  * Codelist CSV files are referenced in the schema.
  * Codelists CSV files referenced in the schema exist.
  * JSON Schema files are valid.

To run locally:

```bash
pytest tests
```

To correct failures, review the warnings and recommended actions in the output.

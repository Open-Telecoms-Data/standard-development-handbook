# Additional pre-commit and build-time processing

This page is for maintainers of the **build tooling** in the `open-fibre-data-standard` repository, i.e. the code that takes `network-schema.json` and derives the logical data model, the CSV and GeoPackage formats, and the rendered documentation. It is not aimed at people editing the schema's content, writing prose in the docs, or reading the published standard.

Everything described here programmatically derived from one source of truth: `schema/data_formats/json/network-schema.json`. None of the derived files should be hand-edited.

There are two distinct processing stages, run at different times, by different tools:

1. **`manage.py pre-commit`** is run manually by a maintainer when `network-schema.json` is updated. The resulting changes are checked into the Git repository. The script regenerates the data model CSVs, the GeoPackage template and example, the CSV example/template, the CSV reference docs, and the GeoPackage ER diagram.
2. **`docs/conf.py`'s `env-before-read-docs` hook** is run automatically by Sphinx every time the docs are built (locally or on Read the Docs). It copies the schema and GeoPackage template into the build output, substitutes `{{version}}` placeholders, and derives the GeoPackage table-definition CSVs used in the reference documentation.

The Sphinx build then renders MyST directives (`{jsonschema}`, `{jsoninclude-quote}`, `{mermaid}`, `{csv-table}`, …) that read the files produced by both stages.

## `manage.py pre-commit`

The pre-commit script is defined in `manage.py` at the repo root. Run it with:

```bash
python manage.py pre-commit
```

A GitHub Actions workflow (`.github/workflows/precommit.yml`) checks that derived files were updated and checked-in.

The command performs the following steps, in order:

### 1. Update `schema/data_formats/json/network-schema.csv`

`network-schema.csv` is a flat CSV rendering of the JSON data format schema, linked from the JSON data format reference page. It is **not** related to the CSV *publication format*.

### 2. Extract the logical data model (`schema/data_model/`)

The logical data model is extracted by `update_data_model_docs()` and `extract_model()` in `manage.py`. This walks the dereferenced schema and reconstructs the logical entity/relationship/attribute model that the JSON Schema encodes implicitly through `$ref`s. The walk is driven by four custom, non-standard JSON Schema keywords maintained by hand in `network-schema.json`:

| Keyword | Where it appears | Meaning |
| --- | --- | --- |
| `x-logical-type: entity` | on a `$defs` entry | This definition is a first-class entity (`Node`, `Span`, `Organisation`, etc.) and gets its own row in `entities.csv` and its own subdirectory. |
| `x-logical-type: attribute` | on a `$defs` entry | This definition is a leaf value object (e.g. `PointGeometry`), i.e. don't recurse into it or treat it as an entity; record it as a single attribute on whatever references it. |
| `x-logical-type: exclude` | on a property | Omit this property from the logical model entirely (used for format-specific plumbing that has no logical-model meaning). |
| `x-references` | on a property or `$defs` entry | Marks a property as a foreign-key-style reference to an entity (used where the JSON representation nests a sub-object, e.g. `OrganisationReference`, rather than using `$ref` directly). |
| `x-relationship-label` | on a property | Human-readable label for the relationship, used as the edge label in the Mermaid ER diagram. |

If you add a new entity or a new kind of reference to the schema, you almost certainly need to add one of these keywords for it to show up correctly in the data model. The extraction script does not infer entity-ness from structure alone.

For each entity, `extract_model()` accumulates relationships and attributes and `update_data_model_docs()` writes the following files:

- `schema/data_model/entities.csv`: the list of entity names (one row per entity)
- `schema/data_model/<entity>/relationships.csv`: one row per relationship, rendered later as a `{csv-table}`
- `schema/data_model/<entity>/attributes.csv`: one row per attribute (not directly rendered in docs, but available for tooling)
- `schema/data_model/<entity>/directive.txt`: a ready-to-`{include}` MyST `{jsonschema}` directive block that renders the entity's attributes from `network-schema.json`
- `schema/data_model/data_model.mmd`: a Mermaid `erDiagram` covering every entity and relationship.

### 3. Generate the GeoPackage template (`schema/data_formats/geopackage/network-schema.gpkg`)

Built by `buildofdsgeopackage.Builder` (`schema/data_formats/geopackage/buildofdsgeopackage.py`) from OFDS Studio and invoked from `manage.py`.

Fixed dictionaries in `Builder.__init__` (`MAPPING_FOREIGN_KEY_NAMES_TO_LAYERS`, `MAPPING_MANY_TO_MANY_KEY_NAMES_TO_LAYERS`) map named schema properties (e.g. "Network providers", "Related phases") onto the GeoPackage layer they should reference. **If a new foreign-key or M:N property is added to the schema, it needs an entry here too**, or the builder won't know which table to point the relationship at.

### 4. Update the example GeoPackage (`examples/geopackage/network.gpkg`)

`manage.py` calls `populate_gpkg.populate_geopackage()` (`schema/data_formats/geopackage/populate_gpkg.py`) from OFDS Studio to populate the `network-schema.gpkg` template with the canonical example data from `examples/json/network-package.json`. The output is written to `examples/geopackage/network.gpkg`, which is committed to the repo and linked from the GeoPackage reference page.

### 5. Update the GeoPackage ER diagram (`docs/reference/data_formats/geopackage/geopackage.mmd`)

Generated by shelling out to the external tool [`mermerd`](https://github.com/KarnerTh/mermerd) against the example built in the previous step.

`manage.py` then post-processes the raw `mermerd` output to prepend frontmatter, strip unwanted columns, and add styling.

The result is the diagram rendered on `docs/reference/data_formats/geopackage/index.md`. **If you add a new table or rename an existing one**, update `geopackage.yaml`'s `ignoreTables` list and the `class ...` lines in `manage.py`'s footer-construction code accordingly, or the new table will either be missing from the diagram or left in its default (unclassed) colour.

### 6. CSV example and template (`examples/csv/` and `examples/csv/template/`)

Both generated by [`flattentool`](https://github.com/OpenDataServices/flatten-tool) from `network-schema.json`.

### 7. CSV reference documentation (`docs/reference/data_formats/csv.md`)

Generated by `update_csv_docs()` / `generate_csv_reference_markdown()` in `manage.py`. It recurses through the dereferenced schema to construct table names, and for each table emits a "related tables" bullet list, links to the corresponding example (`examples/csv/<table>.csv`) and blank template (`examples/csv/template/<table>.csv`), and a `{jsonschema}` directive against `_readthedocs/html/network-schema.json` (see Stage 2 — this only exists after a docs build) with `:include:` listing exactly the JSON pointers that correspond to that table's columns, and `:collapse:` for the WKT-collapsed `location`/`route` properties.

**If the table/nesting structure that `flattentool` derives from the schema ever diverges from what `generate_csv_reference_markdown()` derives (they are two independent implementations walking the same schema), the docs and the actual CSV files will disagree.**

### 8. `mdformat`

The final step runs `mdformat docs` to normalise the Markdown formatting of everything under `docs/`.

## `docs/conf.py`'s build-time processing

The build-time processing runs with every docs build via `env-before-read-docs`, which does the following:

1. Copies `schema/data_formats/json/network-schema.json` to `<outdir>/network-schema.json` and substitutes `{{version}}` placeholders in the copy.
3. Copies `schema/data_formats/geopackage/network-schema.gpkg` to `<outdir>/network-schema.gpkg` substitutes `{{version}}` placeholders in the copy.
4. Extracts GeoPackage table-definition CSVs from the processed GeoPackage template. erroring.
5. Copies `network-package-schema.json` and the whole `schema/codelists/` tree into the output directory unchanged (no placeholder substitution needed).
# Contributing

This page describes the internal process for making changes to the standard repository.

## Pre-reading

Before contributing to Markdown pages, read the [markdown style guide](../style/markdown_style_guide.md).

Before contributing to JSON Schema and CSV codelists, read the [schema style guide](../style/schema_style_guide.md) and the [schema patterns](https://os4d.opendataservices.coop/patterns/schema/) section of [Standards Lab](https://os4d.opendataservices.coop/).

To get up to speed on OFDS standard development, you should be familiar with:

-  The standard itself

   -  [OFDS documentation](https://github.com/Open-Telecoms-Data/open-fibre-data-standard)

-  The policies it follows

   -  [Semantic versioning](https://semver.org)
   
To improve your technical writing skills, consider taking [Google's Technical Writing Courses](https://developers.google.com/tech-writing), which can be completed in a day.

## Workflow

Before making changes to the standard, you should first propose them in an [issue](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/issues). This allows other contributors to discuss the change and provide feedback before you start work on it.

1. **Setup:** You first need to [set up your development environment](../technical/setup.md) so that you can update derived files, and check for build errors and test failures before pushing your changes to GitHub.
1. **Branch:** Create a 'feature' or 'topic' [branch](https://docs.github.com/en/pull-requests/reference/branches) from the current staging (`X.Y-dev`) branch of the [open-fibre-data-standard](https://github.com/Open-Telecoms-Data/open-fibre-data-standard) repo, e.g. `0.5-dev`. The usual feature branch naming convention is `<issue-number>-<short-description>`, e.g. `123-add-new-entity`.
1. **Edit files:** Make your changes by editing the source files in the repository's [directory structure](../technical/repository.md#structure), following the guidance in [editing files](#editing-files) and [logging changes](#logging-changes). Do not edit derived files, which are generated from the source files.
1. **Update derived files:** Run `manage.py pre-commit` to update the derived files in the repository, including the data model CSVs, diagrams, and Sphinx directives.
1. **Build:** [Build the documentation](../technical/build.md#build-the-documentation) and [resolve any build errors](../technical/build.md#resolve-build-errors).
1. **Test:** [Run tests](../technical/build.md#run-tests) to identify common issues and [resolve any failures](../technical/build.md#resolve-test-failures).
1. **Commit:** Commit your changes, following the guidance on [atomic commits](#atomic-commits) and [commit messages](#commit-messages).
1. **Merge:** Create a pull request to merge your changes into the current staging branch and assign it to another team member for review. If changes are requested, make the changes and repeat this step. Once approved, you can merge it yourself.

## Guidance

### Editing files

When editing files, follow these guidelines:

   * Only edit the source files. The source files are:
      * Markdown files in the `docs/` directory, which are rendered into the published documentation.
      * JSON Schema files in the `schema/data_formats/json` directory, which define the schema for the JSON data format, and are used to derive the data model and other data format schemas and documentation.
      * CSV files in the `codelists/` directory, which define the controlled vocabularies used in the data model and schemas.
      * JSON files in the `examples/json` directory, which provide example data for the JSON data format, and are used to derive the GeoPackage and CSV example files.
   * If you add a new definition in `network-schema.json`:
      * Add the necessary [custom keywords for extracting the logical data model](schema.md#custom-keywords-for-extracting-the-logical-data-model) to control its representation in the logical data model.
      * Add a subheading for the new definition in `docs/reference/data_formats/json/reference.md`, following the pattern of the other definitions.
      * If the definition is represented as an entity in the logical data model, add a subheading for it in `docs/reference/data_model/index.md`, following the pattern of the other entities.
   * If you add a new foreign-key or M:N property to `network-schema.json`:
      * Add the necessary [custom keywords for extracting the logical data model](schema.md#custom-keywords-for-extracting-the-logical-data-model) to control its representation in the logical data model.
      * Add an entry for it in `schema/data_formats/geopackage/buildofdsgeopackage.py`'s `MAPPING_FOREIGN_KEY_NAMES_TO_LAYERS` or `MAPPING_MANY_TO_MANY_KEY_NAMES_TO_LAYERS` dictionaries, so that the GeoPackage builder knows which table to point the relationship at.
   * If you add a new table or rename an existing one, update `geopackage.yaml`'s `ignoreTables` list and the `class ...` lines in `manage.py`'s footer-construction code, so that the GeoPackage ER diagram is generated correctly.
   * If you use a JSON Schema validation keyword, type or format that is not [already used in the schema](schema.md#json-schema-usage):
     * Update the list of validation keywords, types or formats in [JSON Schema usage](schema.md#json-schema-usage).
     * Add a field that fails validation against the new keyword, type or format to [`network-package-invalid.json`](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/examples/json/network-package-invalid.json).
     * Check that [OFDS CoVE](https://ofds.cove.opendataservices.coop/) reports an appropriate validation error.
   * If you add a normative rule to a schema description or reference documentation, that is not enforced in the JSON Schema:
      * Update the list of [other normative rules](schema.md#other-normative-rules).
      * Add a field that does not conform to the rule to [`network-package-additional-checks.json`](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/examples/json/network-package-additional-checks.json)/
      * Open a [new issue](https://github.com/Open-Telecoms-Data/lib-cove-ofds/issues/new/choose) to add an additional check to Lib Cove OFDS.
   * When writing Markdown content:
      * Follow the [common conventions](../style/common_conventions.md) and [Markdown style guide](../style/markdown_style_guide.md).
      * **Never** use normative words on guidance pages. Use [non-normative synonyms](https://tools.ietf.org/html/draft-hansen-nonkeywords-non2119-04#page-3) instead.
      * Consider composing Markdown content in [Hemingway Editor](http://www.hemingwayapp.com/) or [Grammarly](https://www.grammarly.com/), as suggested in the [common conventions](../style/common_conventions.md).

### Atomic commits

Make atomic changes in one commit, rather than over many commits: for example, when adding a definition to the schema, add it to the schema reference documentation in the same commit. That way, reverting a commit doesn't leave the standard in an incoherent state.

### Commit messages

Use the following format for commit messages:

```
   scope: Capitalized, <72 characters, no period

   A longer description of paragraph text, as needed.
```

For example:

```
   primer/index: Use "JSON data" instead of "JSON text"
```

The *scope* is based on which files were changed:

* The changelog file: `changelog`
* One Markdown file (can include changes to example files): `path/to/page`, for example: `primer/index`
* Many Markdown files in a single section: `path/to/section`, for example: `guidance`
* One schema file (can include changes to reference pages): the name of the file without extension, for example: `schema`

Other, less-used scopes are:

* `build`: Changes to the build system (requirements files, ``include``, ``script``, ``Makefile``, ``*.cfg``, ``*.py``)
* `ci`: Changes to continuous integration (``.github/workflows``)
* `locale`: Changes to translations (``docs/locale``)
* `test`: Changes to tests (``tests``)
* `chore`: Any other change not warranting a changelog entry (e.g. renaming pages or fixing typographical errors, broken URLs, Markdown syntax, etc.)

Most commits are made in pull requests, such that it's easy to find the discussion on GitHub. As such, it's not necessary to provide a long narrative, if it exists in a pull request or linked issue.

Reference:

- [Angular Commit Message Format](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit-message-header)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Write joyous git commit messages](https://joshuatauberer.medium.com/write-joyous-git-commit-messages-2f98891114c4)

### Logging changes

Update the changelog with a short description of the change, following the [changelog style guide](../style/changelog_style_guide) and maintaining schema order in the list of changes.

### Reviewing changes

You can use GitHub's [suggestions feature](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/reviewing-proposed-changes-in-a-pull-request) to suggest changes directly.

You should check for:

-  **Correctness**: Do the changes conform to the standard and its principles?
-  **Style**: Do the changes respect the [style guides](../style/index.md)? Are [normative words](https://tools.ietf.org/html/draft-hansen-nonkeywords-non2119-04#page-3) used on guidance pages?
-  **Spelling and grammar**: If there are few errors, suggest changes directly. If there are many errors, ask the author to use Grammarly or similar.

## Repository management

### Issues

-  [All issues](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/issues) should be assigned one or more [labels](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/issues/labels).

### Milestones

-  Each development version should have a milestone, like `alpha` or `beta`. There should at most one milestone for each of the patch, minor and major levels.
-  [All issues](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/issues) should be assigned to a [milestone](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/milestones).

### Projects

-  Each version under active development should have a [organisation-wide project](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/projects?type=new).

## Modelling approaches

Before proposing new structures:

1. Draft a JSON example with reasonable values
2. Check [other standards](https://lov.linkeddata.es/dataset/lov) for potential models

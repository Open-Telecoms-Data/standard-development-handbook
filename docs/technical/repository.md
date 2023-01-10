# Standard repository

## Branches and tags

The standard uses [semantic versioning](https://semver.org/), with versions following the *MAJOR.MINOR.PATCH* name convention.

Each minor version of the standard's documentation is built from a "live" branch named after the version, like `0.1`. For each live branch, there is a dev branch with a `-dev` suffix. Patch versions may further branch off the dev branch, with work merged into the dev branch before finally being merged into the live branch. [open-fibre-data-standard.readthedocs.io](https://open-fibre-data-standard.readthedocs.io) redirects to (`https://open-fibre-data-standard.readthedocs.io/en/latest/`), which points to the most recent live branch specified as the default branch in the [Read the Docs settings](https://readthedocs.org/dashboard/open-fibre-data-standard/advanced/); this makes it possible to link to the current version of the documentation without specifying the version number.

Sample branch structure:

-   **0.1**: contains the deployed version of OFDS 0.1
-   **0.2**: contains the deployed version of OFDS 0.2
    -   **0.2-dev**: used to stage changes to version 0.2 (such as minor documentation changes)
    -   **0.3-dev**: used to stage changes for version 0.3 (such as schema changes)

The `X.X` and `X.X-dev` branches are [protected](https://help.github.com/articles/about-protected-branches/).

The published documentation has versions on `MAJOR.MINOR` [branches](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/branches) (e.g. <https://open-fibre-data-standard.readthedocs.io/en/0.1/>), whereas the published schema has versions on different `MAJOR__MINOR__PATCH` [tagged releases](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/tags) (e.g. <https://raw.githubusercontent.com/Open-Telecoms-Data/open-fibre-data-standard/0__1__0__beta/schema/network-schema.json>). This use of branches and tags allows documentation to change between versions, while ensuring schema isn't changed between versions.

## Structure

- [.github/workflows](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/.github/workflows): GitHub Actions workflows for CI and tests
- [_assets](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/_assets): images used in the documentation
- [codelists](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/codelists): codelist CSV files
- [docs](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/docs): documentation
  - [.tx/config](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/docs/.tx/config): configuration of Transifex (not yet implemented)
  - [\_static/](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/docs/_static): CSS and JavaScript for the documentation
  - [\_templates/](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/docs/_templates): Jinja templates for the documentation
  - `*.md` `*/*.md`: English documentation text
  - [conf.py](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/docs/conf.py): Sphinx configuration
  - [locale/](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/docs/locale/): translations of the English documentation (not yet implemented)
- [examples](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/examples): examples of OFDS data in different publication formats and data use examples
- [schema](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/schema): schema-related files
  - `*.json`: JSON Schema files
  - `*.csv`: CSV representation of JSON Schema files
- [tests/](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/HEAD/tests): Python tests

The following files are created by running a build and are not version controlled:

-   `.ve/`: Python virtualenv, containing all dependencies
-   `build/`: contains the built copy of the documentation website

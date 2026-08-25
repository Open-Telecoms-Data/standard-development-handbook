# Introduction

This page provides an introduction for new maintainers of the Open Fibre Data Standard (OFDS).

## The tech stack

The standard is built using a ["Docs as Code"](https://www.writethedocs.org/guide/docs-as-code/) approach, ensuring that the schema, codelists and documentation remain synchronized.

* **Schema:** [JSON Schema 2020-12](https://json-schema.org/understanding-json-schema/reference#json-schema-reference) with the [Open Data Services JSON Schema Extension](https://docs.opendataservices.coop/projects/json-schema-extension/latest/)
* **Codelists:** CSV files structured according to the [Open Data Services Codelist Schema](https://docs.opendataservices.coop/projects/codelist-schema/latest/)
* **Documentation:** [Sphinx](https://www.sphinx-doc.org/) using [MyST-Parser](https://myst-parser.readthedocs.io/) for Markdown support.  
* **Language:** Python 3.12+ (for build scripts, testing, and CLI utilities).  
* **Hosting:** [Read the Docs](https://readthedocs.org/) for automated versioned deployments.  
* **Quality Assurance:** [pytest](https://docs.pytest.org/en/stable/) for logic/schema validation, [mdformat](https://mdformat.readthedocs.io/en/stable/) for Markdown formatting, [GitHub Actions](https://docs.github.com/en/actions) for continuous integration.

## How the documentation is built

The following diagram illustrates how the source files in the repository are transformed into the final published standard.

```mermaid
    graph LR
        subgraph Repo["Git Repository"]
        direction LR

            subgraph Source["Source Files"]
                direction LR
                S1[JSON Schema]
                S2[Codelist CSVs]
                S3[Markdown Docs]
                S4[JSON Examples]
            end

            R1["manage.py pre-commit"]

            subgraph Derived["Derived Files"]
                direction LR
                D1["Data Model CSVs, Diagram and Sphinx Directives"]
                D2["JSON Schema CSV"]
                D3["GeoPackage Template and Diagram"]
                D4["CSV Template and Examples"]
                D5["CSV Reference Documentation"]
            end

        end
        


        subgraph Build["Build Process (Sphinx + conf.py)"]
            direction TB
            B1["Replace version placeholders in JSON schema and GeoPackage template"]
            B2[Extract GeoPackage table definitions]
            B3[Build HTML files]
        end

        subgraph Output["Documentation site (Read the Docs)"]
            direction LR
            O1[Processed JSON Schema]
            O2[Processed GeoPackage Template]
            O3[HTML Website]
        end

        Source --> R1 --> Derived
        Repo --> Build
        B1 --> B2--> B3
        Build --> Output
```

## Contribution workflow

The following diagram illustrates the workflow for making changes to the schema, codelists and documentation.

```mermaid
    flowchart LR

        1[Set Up]
        2[Branch]
        3[Edit]
        4{Build}
        5{Test}
        6[Commit]
        7[Merge]

        1 --> 2
        2 --> 3
        3 --> 4
        4 -->|Fail| 3

        4 -->|Succeed| 5
        5 -->|Fail| 3
        5 -->|Pass| 6
        6 --> 7
```

The key steps in the workflow are:

1. **Set up your development environment:** Install the tools you need to edit, test and build the standard. The recommended approach is to [use GitHub Codespaces](setup.md#use-github-codespaces), a pre-configured and hosted development environment.
2. **Create a branch:** Create a branch from the current staging branch, to develop features, fix bugs, or safely experiment with new ideas in a contained area of the repository.
3. **Edit files:** Edit the schema, codelists and/or Markdown files in the repository's [directory structure](repository.md#structure).
4. **Build the documentation**:** Check that the build succeeds, [correct any errors](build.md#resolve-build-errors), and preview your changes locally.
5. **Test the schema and codelists:** [Run tests](build.md#run-tests) to identify common issues and [resolve any failures](build.md#resolve-test-failures).
6. **Commit your changes:** Once the docs build succeeds, the tests pass, and you are happy with your changes, commit them to your branch.
7. **Merge your changes:** Create a pull request to merge your changes into the current staging branch for inclusion in a future release.
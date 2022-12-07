# Contributing

This page describes the internal process for making changes to the standard repository.

## Getting started

Before contributing to Markdown pages, read the [markdown style guide](../style/markdown_style_guide.md).

Before contributing to JSON Schema and CSV codelists, read the [schema style guide](../style/schema_style_guide.md) and the [schema patterns](https://os4d.opendataservices.coop/patterns/schema/) section of [Standards Lab](https://os4d.opendataservices.coop/).

To get up to speed on OFDS standard development, you should be familiar with:

-  The standard itself

   -  [OFDS documentation](https://github.com/Open-Telecoms-Data/open-fibre-data-standard)

-  The policies it follows

   -  [Semantic versioning](https://semver.org)
   
To improve your technical writing skills, consider taking [Google's Technical Writing Courses](https://developers.google.com/tech-writing), which can be completed in a day.

## Proposing changes

1. Agree on a proposal in a GitHub issue.
1. Assign the issue to yourself, and move the issue's card to the *In progress* column.
1. Create a branch of the `open-fibre-data-standard` repo (not a branch of your fork) in which to make your changes.

   -  **Never** use normative words on guidance pages. Use [non-normative synonyms](https://tools.ietf.org/html/draft-hansen-nonkeywords-non2119-04#page-3) instead.
   -  Consider composing Markdown content in [Hemingway Editor](http://www.hemingwayapp.com/) or [Grammarly](https://www.grammarly.com/), as suggested in the [common conventions](../style/common_conventions.md).
   -  After adding a definition to `schema.json`, add a sub-heading for the new sub-schema in `reference.md`.

1. Update the **changelog**, maintaining schema order in the list of changes.
1. Run `./manage.py pre-commit` to update the reference documentation
1. If you used a validation keyword, type or format that is not [already used in the schema](schema.md#json-schema-usage):
  1. Update the list of validation keywords, types or formats in [JSON Schema usage](schema.md#json-schema-usage).
  1. Add a field that fails validation against the new keyword, type or format to [`network-package-invalid.json`](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/examples/json/network-package-invalid.json).
  1. Check that [OFDS CoVE](https://ofds.cove.opendataservices.coop/) reports an appropriate validation error.
1. If you added a normative rule that is not encoded in JSON Schema:
  1. Update the list of [other normative rules](schema.md#other-normative-rules).
  1. Add a field that does not conform to the rule to [`network-package-additional-checks.json`](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/examples/json/network-package-additional-checks.json)/
  1. Open a [new issue](https://github.com/Open-Telecoms-Data/lib-cove-ofds/issues/new/choose) to add an additional check to Lib Cove OFDS.
1. Commit your changes.

```{admonition} Atomic changes

  Make atomic changes in one commit, rather than over many commits: for example, when adding a definition to the schema, add it to the schema reference documentation in the same commit. That way, reverting a commit doesn't leave the standard in an incoherent state.
  
```

````{admonition} Commit messages

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
````

1. Create a pull request.

   -  Write the pull requests' description, filling in [the relevant bits of the provided template](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/blob/0.1-dev/pull_request_template.md).
   -  Set the *base* branch, e.g. `main`.
   -  Set the *milestone*, e.g. `1.0`.

1. Assign to another team member to review.

   -  See the next section for reviewer's instructions.
   -  If changes are requested, make the changes, then repeat this step.

1. Once approved, you can merge it yourself.

## Pull Request Tests

When you submit a pull request, some tests will be run against it automatically.

### Linkcheck

This will check all external links are valid, and error if not.

If this fails for your pull request, that is ok. This may be due to existing problems or a link in another section of the docs that used to work but now doesn't.

Check the details to make sure that any failures aren't due to any links you have added, and if so attempt to fix them.

### Tests

Various Python tests will run. This must pass before your pull request can be merged.

To run this locally, see [instructions on the build page](../technical/index).

### Mdformat

This formats the md files to a standard specification. This must pass before your pull request can be merged.

This should be done by your pre-commit hook. If this fails, make sure your pre-commit hook is running correctly.

### Build test

This tries to build the site, and treats any warnings as problems as well as errors. This must pass before your pull request can be merged.

To run this locally, see [instructions on the build page](../technical/index).

## Logging changes

1. Follow the [changelog style guide](../style/changelog_style_guide).

## Reviewing changes

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

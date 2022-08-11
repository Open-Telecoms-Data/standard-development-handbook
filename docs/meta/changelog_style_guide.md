# Changelog style guide

* Use the [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) format.
* Begin each entry with a a link to the pull request for the change.

## Normative content

Changelog entries should be descriptive:

* Bad entry: Update schema.
* Good entry: Make `name` required.

If changes are made to files under the `schema` directory, it is assumed that corresponding changes were made to files under the `docs` directory. Do not add an entry under the "Documentation" heading if the changes directly correspond to entries under the "Codelists" and/or "Schema" headings.

For changes to schema and codelists, preserve schema/codelist ordering when adding new changelog entries. Otherwise, to reduce merge conflicts, add new changelog entries to the end of the relevant bullet list.

## Non-normative content

Changelog entries should be descriptive. Do not add an entry like "Improve primer." Instead, simply add the PR number to the "Primer" list item.

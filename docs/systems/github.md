# GitHub

## Organizations

* [open-contracting](https://github.com/open-contracting/): all repositories supported by OCP
* [open-contracting-extensions](https://github.com/open-contracting-extensions/): OCDS extensions and OCDS profiles
* [open-contracting-archive](https://github.com/open-contracting-archive/): all unsupported repositories
* [open-contracting-partnership](https://github.com/open-contracting-partnership/): [www.open-contracting.org](https://www.open-contracting.org/)-related repositories

## Issues

### Managing long issues

Some issues produce long discussions, and the original intent of the issue may change over time; this can make it more difficult to catch up on the current state of the issue. To deal with this, the issue's description should be edited to reflect the current state of the issue, summarize the discussion so far, and link to the most recent comment from which the discussion may continue.

Restarting the discussion with a new issue causes the following problems:

* Old participants aren't notified of activity on the new issue, and need to subscribe to it.
* New participants need to read all issues referencing the new issue to rebuild the context.
* If the old issue is closed in favor of a new issue, and the new issue is thereafter not resolved but is closed (for whatever reason, like insufficient demand), a reader of the old issue may assume that the new issue is either open or resolved. They would need to follow the chain to realize that it's unresolved and closed, before adding a comment to say, "I need this."

### Automatically closing issues with pull requests

When creating a pull request that fixes one or more issues, add the text "fixes #42" or "closes #42" in the pull request's description so that GitHub [automatically closes them after merging](https://help.github.com/articles/closing-issues-using-keywords/).

### Manually closing issues

All issues should be closed with a brief rationale. This makes it easy to understand what happened and affords participants an opportunity to engage with the rationale. For example:

* "Resolved in the above commit" if there's a commit referencing the issue that appears nearby
* "Resolved in the [name] extension" with a link to the extension that was created
* "Closing, because [explanation]"

## Projects

The [`open-contracting` organization](https://github.com/orgs/open-contracting/projects) uses [GitHub Projects](https://help.github.com/articles/about-project-boards/) to organize work in a [Kanban](https://en.wikipedia.org/wiki/Kanban) system.

We also have an [agile board](https://crm.open-contracting.org/projects/ocds-team-tools-development-portfolio/agile/board) in the CRM. Its uses and features are distinct (e.g. time tracking) and complementary (e.g. long-term project management) to what GitHub offers.

People expect to have visibility of all of a repository's issues within the *Issues* tab; therefore, a card that is not attached to an issue should never be added to a project.

New projects should be created using the *Automated kanban* template.

## Branches

In general, repositories should have only a default branch (usually `master`) and pull request branches. If the repository is a fork, it may have a `master` branch for the source branch and an `opencontracting` (or `open_contracting`) branch for the fork branch.

If a repository needs multiple branches (like the standard and profiles), the needed branches should be protected. Otherwise, unprotected branches more than a month old should either be opened as pull requests, protected, or deleted.

See the pages for the branch management of the [standard](../../standard/technical/repository) and [profiles](../../profiles/technical/repository) (including OC4IDS).

## Repository settings

### Metadata

1. Add a description. The description should not describe the project's status ('draft'), because people frequently forget to update repository descriptions. Describe the status in the readme instead.
1. Add a website to the repository, if relevant: for example, a link to a deployment of the tool or to its documentation.

The `fix:lint_repos` and `fix:protect_branches` Rake tasks in [standard-maintenance-scripts](https://github.com/open-contracting/standard-maintenance-scripts) otherwise normalize configurations.

We don't generally enable the following behaviors on [protected branches](https://help.github.com/articles/about-protected-branches/) for the provided reasons:

* **Require branches to be up to date before merging**: While this may avoid introducing errors, it slows development in an environment in which there are many simultaneous pull requests, because each would require an extra step before merging. If the automated tests fail after merging, the error can be corrected, or the changes can be reverted.
* **Require pull request reviews before merging**: While this is a best practice, it slows development as the team is not sufficiently large to staff it. It is okay, for example, for an author to self-merge a simple change. Authors may, of course, request reviews for significant changes.

### Archival

Repositories that are no longer supported should be archived.

1. Agree whether to archive the repository. The archived repositories presently include:
    * Superseded repositories (e.g. [json-merge-patch](https://github.com/OpenDataServices/json-merge-patch) supersedes [jsonmerge](https://github.com/open-contracting-archive/jsonmerge))
    * Abandoned extensions (e.g. [ocds-equityTransferCaps-extension](https://github.com/open-contracting-archive/ocds-equityTransferCaps-extension))
    * Merged changes to the core standard, expressed as extension repositories (`ocds_upgrade_###`)
    * Exploratory repositories from pre-1.0 and pre-2015
1. Scan the repository's open issues, milestones, pull requests and non-default branches in case any can be quickly closed, merged or deleted. Counter [GitHub's recommendation](https://help.github.com/articles/about-archiving-repositories/), open issues and pull requests indicate the development status of a repository, and should be left open.
1. Change the repository's description to describe the reason for archival. If the repository has been superseded, change it to "Superseded by [owner]/[repository]" and change the URL to the new repository's URL.
1. Run the [`fix:archive_repos REPOS=repo1,repo2`](https://github.com/open-contracting/standard-maintenance-scripts#change-github-repository-configuration) task on the repository.
1. Move the archive to the `open-contracting-archive` organization.
1. [Archive](https://help.github.com/articles/about-archiving-repositories/) the repository through its settings.
1. Run the [`local:badges`](https://github.com/open-contracting/standard-maintenance-scripts#change-github-repository-configuration) task.

## Integrations

* **Coveralls**, to measure test coverage
* **ReadTheDocs**, to build repository-specific documentation ([access all docs](https://github.com/open-contracting/standard-maintenance-scripts/blob/master/badges.md#readme))
* **GitHub Actions**, to run tests, and to build documentation ([view all badges](https://github.com/open-contracting/standard-maintenance-scripts/blob/master/badges.md#readme))
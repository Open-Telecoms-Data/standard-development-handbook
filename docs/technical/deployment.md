# Deployment

[Read the Docs](https://readthedocs.org/) is used to build and deploy the documentation.

[Automation rules](https://docs.readthedocs.io/en/stable/automation-rules.html) are used to:

* Build a version of the documentation for each branch in the standard repository
* Delete a version when a branch is deleted

Each time you push a commit to a branch, Read the Docs builds the version for that branch. In addition to the versions for each branch, there is also:

* A `latest` version that redirects to the `0.1` branch.
* A `stable` version that redirects to the most recent release.
* A version for each [tag](https://github.com/Open-Telecoms-Data/open-fibre-data-standard/tags) in the standard repository, e.g. `0__1__0__beta`.

Only the `latest` version is available in the [flyout menu](https://docs.readthedocs.io/en/stable/glossary.html#term-flyout-menu). To view other versions, substitute the branch name into the URL, as follows:

```
https://open-fibre-data-standard.readthedocs.io/en/branch-name/
```

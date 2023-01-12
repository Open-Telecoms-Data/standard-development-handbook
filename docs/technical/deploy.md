# Deploying the documentation

This section describes the steps involved in deploying an updated version of the standard to become the live version.

This process is used for major, minor and patch versions, as well as non-normative versions.

For changes to the documentation only (no schema changes), start from [merge and release](#merge-and-release).

## Prepare for release

You can skip this section if you are not releasing a new major, minor or patch version.

### Update version numbers and changelog

Update the MAJOR.MINOR version number in the following files:

* `docs/`
  * `conf.py`: update `version`
  * `*.md` `*/*.md`: update release admonition

Update the MAJOR_MINOR_PATCH version number in the following files:

* `docs/`
  * `conf.py`: update `release`
  * `reference/schema.md`: update canonical schema URL
* `schema/`
  * `network-schema.json`: update `id`
  * `network-package-schema.json`: update `id` and `properties/networks/items/$ref`
* `examples/`
  * `json/*.json`: update `links/0/href` for each network
  * `geojson/*.geojson`: update `/properties/network/links/0/href` for each feature
  * `csv/links.csv`: update `links/0/href`

Update the version number and date in the changelog.

### Set up a development instance of CoVE

Set up a development instance of CoVE using the new schema, and run tests against it.

## Merge and release

### Merge the development branch onto the live branch

Create a pull request to merge the development branch into its corresponding live branch, e.g. `0.2-dev` into `0.2`. This might happen by first merging a patch dev branch (`0.2.1-dev`) into the minor dev branch (`0.2-dev`), and then merging into the live branch (`0.2`). The pull request can be created through GitHub's web interface.

### Create a tagged release

You can skip this step if you are not releasing a new major, minor or patch version.

1. Create a tag. For example:

```bash
  git tag -a 0__2__0 -m '0.2.0 release'
```

2. Push the tag:

```bash
  git push --follow-tags
```

### Update readthedocs

You can skip this step if you are not releasing a new major or minor version.

When releasinog a new major or minor version, the new release should be set to the 'latest' version, and all previous versions should be visible in the flyout menu on readthedocs. To do this:

- Log in to readthedocs and open the `open-fibre-data-standard` project. You will need ODSC admin credentials to do this.
- To set the new release to be the 'latest' branch, to 'Admin' ? 'Advanced settings and change the option.
- To change the visibility of the new and previous versions in the flyout, go to 'Versions' and 'Edit' on relevant versions. The 'Hidden' checkbox will determine if a version is visible in the flyout menu.
## Complete the deployment

### Update CoVE

You can skip this step if you are not releasing a new major, minor or patch version.

#### Update Lib CoVE OFDS

- Update the URL paths in [schema.py](https://github.com/Open-Telecoms-Data/lib-cove-ofds/blob/main/libcoveofds/schema.py)
- Make sure all tests pass
- Release a new version

#### Update and deploy CoVE OFDS

- Upgrade the requirements to use the new version of the CoVE library

```bash
  pip-compile -P libcoveofds; pip-compile requirements_dev.in
```
- Update the URL paths in [settings.py](https://github.com/Open-Telecoms-Data/cove-ofds/blob/live/cove_project/settings.py)
- Make sure all tests pass
- Deploy the app

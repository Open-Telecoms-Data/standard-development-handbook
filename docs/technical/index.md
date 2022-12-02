# Technical processes

```{toctree}
:maxdepth: 2
:glob:

build
update
```

## Running tests locally

When you push code to GitHub, tests are automatically run. You will not be allowed to merge your pull request until these tests pass.

To run these locally, first create and install a virtual environment [as instructed on the build page](build).

To run tests locally, run:

```
pytest tests/
```

To build the project while checking for warnings, run:

```
sphinx-build -nW ./docs/ ./temp_build
```

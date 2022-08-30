# Building the documentation

## Get started
  
Create a virtual environment:

```
sudo apt-get install python3-venv
python3 -m venv .ve    
source .ve/bin/activate
```

Install submodules

```
git submodule init
git submodule update
```


Install requirements

```
pip install --upgrade pip setuptools
pip install -r docs/requirements.txt
```

## Build the documentation

Build the docs into `docs/_build/dirhtml`:

```
cd docs
make dirhtml
```


Sphinx, which builds the documentation, doesn’t watch directories for changes. To regenerate the documentation and refresh the browser whenever changes are made, run:

```
make autobuild
```

Otherwise, view the documentation by running a local web server:

```
cd _build/dirhtml
python -m http.server
```

Then go to http://localhost:8000/ in a browser.

## View development documentation on readthedocs

Versions are managed via [readthedocs](https://readthedocs.org/) admin (credentials needed). Automation rules have been set up to automatically build and activate hidden versions when new branches are created. Therefore development branches will not be available from the flyout menu on readthedocs, but can be viewed by inserting the branch name into the url, as follows:

```
https://open-fibre-data-standard.readthedocs.io/en/my-development-branch-name/

```
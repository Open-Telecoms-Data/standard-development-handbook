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
pip install -r requirements.txt
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

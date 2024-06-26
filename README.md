# OFDS Development Handbook

A guide for authors of the Open Fibre Data Standard.

## Build and view the documentation

Create and activate a virtual environment, then install requirements:

```
pip install -r docs/requirements.txt
```

And build the documentation:

```
cd docs
make html
```

The built documentation is in `_build/html` under `docs`. To view the documentation:

```
python -m http.server --directory _build/html
```

And open <http://localhost:8000/> in a browser.

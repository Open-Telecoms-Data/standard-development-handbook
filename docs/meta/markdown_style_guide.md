# Markdown style guide

This style guide covers our conventions when writing Markdown files in Sphinx documentation.

This page shows relevant directives from [Sphinx](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html) and [reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/directives.html). It also shows some examples of [reStructuredText](https://docutils.sourceforge.io/docs/user/rst/quickref.html).

We use the following directives from [sphinxcontrib-opendataservices](https://sphinxcontrib-opendataservices.readthedocs.io/en/latest/) and [sphinxcontrib-opendataservices-jsonschema](https://sphinxcontrib-opendataservices-jsonschema.readthedocs.io/en/latest/):

-  [csv-table-no-translate](https://sphinxcontrib-opendataservices.readthedocs.io/en/latest/misc/#directive-csv-table-no-translate)
-  [jsoninclude](https://sphinxcontrib-opendataservices.readthedocs.io/en/latest/jsoninclude/#directive-jsoninclude)
-  [jsoninclude-flat](https://sphinxcontrib-opendataservices.readthedocs.io/en/latest/jsoninclude/#directive-jsoninclude-flat)
-  [jsonschema](https://sphinxcontrib-opendataservices-jsonschema.readthedocs.io/en/latest/use.html)

## Layout

### sidebar

```{sidebar} Title

   A [nicer demo](https://jupyterbook.org/content/layout.html#sidebars-within-content)
```

````md
```{sidebar} Title

   A nicer demo
```
````

### transition

before

---

after

```md
   before

   ---

   after
```

## Admonitions

```{note}
   A note
```

````md
```{note}
   A note
```
````

In addition to `note`, there are ([see demo](https://pydata-sphinx-theme.readthedocs.io/en/latest/demo/demo.html#admonitions)):

```{hlist}
   :columns: 3

   -  note
   -  hint
   -  tip
   -  important
   -  attention
   -  caution
   -  warning
   -  danger
   -  error
```

```{admonition} Custom title
   Content
```

````md
```{admonition} Custom title
   Content
```
````

## References

See the documentation on [Markdown footnotes](https://jupyterbook.org/content/content-blocks.html#footnotes) (or [reStructuredText footnotes](https://docutils.sourceforge.io/docs/user/rst/quickref.html#footnotes)).

### seealso

```{seealso}

   Reference: [Schema](reference/schema.md)
      The schema provides a detailed specification of the fields, data structures and rules for publishing OFDS data.
```

````md
```{seealso}

   Reference: [Schema](reference/schema.md)
      The schema provides a detailed specification of the fields, data structures and rules for publishing OFDS data.
```
````

### glossary

```{glossary}
a term
  its definition

another term
a synonym
  its definition
```

{term}`a term` reference.

````md
```{glossary}
a term
  its definition

another term
a synonym
  its definition
```

{term}`a term` reference.
````

## Code blocks

### code-block

```json
{
   "some": "text",
   "key": "value"
}
```

````
```json
{
   "some": "text",
   "key": "value"
}
```
````

```{code-block} json
:linenos:
:lineno-start: 2
:emphasize-lines: 1-2,4
:caption: A caption
:name: label-to-reference

{
  "some": "text",
  "key": "value"
}
```

````md
```{code-block} json
:linenos:
:lineno-start: 2
:emphasize-lines: 1-2,4
:caption: A caption
:name: label-to-reference

{
  "some": "text",
  "key": "value"
}
```
````

### literalinclude

````md
```{literalinclude} filename.ext
:language: json
```
````

The path can be relative to the file, or relative to the top source directory if starting with `/`.

It accepts the same options as `code-block`. It also accepts:

`:lines: 1-2,4`
: Show specific lines only

`:start-after: text to match`
: Show lines after the first matching line

`:end-before: text to match`
: Show lines before the first matching line

`:start-at: text to match`
: Show lines as of the first matching line

`:end-at: text to match`
: Show lines up to the first matching line

`:lineno-match:`
: Show the original line numbers

`:prepend:`
: Prepend a line

`:append:`
: Append a line

## Lists

### Definition list

who
: what

where
: when

```md
who
: what

where
: when
```

### Field list

:who:
  what
:where: when

```md

   :who:
      what
   :where: when
```
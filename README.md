The OpenReview documentation
============================

The documentation uses [Sphinx](https://www.sphinx-doc.org/en/master/index.html). In order to compile the source files, the sphinx python module is required:
```bash
$ pip install sphinx
```
You are now ready to compile the documentation. Change the directory to the `docs` directory where the source files and the build are located:
```bash
$ cd docs
```

Every time that the source files are modified the build needs to be run:
```bash
$ make html
```

The result of the build is an html whose index is inside `docs/_build/html/index.html`. If you are inside de `docs` folder:
```bash
$ open _build/html/index.html
```

# Python Practice

Practice Python.

## Install

Create Python 3 version environment and activate it using `virtualenv`.

```bash
virtualenv venv --python=python3
. venv/bin/activate
```

Then, install libraries to setup environment.

```bash
pip install -r django/tutorial/requirements.txt
```

If you want to store installed packages in requirements format, execute
following command.

```bash
pip freeze > requirements.txt
```

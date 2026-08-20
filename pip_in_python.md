# PIP in Python

PIP is the package manager used in Python. It helps us install, remove, update, and manage Python packages.



## Common PIP Commands

* `pip install package` - installs a package.
* `pip uninstall package` - removes a package.
* `pip install --upgrade package` - updates a package.
* `pip install package==version` - installs a specific version.
* `pip list` - shows installed packages.
* `pip show package` - shows package details.
* `pip freeze` - shows installed packages with versions.
* `pip check` - checks for dependency problems.
* `pip install -r requirements.txt` - installs packages from a requirements file.
* `pip freeze > requirements.txt` - saves installed packages and versions.
* `pip install --upgrade pip` - updates PIP itself.

## Virtual Environment

`venv` creates a separate environment for a project. This keeps one project's packages separate from other projects.

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

Then install packages:

```bash
python -m pip install requests
```

`python -m pip` makes sure PIP is run using the selected Python installation.

## Example

```bash
pip install requests
pip show requests
pip list
pip freeze > requirements.txt
pip install -r requirements.txt
pip check
```



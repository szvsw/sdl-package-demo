# An Example Python Package

This repo is a simple demonstration of how to create and publish a Python package.

## Create the Repo

1. Create a repository on GitHub
1. For your `.gitignore`, choose the Python option.
1. Clone into your repo

## Directory setup

Setup the following directory structure inside of your repository:

```
my-package/
├─ my_package/
│  ├─ __init__.py
│  ├─ module1.py
│  ├─ module2.py
│  ├─ sub_package/
│  │  ├─ __init__.py
│  │  ├─ module3.py
│  │  ├─ module4.py
├─ tests/
│  ├─ test_module1.py
│  ├─ test_module2.py
│  ├─ test_module3.py
│  ├─ test_module4.py
├─ LICENSE
├─ README.md
├─ .gitignore
├─ pyproject.toml
```

## Populate pyproject.toml

Your `pyproject.toml` is a configuration file for packaging and building a Python package that is ready for distribution. If you are familiar with `setup.py`, you can think of it as a cleaner, more readable way of automating the configuration normally done there.

It is a table of key-value pairs, with sections defined by brackets, e.g. `[section_name]`. Some sections may have keys which may need their own tables, e.g. `[section_name.sub_section]`.

Let's build it!

### The Build System section

The first `[build-system]` section defines which packaging backend you will use. Stick to the defaults for now!

```
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

### The Project Section

Next, the `[project section]` is where you enter the main project metadata, like the package name, the version, etc. Start with the following keys:

```
[project]
name = "your-package-name"
description = "An example package which does something"
version = "0.0.1"
```

Next let's add a few optional keys to the project section which identify some important files and additional metadata. Note that the value stored at a key can itself be a table (as in the `license` key), an array (as in the `classifiers` key), or even an array of tables (as in the `authors` key).

```
readme = "README.md"
license = {file = "LICENSE"}
authors = [
    {name = "Your Name", email = "you@email.com"},
    {name = "Another Name", email = "them@email.com"}
]
classifiers = [
    "Development Status :: 4 - Beta",
    "Intended Audience :: Science/Research",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3.7",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
]
```

Next, we will specify which dependencies your package needs. This example assumes that the package requires Python v3.7 or higher, as well as the `requests` package.

```
requires-python = ">=3.7"
dependencies = [
    "requests"
]
```

_nb: if you already have a `requirements.txt` file, follow the instructions [here](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#dynamic-metadata) for dynamically injecting your requirements into the `dependencies` key._

Finally, let's add some additional metadata by creating a subsection for the `urls` key:

```
[project.urls]
Documentation = "https://somesite.com"
Repository = "https://github.com/username/repository"
```

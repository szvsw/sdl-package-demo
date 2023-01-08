# An Example Python Package

This repo is a simple demonstration of how to create and publish a Python package to PyPI (the Python Package Index, pronounced "pie-pee-eye", but I prefer `pie-pie`, because who doesn't like pie).

## PyPI Account Setup

Before getting started, please create an account at both `test.pypi.org` AND `pypi.org`.

Next, create a file called `.pypirc`. Add the following to it:

```
[pypi]
  username = __token__
  password = <the full pypi token string including the pypi- prefix>

[testpypi]
  username = __token__
  password = <the full testpypi token string including the pypi- prefix>
```

We will replace the passwords once we have generated each of the tokens. Save this file in your home directory, e.g. `C:\Users\YourName` for Windows. This will allow `twine` (the PyPI publishing tool) to automatically log you in whenever you upload a package.

After you have created and verified your account(s), navigate to the `Account Settings` page and find the `API Tokens` section. Click `Add API Token`, and then enter a name (e.g. `pypi-global-api-token`) and select `Entire Account` for the scope. Your token should now appear. Edit your `.pypirc` file in the appropriate location.

## Create the Repo for your Package

If you do not yet have a repo for your package, create one!

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

Your package files should be copied into the `my_package` subfolder (feel free to rename). For now, you can leave the `tests` folder empty. Your `.gitignore` should probably follow a standard Python [.gitignore] (https://github.com/github/gitignore/blob/main/Python.gitignore). Add your license text (e.g. [MIT License](https://opensource.org/licenses/MIT)) to the license file.

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

There's a lot more advanced configuration we can do, plugins that can be installed, etc etc, but that should be enough for now!

## Build and Publish the Package

Install the `pip` packages `build` (for creating the disribution files) and `twine` (for publishing the package to PyPI):

```
pip install build twine
```

From the root directory of your project, run the following command to build your dist and wheel files!

```
python -m build
```

When `build` is complete, you should now see a `dist` file in your package directory.

That was easy! Next, let's publish it. We will start by publishing to `test.pypi.org` in order to make sure everything is working.

```
python -m twine upload --repository testpypi dist/*
```

When it completes, head over to `test.pypi.org` and check it out! It's now ready to be installed (though the `testpypi` index is pruned regularly). You can install it via `pip` as you would any other package, but you will need to specify that it is coming from `testpypi` with the `--index-url` flag.

```
pip install --index-url https://test.pypi.org/simple/ your-package-name
```

Finally, let's publish it to the real Python PI:

```
twine upload dist/*
```

## Additional Resources

- [Setuptools User Guide](https://setuptools.pypa.io/en/latest/userguide/index.html)
- [More in-depth instructions on package creation and publication](https://www.freecodecamp.org/news/how-to-create-and-upload-your-first-python-package-to-pypi/)
- [And some more...](https://realpython.com/pypi-publish-python-package/#create-a-small-python-package)
- [Some info on .pypirc](https://packaging.python.org/en/latest/specifications/pypirc/)

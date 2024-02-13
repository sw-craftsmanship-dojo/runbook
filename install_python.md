# Install Python

> This document describes how to install Python on macOS.  We will make use of pyenv to ensure that it does not interfere with their existing version(s) of Python that may be installed.

## Prerequisite

> Homebrew is a prerequisite on macOS for setting up pyenv.  It is a package manager that can be used to install many different packages.  Here we'll guide participants how to set it up.

To install Homebrew - the package manager, the simplest way to do it is to open your terminal and run:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

For other installation options, you can follow [the Homebrew installation guide](https://docs.brew.sh/Installation)

## Install Pyenv

> Pyenv is a simple Python version management system.  It allows you to quickly switch between different versions of Python.

Now that you have homebrew installed, you can open your terminal and run:

```sh
brew install pyenv
```

This will install pyenv and all of its dependencies.

## Initializing Pyenv

Before being able to use pyenv you will need to initialize it.  To do this, run:

```sh
pyenv init
```

This will give you some code to add to the end of your `~/.zprofile`, `~/.zshrc` or other shell files for the particular shell you are using.  The instructions will tell you **exactly** what to update.

## Installing a specific version of Python

After you installed pyenv, you can install a specific version of Python.  The recommendation in the dojo is to install the latest version.  To see the latest version go to the [Supported Python Version](https://devguide.python.org/versions/#supported-versions).  Try going for the highest version that is in bugfix status.

Now that you've chosen your version, you can install it by running: (In this guide I'll use 3.12)

```sh
pyenv install 3.12
```

pyenv will take care of installing pyenv.  Once completed you will want to tell pyenv which version of Python you wish to use.  Pyenv has multiple ways of setting this version - [Understanding Python version selection](https://github.com/pyenv/pyenv?tab=readme-ov-file#understanding-python-version-selection)

- `pyenv shell 3.12` would set it for the current shell only
- `pyenv local 3.12` would update the `.python-version` file in the current directory, so that any time you run `pyenv local` in the future, it would use the version from `.python-version`
- `pyenv global 3.12` would set it globally and update the `$(pyenv root)/version` so that even when you retart your laptop, it would be used.

## Installing pipenv

> pipenv is a virtualenv management tool.  This allows you to have a virtualenv where all of the packages that you're using within a project are stored and they cannot interfere with another project that you may be working on. 

Now that you've chosen the version of python to use with the previous command, you need to install pipenv for that version.  (Every time that you install a new version of python you will need to re-execute this command)

```sh
pip install pipenv
```

Once you've installed pipenv you can test that it works by running:

```sh
cd /tmp
pipenv install pytest --dev
```

This should successfully create a virtualenv for your /tmp directory, create a Pipfile and Pipfile.lock as well as install pytest as a development package.

---
description: Overview of the Poetry Dependency Management and Packaging Tool
---

# Poetry

When it comes to managing virtual environments in Python, `poetry` can be used as an alternative to `pip`. I prefer `poetry` over `pip` because I think it's easier to handle virtual environments and dependencies in `poetry`.&#x20;

I like how `poetry` allows for the grouping of dependencies in the `pyproject.toml`, which is something I've always liked about `package.json` files in `node.js` projects. One example of this is separating project dependencies from dev dependencies.

Note that the instructions below are for MacOS, but the core Poetry commands are consistent across MacOS, Linux, and Windows. Also, the setup below is based on my personal preference for configuring `poetry`.

:desktop: [Official Poetry Docs](https://python-poetry.org/)

## **Initial Setup**

1. Go to `~/Applications/Python` in a new Finder window and click `Install Certificates.command`.
2. Open up a terminal in the root directory `~` and run the following curl command to install poetry.

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

3. Go to your bash profile in  `~/.bash_profile`, add the following line, and save the file.

```bash
export PATH="/Users/<INSERT_USER_NAME>/.local/bin:${PATH}"
```

4. Restart your terminal (or run `source ~/.bash_profile`) and run `poetry --version` to make sure `poetry` was installed and can be properly mapped.&#x20;
5. Run `poetry config --list` in the terminal to see a list of `poetry` settings.
6. To optionally store virtual environments in project directories, you can modify the setting: `virtualenvs.in-project`. This is set to `null` by default, but we  can change it to `true` with the command below.

```bash
poetry config virtualenvs.in-project true
```

7. Run `poetry config --list` to make sure the `virtualenvs.in-project` setting was updated from `null` to `true`.

## Project Setup

1. To start  new project, run

```bash
poetry new <PROJECT_NAME>
```

2. If adding poetry to an existing project, `cd` into the project’s root directory and run&#x20;

```bash
poetry init
```

3. To install a package, run

```bash
poetry add <PACKAGE_NAME>
```

Poetry will automatically create a virtual environment, but it won't activate it automatically

4. To run scripts using the virtual environment created by poetry, we need to prepend commands with `poetry run`.

```bash
poetry run <COMMAND>

## Some Examples ##
poetry run python script.py
poetry run isort --check app/ tests/
```

5. If you want to run commands within a poetry virtual environment without prepending commands with `poetry run`, the virtual environment must be activated in the terminal with `poetry shell`, or you can create an alias for the `poetry run`.
6. To see a list of installed dependencies, run `poetry show` or `poetry show --tree`.
7. To uninstall a package, run&#x20;

```bash
poetry remove <PACKAGE_NAME>
```

8. Run `poetry install` to sync your virtual environment with the dependencies in `pyproject.toml`.

## Summary

For every directory that must have its own virtual environment, a `pyproject.toml` should be created within that directory. A `.venv` directory is created if `virtualenvs.in-project` is set to true. A `poetry.lock` file is created upon package installation in the same directory as the `pyproject.toml` file. To create the virtual environment, installing a package will automatically create one (if one does not exist).

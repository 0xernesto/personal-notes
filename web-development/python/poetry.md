---
description: Python Dependency Management and Packaging Tool
---

# Poetry

When it comes to managing virtual environments in Python, `poetry` can be used as an alternative to `pip`. I prefer `poetry` over `pip` because I think it's easier to handle virtual environments and dependencies in `poetry`.&#x20;

I like how `poetry` allows for the grouping of dependencies in the `pyproject.toml`, which is something I've always liked about `package.json` files in `node.js` projects. One example of this is separating project dependencies from dev dependencies.

Note that the instructions below are for MacOS, but the core Poetry commands are consistent across MacOS, Linux, and Windows. Also, the setup below is based on my personal preference for configuring `poetry`.

:desktop: [Official Poetry Docs](https://python-poetry.org/)

## <mark style="color:purple;">**Initial Setup:**</mark>

1. Go to `~/Applications/Python` in a new Finder window and click `Install Certificates.command`.
2. Open up a terminal in the root directory `~` and run the following curl command to install poetry.

```markup
curl -sSL https://install.python-poetry.org | python3 -
```

3. Go to your bash profile in  `~/.bash_profile`, add the following line, and save the file.

```markup
export PATH="/Users/<INSERT_USER_NAME>/.local/bin:${PATH}"
```

4. Restart your terminal (or run `source ~/.bash_profile`) and run <mark style="color:yellow;">`poetry --version`</mark> to make sure `poetry` was installed and can be properly mapped.&#x20;
5. Run <mark style="color:yellow;">`poetry config --list`</mark> in the terminal to see a list of `poetry` settings.
6. To optionally store virtual environments in project directories, you can modify the setting: `virtualenvs.in-project`. This is set to `null` by default, but we  can change it to `true` with the command below.

```markup
poetry config virtualenvs.in-project true
```

7. Run <mark style="color:yellow;">`poetry config --list`</mark> to make sure the `virtualenvs.in-project` setting was updated from `null` to `true`.

## <mark style="color:purple;">Project Setup:</mark>

1. To start  new project, run

```markup
poetry new <PROJECT_NAME>
```

2. If adding poetry to an existing project, `cd` into the project’s root directory and run&#x20;

```markup
poetry init
```

3. To install a package, run

```markup
poetry add <PACKAGE_NAME>
```

Poetry will automatically create a virtual environment, but it won't activate it automatically

4. To run scripts using the virtual environment created by poetry, we need to prepend commands with `poetry run`.

```markup
poetry run <COMMAND>

## Some Examples ##
poetry run python script.py
poetry run isort --check app/ tests/
```

5. If you want to run commands within a poetry virtual environment without prepending commands with `poetry run`, the virtual environment must be activated in the terminal with <mark style="color:yellow;">`poetry shell`</mark>, or you can create an alias for the `poetry run`.
6. To see a list of installed dependencies, run <mark style="color:yellow;">`poetry show`</mark> or <mark style="color:yellow;">`poetry show --tree`</mark>.
7. To uninstall a package, run&#x20;

```markup
poetry remove <PACKAGE_NAME>
```

8. Run <mark style="color:yellow;">`poetry install`</mark> to sync your virtual environment with the dependencies in `pyproject.toml`.

## <mark style="color:purple;">Summary</mark>

For every directory that must have its own virtual environment, a `pyproject.toml` should be created within that directory. A `.venv` directory is created if `virtualenvs.in-project` is set to true. A `poetry.lock` file is created upon package installation in the same directory as the `pyproject.toml` file. To create the virtual environment, installing a package will automatically create one (if one does not exist).

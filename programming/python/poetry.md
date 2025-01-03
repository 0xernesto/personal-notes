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
poetry new <project_name>
```

2. If adding poetry to an existing project, `cd` into the project’s root directory and run&#x20;

```bash
poetry init
```

3. To activate the virtual environment, run

```bash
poetry shell
```

## Workflow

Once the project is set up with Poetry, here are the common commands one must do on a regular basis:

Run `poetry shell` to activate the virtual environment.

Run `poetry install` to sync your virtual environment with the latest dependencies.

Run `poetry run <script_name>` to run a script defined in `pyproject.toml`.

Run `poetry add <package_name>` to install a package.

Run `poetry remove <package_name>` to uninstall a package.

Run `poetry show` or `poetry show --tree` to see al ist of installed dependencies.

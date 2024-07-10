---
description: Overview of the Standard Python Package Manager, PIP.
---

# PIP

## Introduction

The standard Python package manager is `pip`. It is ideal for quickly starting Python projects, like simple scripts or small projects with few dependencies. While capable of handling larger projects, tools like `poetry` are ideal for larger projects with complex dependencies.

The following notes assume:

* Python 3
* MacOS
* An alias in `bash` profile to map `python` to `python3`.

## Setup

The following is a step-by-step process for setting up a virtual environment in `my-project` and installing dependencies in it with `pip`.

1. `cd` into your project directory.

```bash
cd my-project
```

2. Create the virtual environment named `env`.

```bash
python -m venv env
```

3. Activate the virtual environment.

<pre class="language-bash"><code class="lang-bash"><strong>source env/bin/activate
</strong></code></pre>

4. Install dependencies.

```bash
pip install <PACKAGE_NAME>
```

## Commands

The following is a list of common commands used when managing a project with `pip`.

#### Activate Virtual Environment

```bash
source env/bin/activate
```

#### Deactivate Virtual Environment

```bash
deactivate
```

#### Uninstall a Package

```bash
pip uinstall <PACKAGE_NAME>
```

#### List Installed Packages

```bash
pip freeze
```

#### Create a requirements.txt File

```bash
pip freeze > requirements.txt
```

#### Install Packages from a requirements.txt File

```bash
pip install -r requirements.txt
```

## Pitfalls & Solutions

The following are common pitfalls when working with `pip` and their solutions.

<details>

<summary>I installed packages without activating my virtual environment.</summary>

To uninstall packages from your machine using the requirements.txt file in your project, make sure your virtual environment is **not** activated and run the following command:

```bash
pip uninstall -r requirements.txt
```

To uninstall all global Python packages from your machine, make sure your virtual environment is **not** activated and run the following command:

```bash
pip freeze | xargs pip uninstall -y
```

</details>

<details>

<summary>How do I pin the versions of my project's dependencies?</summary>

To prevent dependency conflicts, it is recommended to create a `requirements.txt` file with pinned package versions. The following command is a way to automatically create a `requirements.txt` file from the packages installed in your virtual environment:

```bash
pip freeze > requirements.txt
```

</details>

<details>

<summary>Can I disable pip when there is no virtual environment activated?</summary>

Yes. To require a virtual environment to be active when running `pip` commands, a config file can be used.

Run the following command to set `require-virtualenv` to `True`.

```bash
pip config set global.require-virtualenv True
```

Now, the contents of `~/.config/pip/pip.conf` should be:

```
[global]
require-virtualenv = True
```

If you ever need to get around this requirement, you can either temporarily set `require-virtualenv` to `False`, or prepend your `pip` commands as follows:

```bash
PIP_REQUIRE_VIRTUALENV=false pip install <PACKAGE_NAME>
```



</details>

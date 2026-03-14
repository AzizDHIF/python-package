# 📦 Python Package Creation Tutorial

> A hands-on guide from the **Hi! PARIS Summer School 2024** — learn how to build, test, document, and publish your own Python package from scratch.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Demo 1 — Build your package locally](#demo-1--build-your-package-locally)
- [Demo 2 — Test your package with pytest](#demo-2--test-your-package-with-pytest)
- [Demo 3 — Git & GitHub](#demo-3--git--github)
- [Demo 4 — Code quality with Ruff & pre-commit](#demo-4--code-quality-with-ruff--pre-commit)
- [Demo 5 — Documentation with Sphinx](#demo-5--documentation-with-sphinx)
- [Demo 6 — GitHub Actions (CI/CD)](#demo-6--github-actions-cicd)
- [Demo 7 — Publish to PyPI](#demo-7--publish-to-pypi)
- [Resources](#resources)

---

## Overview

This project was created for the **Hi! PARIS Summer School 2024** practical session hosted by the Hi! PARIS Engineering Team. It walks you through every step of the Python packaging lifecycle:

- 🏗️ Creating and building a package with **Poetry**
- 🧪 Writing and running tests with **pytest**
- 🔍 Linting and formatting with **Ruff**
- 📖 Auto-generating documentation with **Sphinx**
- ⚙️ Automating workflows with **GitHub Actions**
- 🚀 Publishing to **PyPI**

> ⚠️ The live demos were made on **macOS** with **VSCode**. Some steps may require adaptation on Windows.

---

## Prerequisites

- Python 3.12+
- [pipx](https://pipx.pypa.io/stable/installation/)
- [Poetry](https://python-poetry.org/docs/)
- Git

---

## Project Structure

```
package_creation_tutorial/
├── README.md
├── pyproject.toml
├── main.py
├── src/
│   └── package_creation_tutorial/
│       ├── __init__.py
│       └── string_ops.py
├── tests/
│   ├── __init__.py
│   └── test_string_ops.py
├── docs/
│   ├── conf.py
│   ├── index.rst
│   └── modules.rst
└── .github/
    └── workflows/
        ├── tests.yaml
        └── sphinx_doc.yaml
```

---

## Demo 1 — Build your package locally

### 1. Install Poetry

```bash
pipx install poetry
```

### 2. Create a new project

```bash
poetry new --src package_creation_tutorial
```

### 3. Activate the virtual environment

```bash
poetry shell
```

### 4. Install dependencies

```bash
# Install all dependencies from pyproject.toml
poetry install

# Add a new dependency
poetry add <package_name>
```

### 5. Build the package

```bash
poetry build
```

### 6. Run the main script

```bash
python main.py
```

**Expected output:**

```
Original string: hello world
Reversed: dlrow olleh
Vowel count: 3
Capitalized: Hello World
```

---

## Demo 2 — Test your package with pytest

### 1. Install test dependencies

```bash
poetry add pytest pytest-cov
```

### 2. Run tests

```bash
poetry run pytest
```

### 3. Run tests with code coverage

```bash
poetry run pytest --cov src/
```

---

## Demo 3 — Git & GitHub

### Initialize a local repository

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin <your_repository_url>
git remote -v
```

### Push changes

```bash
git status
git add <file>
git commit -m "your commit message"
git push
```

### Working with branches

```bash
# Create a new branch
git branch <branch_name>

# Switch to the branch
git checkout <branch_name>
```

---

## Demo 4 — Code quality with Ruff & pre-commit

### 1. Install Ruff

```bash
poetry add ruff
```

### 2. Run Ruff (lint + auto-fix)

```bash
ruff check --fix .
```

### 3. Install pre-commit

```bash
poetry add pre-commit
```

### 4. Configure `.pre-commit-config.yaml`

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.0.292
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
```

### 5. Install the pre-commit hook

```bash
pre-commit install
```

Every `git commit` will now automatically lint and format your code with Ruff before allowing the commit.

---

## Demo 5 — Documentation with Sphinx

### 1. Install Sphinx

```bash
poetry add sphinx sphinx_rtd_theme
```

### 2. Initialize the `docs/` folder

```bash
sphinx-quickstart docs
```

### 3. Build the documentation

```bash
cd docs/
make html
```

### 4. View the documentation locally

Open `docs/build/html/index.html` in your browser.

> Sphinx uses your **docstrings** and **type hints** to auto-generate the API reference. Make sure your functions are properly documented!

---

## Demo 6 — GitHub Actions (CI/CD)

GitHub Actions are triggered on every `push` or `pull_request` to the `master` branch.

### Automated unit tests — `.github/workflows/tests.yaml`

```yaml
name: unit-tests

on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.12.4'
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -e ".[dev]"
      - name: Install Pytest
        run: pip install pytest
      - name: Run tests
        run: pytest
```

### Automated documentation deployment — `.github/workflows/sphinx_doc.yaml`

```yaml
name: documentation

on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12.4'
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install ghp-import sphinx==7.3.7 sphinx_rtd_theme
      - name: Build HTML
        run: |
          cd docs/
          make html
      - name: Deploy to GitHub Pages
        run: ghp-import -n -p -f docs/build/html
```

### Enable GitHub Pages

1. Go to **Settings → Actions → General** and enable **Read and write permissions**
2. Push the workflow files to `master`
3. Go to **Settings → Pages**, select the `gh-pages` branch, and click **Save**

Your documentation will be live at: `https://<your_github_name>.github.io/<repository_name>/`

---

## Demo 7 — Publish to PyPI

### 1. Create a PyPI account

Register at [https://pypi.org](https://pypi.org)

### 2. Generate an API token

Go to your PyPI account settings and create a token with upload permissions.

### 3. Configure Poetry with your token

```bash
poetry config pypi-token.pypi <your-api-token>
```

### 4. Build and publish

```bash
poetry build
poetry publish
```

Your package will be available at: `https://pypi.org/project/<your-package-name>/`

---

## Resources

| Resource | Link |
|---|---|
| 📦 Full demo repository | [github.com/brash6/package_creation_tutorial](https://github.com/brash6/package_creation_tutorial) |
| 🌐 Hi! PARIS Engineering Team | [engineeringteam.hi-paris.fr](https://engineeringteam.hi-paris.fr/) |
| 🐙 Hi! PARIS GitHub | [github.com/hi-paris](https://github.com/hi-paris) |
| 📖 Poetry documentation | [python-poetry.org/docs](https://python-poetry.org/docs/) |
| 🧪 Pytest documentation | [docs.pytest.org](https://docs.pytest.org/en/7.3.x/) |
| 📊 Pytest-cov documentation | [pytest-cov.readthedocs.io](https://pytest-cov.readthedocs.io/en/latest/) |
| ⚙️ GitHub Actions documentation | [docs.github.com/en/actions](https://docs.github.com/en/actions) |
| 📦 PyPI | [pypi.org](https://pypi.org/) |

---

*Made with ❤️ by the Hi! PARIS Engineering Team — Summer School 2024*

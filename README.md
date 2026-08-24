<!--
Copyright 2026 EOAP

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

# EOAP Python Project Template

This repository is a reusable **Copier project archetype** for bootstrapping EOAP-style Python packages.

It is intended to be deployed as a dedicated GitHub repository, for example:

```text
EOAP/python-project-template
```

Generated projects include:

- a Hatch-based `pyproject.toml`;
- an Apache-2.0 `LICENSE` and `NOTICE`;
- Apache-2.0 source headers using each file type's native comment syntax;
- a top-level `CHANGELOG.md` following Keep a Changelog conventions;
- Diátaxis documentation under `docs/`;
- a top-level `mkdocs.yaml` wired directly to the generated documentation;
- a `Taskfile.yaml` configured for `EOAP/taskfile-utils` remote Taskfiles;
- a `.env` enabling remote Taskfiles with `TASK_X_REMOTE_TASKFILES=1`;
- GitHub Actions for quality checks, tests, package build, and documentation build;
- a `src/` package layout and initial tests.

## Repository layout

```text
.
├── README.md
├── copier.yaml
├── LICENSE
├── NOTICE
├── CHANGELOG.md
└── template/
    ├── README.md.jinja
    ├── pyproject.toml.jinja
    ├── mkdocs.yaml.jinja
    ├── Taskfile.yaml.jinja
    ├── CHANGELOG.md.jinja
    ├── LICENSE.jinja
    ├── NOTICE.jinja
    ├── docs/
    ├── src/
    ├── tests/
    └── .github/
```

The root of this repository documents and configures the archetype. The `template/` directory contains the files rendered into each generated Python project.

## Prerequisites

Install the tools used by the generated projects:

```bash
python -m pip install --user pipx
pipx ensurepath
pipx install copier
pipx install hatch
```

Install Task if you want to use the generated `Taskfile.yaml` locally. Follow your platform's preferred installation method for Task.

## Step 1 — Deploy this archetype repository

Create an empty GitHub repository, for example:

```text
https://github.com/EOAP/python-project-template
```

Then push this archetype into it:

```bash
git init
git add .
git commit -m "Initial Python project template"
git branch -M main
git remote add origin git@github.com:EOAP/python-project-template.git
git push -u origin main
```

## Step 2 — Tag a stable template release

Use tags so projects can be generated from a stable, repeatable archetype version:

```bash
git tag v0.X.0
git push origin v0.X.0
```

Prefer generating projects from a tag rather than directly from `main`.

## Step 3 — Generate a new Python project

From another directory, run:

```bash
copier copy --vcs-ref v0.5.0 https://github.com/eoap/python-project-template.git stripodi-test
```

Copier will ask for project metadata such as project name, package name, author, repository URL, and documentation URL.

You can also generate from a local checkout while developing the archetype:

```bash
copier copy /path/to/python-project-template my-new-project
```

## Step 4 — Initialize the generated project

Move into the generated repository:

```bash
cd my-new-project
```

Create the Hatch environment:

```bash
hatch env create
```

Run the test suite directly with Hatch:

```bash
task quality:test
```

Build the package:

```bash
hatch build
```

## Step 5 — Customize project content

After generation, update at least these files:

```text
README.md
CHANGELOG.md
docs/tutorials/first-steps.md
docs/how-to/install.md
docs/how-to/use-cli.md
docs/reference/api.md
docs/explanation/architecture.md
src/<python_package>/cli.py
tests/test_version.py
```

Keep the documentation organized according to Diátaxis:

- tutorials teach by walking users through a learning path;
- how-to guides solve specific tasks;
- reference pages describe APIs and technical facts;
- explanation pages clarify concepts, design choices, and background.

## Step 6 — Update generated projects when this template changes

Copier can update a generated project from a newer template version when the generated project keeps its `.copier-answers.yml` file:

```bash
copier update
```

For controlled updates, first tag a new archetype release:

```bash
git tag v0.X.0
git push origin v0.X.0
```

Then update the generated project using Copier's update workflow.

## Template variables

The main variables are defined in `copier.yaml`:

| Variable | Purpose |
| --- | --- |
| `project_name` | Human-readable project name. |
| `project_slug` | Repository/distribution name, usually kebab-case. |
| `python_package` | Importable Python package name, usually snake_case. |
| `project_description` | One-line package description. |
| `author_name` | Author or organization name. |
| `author_email` | Author contact email. |
| `repository_url` | Source repository URL. |
| `documentation_url` | Published documentation URL. |
| `copyright_year` | Year used in generated source headers. |
| `license_id` | SPDX license identifier; default is `Apache-2.0`. |

## Development workflow for this archetype

When editing the archetype:

1. Modify files under `template/`.
2. Generate a disposable project locally with `copier copy . /tmp/example-project`.
3. Run `task quality:check`, `task quality:test`, `task quality:lint` inside the generated project.
4. Commit the archetype changes.
5. Tag a new release when the archetype is ready for consumers.

## License

This archetype is licensed under the Apache License, Version 2.0. Generated projects also use Apache-2.0 by default.

# Code style

This repository has no formatter, linter or build, so nothing enforces style
automatically. These are the conventions the existing files follow; keep new
work consistent with them.

## Markdown

- English, wrapped at about 80 columns. Tables where a reader looks something
  up, prose where they need the reason.
- Fenced code blocks name their language (`bash`, `yaml`).
- Links within this repository are relative. Links to other agrivaro
  repositories are absolute `https://github.com/agrivaro/...` URLs; most of
  those repositories are private, so such a link works for members only.
- This repository is public: see [`AGENTS.md`](AGENTS.md#this-repository-is-public)
  for what may never be written here.
- A default file (`PULL_REQUEST_TEMPLATE.md`, `SECURITY.md`,
  `CONTRIBUTING.md`) is read inside other repositories. Write it so it holds in
  every one of them, and point to a repository's own documents instead of
  naming their contents.

## Workflow YAML

`.github/workflows/brand-guard.yml` sets the pattern:

- A comment header says what the workflow is for, which repository its values
  mirror, how to refresh them, and how to call it.
- Every `workflow_call` input has a `description`, a `type` and an explicit
  `required`; both of the guard's inputs are required.
- Inputs reach the script through `env:` and are read as shell variables.
  Never write `${{ inputs.* }}` inside a `run:` block: GitHub substitutes the
  text before the shell starts, so a caller's value would execute as code.
- `run:` blocks set `shell: bash` and start with `set -euo pipefail`.
- Failures are GitHub annotations (`echo "::error::..."`, or
  `::error file=<path>::` when a file is at fault). The script records each
  failure, keeps going, and exits once at the end, so one run reports every
  problem.
- A check that inspected nothing fails. The guard counts the SVG files it
  hashed and fails at zero.
- Third-party actions are referenced by major version tag, as
  `actions/checkout@v4` is today.

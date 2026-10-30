# Architecture

This repository holds no application. It is configuration that GitHub reads by
convention from a public repository named `.github`, plus one reusable
workflow. This file explains how each part takes effect; the
[README](README.md) says how to use them.

## How GitHub uses each file

**The organization profile.** GitHub renders `profile/README.md` of the public
`.github` repository on the organization's overview page.

**Default community health files.** `PULL_REQUEST_TEMPLATE.md`, `SECURITY.md`
and `CONTRIBUTING.md` are defaults. GitHub uses each one for every repository
owned by the organization that has no file of that type itself, whatever that
repository's visibility. A repository's own file always wins. GitHub looks for
defaults in the `.github` folder, the root and the `docs` folder of this
repository, in that order; this repository keeps them in the root. The
mechanism only works while this repository is public, and it has no default
for a licence. Source: GitHub's documentation, "Creating a default community
health file".

**The reusable workflow.** `.github/workflows/brand-guard.yml` declares
`on: workflow_call`, so it never runs on a push to this repository. A
repository calls it with
`uses: agrivaro/.github/.github/workflows/brand-guard.yml@main` and two inputs.
The job then runs in the caller's context: `actions/checkout` checks out the
caller's repository, and the script reads only the caller's files. It uses no
secret and writes nothing.

## The brand guard

```text
agrivaro/branding (private)          agrivaro/.github (public)
  hexcode.md  ──── copied by hand ──▶  colors="…" in brand-guard.yml
  logos/*.svg ──── sha256 by hand ───▶  hashes="…" in brand-guard.yml
                                               │
                                               ▼  workflow_call
                                     calling repository's checkout
                                       brand_files  must contain each hex
                                       asset_dirs   every *.svg must match a pin
```

The values are inline because a calling job's `GITHUB_TOKEN` is scoped to the
calling repository and cannot read the private `branding` repository at run
time. That makes the pins a mirror that has to be refreshed with every brand
change ([`AGENTS.md`](AGENTS.md#the-brand-guard-mirrors-agrivarobranding)).

What the guard decides:

- every file in `brand_files` exists, and each of the four canonical hex values
  appears in at least one of them, ignoring case;
- every directory in `asset_dirs` exists, and every `.svg` file under it hashes
  to one of the pins;
- finding no SVG at all is a failure, so a wrong path cannot pass by inspecting
  nothing;
- every problem is reported as an annotation before the job exits, rather than
  stopping at the first one.

What it cannot decide: raster files such as PNG app icons are not checked,
because a translation of a vector mark cannot be hash-compared, and derived
colours from `colors.md` are not checked at all.

## Codemap

| Path | What |
| --- | --- |
| `profile/` | The organization overview page |
| `PULL_REQUEST_TEMPLATE.md` | The default pull request template, with the brand checklist |
| `SECURITY.md`, `CONTRIBUTING.md` | Default reporting route and contribution guide |
| `.github/workflows/` | The reusable brand guard |
| Root `*.md`, `LICENSE` | This repository's own documentation and terms |

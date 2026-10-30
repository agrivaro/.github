# AGENTS.md

The contract for working in `agrivaro/.github`, the organization's defaults
repository. Read it before editing anything here. The product repositories are
private and each has its own `AGENTS.md`; none of their rules live here.

## This repository is public

Anyone can read every file, commit and pull request in it. Write as if for the
organization's overview page: no customer names, prices, environment names,
internal URLs, known security gaps, credentials or screenshots. A fact that is
only true inside a private repository stays in that repository.

## What lives here, and whom it reaches

| Path | Reaches |
| --- | --- |
| `profile/README.md` | The organization overview page on github.com/agrivaro |
| `PULL_REQUEST_TEMPLATE.md` | Every repository in the organization without a pull request template of its own |
| `SECURITY.md`, `CONTRIBUTING.md` | Every repository in the organization without a file of that type, private ones included |
| `.github/workflows/brand-guard.yml` | Every repository that calls it, on that repository's next run |
| `README.md`, `ARCHITECTURE.md`, `CODE_STYLE.md`, `DESIGN_SYSTEM.md`, `PRD.md`, `TESTING.md`, `LICENSE`, this file | This repository only. GitHub cannot inherit a licence, so every repository carries its own `LICENSE` |

Editing a default edits it for every repository that relies on it, so the
wording must hold for all of them. Before changing one, check which
repositories still use it, for example
`gh api repos/agrivaro/<repo>/contents/SECURITY.md` (a 404 means that repository
uses the default). [`ARCHITECTURE.md`](ARCHITECTURE.md) explains how GitHub
resolves each file.

## The brand guard mirrors agrivaro/branding

The four hex values and the `sha256` pins in
[`.github/workflows/brand-guard.yml`](.github/workflows/brand-guard.yml) are
copies of `hexcode.md` and `logos/` in the private `agrivaro/branding`
repository. They are never the source:

- update them in the same work item as the brand change and its consumers, from
  `shasum -a 256` over `branding/logos/*.svg` at the commit being mirrored, never
  by hand-editing a hash;
- callers reference the workflow as `@main`, so a merge here changes the next
  run of every caller;
- inputs reach the script through `env:`, never as `${{ inputs.* }}` inside
  `run:`. Keep it that way: interpolating an input into the script lets the
  caller's value execute as shell ([`CODE_STYLE.md`](CODE_STYLE.md)).

[`DESIGN_SYSTEM.md`](DESIGN_SYSTEM.md) says what this repository carries from
the brand and what it must not invent.

## Branches and pull requests

This repository has one long-lived branch, `main`. Branch off it, name the
branch `<kind>/<what-it-does>`, and open the pull request against `main`. That
differs from the product repositories, which integrate on `development`.
[`CONTRIBUTING.md`](CONTRIBUTING.md) has the checklist and
[`TESTING.md`](TESTING.md) how to verify a change, because nothing here runs on
its own push.

## Language

English, wrapped at about 80 columns, like the rest of the organization's
repository documentation.

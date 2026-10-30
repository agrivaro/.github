# Design system

This repository holds no visual source and defines no colour, font or mark.
The Agrivaro product brand has one source of truth: the private
`agrivaro/branding` repository.

| What | Owned by, in `agrivaro/branding` |
| --- | --- |
| The four canonical brand colours | `hexcode.md` |
| Derived colours, colour roles and contrast rules | `colors.md` |
| The canonical vector marks | `logos/` |

When this repository and `branding` disagree, `branding` is right and this
repository is the one to fix.

## What this repository carries from the brand

Two mirrors, both written for enforcement or review rather than for design:

- **The brand guard**
  ([`.github/workflows/brand-guard.yml`](.github/workflows/brand-guard.yml))
  holds the four canonical hex values and the `sha256` of every file in
  `branding/logos/`, so consumer repositories can prove their copies have not
  drifted. [`ARCHITECTURE.md`](ARCHITECTURE.md#the-brand-guard) describes what
  it checks and what it cannot.
- **The pull request template's Brand section**
  ([`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md)) restates the brand
  rules a reviewer most often needs: colours only from `hexcode.md` and
  `colors.md`, black text on orange, the light orange accent on a green or black
  ground, byte-for-byte logo copies, and platform metadata updated with the
  brand.

The organization profile page uses no images or colours of its own.

## Making a change

A brand change starts in `agrivaro/branding`, then updates every consumer in
the same work item. In this repository that means the guard's hex values and
pins, refreshed as described in [`AGENTS.md`](AGENTS.md#the-brand-guard-mirrors-agrivarobranding),
and the template's Brand section if a rule it restates has changed. Never
introduce a colour or mark here that `branding` does not have.

## What is not covered

- **Tenant branding.** A customer's company name and logo are set per
  environment in the product and never become brand inputs.
- **Autovaro.** The Autovaro skin's palette and artwork live in
  `agrivaro/autovaro`. The guard knows only the Agrivaro marks and values.

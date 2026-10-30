# Product requirements: agrivaro/.github

**Status:** describes the repository as it is on 30 October 2026. The
requirements below are drawn from the repository's own [README](README.md),
the workflow's header comment and GitHub's documented behaviour; this file adds
no new commitment.

## Problem

Agrivaro is built across several repositories, almost all of them private.
Without a shared surface, every repository would carry its own copy of the
pull request checklist and the vulnerability reporting route, those copies
would drift, and a visitor to github.com/agrivaro would see nothing about what
the organization builds. Separately, the Agrivaro brand is mirrored by hand
into several repositories, and nothing would notice when a mirror falls behind
the brand source.

## Users

| Who | What they need from this repository |
| --- | --- |
| Visitors to the organization page | A short, public description of what Agrivaro builds |
| Members opening a pull request in any repository | One checklist, including the brand gate |
| Anyone who finds a vulnerability | One reporting route, wherever they found it |
| Consumer repositories' CI | A check that their brand copies match the source |

## Functional requirements

| # | Requirement | State |
| --- | --- | --- |
| R1 | `profile/README.md` describes what Agrivaro builds on the organization page, in English | Met |
| R2 | A default pull request template applies to every repository without its own, and carries the brand checklist | Met |
| R3 | A default `SECURITY.md` and `CONTRIBUTING.md` apply to every repository without its own | Met by the change that added this file |
| R4 | A reusable workflow fails when a consumer's brand files lack a canonical hex value or a vendored logo SVG is not a byte-for-byte canonical copy, and fails when it finds no SVG to check | Met |
| R5 | The guard's values match `agrivaro/branding` | Met: on 30 October 2026 the 13 pins equal the `sha256` of the 14 files in `branding/logos/` (two are identical) and the four hex values equal `hexcode.md` |

## Non-functional requirements

- **Public by necessity.** GitHub applies the defaults and shows the profile
  only while this repository is public, so it may hold nothing confidential
  ([`AGENTS.md`](AGENTS.md#this-repository-is-public)).
- **Safe to run in other repositories' CI.** The guard reads only the caller's
  checkout, uses no secret, writes nothing and never interpolates an input into
  shell ([`CODE_STYLE.md`](CODE_STYLE.md#workflow-yaml)).
- **Holds for every repository.** A default's wording must be true in each
  repository that inherits it.

## Out of scope

- The brand itself: `agrivaro/branding` owns every colour and mark
  ([`DESIGN_SYSTEM.md`](DESIGN_SYSTEM.md)).
- Product code and product documentation, which live in the private product
  repositories.
- A licence for other repositories: GitHub cannot inherit one.

## Open questions

Facts observed on 30 October 2026 that someone with the authority should
decide on:

1. **No repository calls the brand guard yet.** The README's values for `web`
   and `crm` are ready to paste, and those repositories' copies pass the checks
   as written.
2. **Two other repositories that mirror the brand would not pass the guard as
   it stands**, one on a missing canonical colour and one on a logo SVG that is
   not a byte-for-byte copy. Whether those are deliberate adaptations or drift
   is for the brand owner to decide before either repository calls the guard.
3. **The pull request template names `crm`, `web` and `branding` as the
   repositories a brand change is mirrored into.** `docs` and `control` mirror
   the brand too.
4. **The profile lists the Agrivaro products only.** Whether Autovaro belongs
   on the organization page is a product decision.

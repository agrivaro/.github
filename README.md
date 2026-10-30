# Agrivaro organization defaults

This public repository holds the organization-wide GitHub surface for
[agrivaro](https://github.com/agrivaro). Repositories in the organization use
these files unless they define their own.

| Path | What it does |
| --- | --- |
| `profile/README.md` | Shown on the organization's overview page. |
| `PULL_REQUEST_TEMPLATE.md` | Default pull request template for every repository that does not have its own. |
| `SECURITY.md` | Default vulnerability reporting route for every repository that does not have its own. |
| `CONTRIBUTING.md` | Default contribution guide for every repository that does not have its own. |
| `.github/workflows/brand-guard.yml` | Reusable workflow that keeps checked-in copies of the Agrivaro brand from drifting. |

## Calling the brand guard

The guard checks that the four canonical hex values are present in the calling
repository and that every vendored logo SVG is a byte-for-byte copy of a file
in `branding/logos/`. It checks `.svg` files only; raster translations (PNG app
icons) cannot be hash-checked.

Add this workflow to the repository:

```yaml
name: Brand guard

on:
  pull_request:

jobs:
  brand:
    uses: agrivaro/.github/.github/workflows/brand-guard.yml@main
    with:
      brand_files: <files that carry the brand hex values>
      asset_dirs: <directories with copied logo SVGs>
```

Values for the repositories whose brand copies pass the guard today, ready to
paste:

| Repository | `brand_files` | `asset_dirs` |
| --- | --- | --- |
| `web` | `src/app.css` | `public/brand` |
| `crm` | `packages/frontend/src/globals.css packages/mobile/src/components/theme.ts` | `packages/frontend/public/brand packages/mobile/assets/brand` |

## Updating the pins

The guard pins the `sha256` of the files in `branding/logos/`. When the brand
changes, update the pins in `.github/workflows/brand-guard.yml` in the same
work item that updates the consumers:

```bash
sha256sum ../branding/logos/*.svg   # macOS: shasum -a 256
```

A stale pin does not break anything on its own, but it stops the guard from
noticing that a consumer still copies an old revision.

## Repository documents

| File | What it covers |
| --- | --- |
| [`AGENTS.md`](AGENTS.md) | The rules for working here: public content, whom each file reaches, the brand pins |
| [`ARCHITECTURE.md`](ARCHITECTURE.md) | How GitHub applies each file, and how the brand guard works |
| [`CODE_STYLE.md`](CODE_STYLE.md) | Markdown and workflow conventions |
| [`DESIGN_SYSTEM.md`](DESIGN_SYSTEM.md) | What this repository carries from `agrivaro/branding`, and what it must not invent |
| [`PRD.md`](PRD.md) | What this repository is for, its requirements and open questions |
| [`TESTING.md`](TESTING.md) | How to verify a change, since nothing here runs on its own push |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How a change gets in, here and in other repositories by default |
| [`SECURITY.md`](SECURITY.md) | How to report a vulnerability |
| [`LICENSE`](LICENSE) | Proprietary; public visibility grants no licence |

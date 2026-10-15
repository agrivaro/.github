# Agrivaro organization defaults

This public repository holds the organization-wide GitHub surface for
[agrivaro](https://github.com/agrivaro). Repositories in the organization use
these files unless they define their own.

| Path | What it does |
| --- | --- |
| `profile/README.md` | Shown on the organization's overview page. |
| `PULL_REQUEST_TEMPLATE.md` | Default pull request template for every repository that does not have its own. |
| `.github/workflows/brand-guard.yml` | Reusable workflow that keeps checked-in copies of the Agrivaro brand from drifting. |

## Brand source

The brand itself lives in the private `agrivaro/branding` repository.
`hexcode.md` and `colors.md` are authoritative and `logos/` holds the
true-vector marks. The SVGs under `profile/assets/` are byte-for-byte copies;
`agrivaro-app-icon.png` is a 512 px render of `logos/05-app-icon.svg`, made for
platforms that want a raster (for example the organization avatar).

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

Values for the current consumers, ready to paste:

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

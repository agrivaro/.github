# Testing

Nothing in this repository runs on its own push: there is no test suite, no
linter and no workflow that triggers on this repository's events. The brand
guard declares only `on: workflow_call`, so it runs when another repository
calls it. Verifying a change is therefore manual, and the pull request says
which of the steps below were done.

## Brand guard pins

The pins must equal the `sha256` of the files in `agrivaro/branding`'s
`logos/`, and the four hex values must equal its `hexcode.md`. With `branding`
checked out next to this repository:

```bash
diff \
  <(grep -oE '[0-9a-f]{64}' .github/workflows/brand-guard.yml | sort -u) \
  <(shasum -a 256 ../branding/logos/*.svg | cut -d' ' -f1 | sort -u) \
  && echo "pins match branding/logos"

grep -o '#[0-9A-Fa-f]\{6\}' ../branding/hexcode.md
grep 'colors=' .github/workflows/brand-guard.yml
```

No output from `diff` and the "pins match" line mean the pins are current. Two
files in `logos/` are identical, so there are fewer pins than files.

## A change to the guard's behaviour

Run it from a consumer repository before merging here:

1. In a branch of a consumer repository, add a caller that references your
   branch instead of `main`:
   `uses: agrivaro/.github/.github/workflows/brand-guard.yml@<your-branch>`,
   with that repository's `brand_files` and `asset_dirs` from the
   [README](README.md#calling-the-brand-guard).
2. Open a draft pull request there and confirm the guard passes.
3. Make it fail once on purpose, for example by changing one canonical hex
   value in a scratch commit, and confirm it goes red with an annotation naming
   the problem. A check that has never been seen to fail proves nothing.
4. Close the draft and delete the scratch branch.

Callers reference `@main`, so once merged the change applies to every caller's
next run.

## A change to a default file

Read the new wording in the context of each repository that inherits it
([`AGENTS.md`](AGENTS.md#what-lives-here-and-whom-it-reaches) says how to find
them), and check that every link resolves for the readers who will follow it.

## What is not tested

- Raster brand files, such as PNG app icons, cannot be hash-compared and are
  not checked by the guard.
- Derived colours from `branding/colors.md` are not checked.
- The profile page's rendering is checked by eye on github.com/agrivaro after
  merging.

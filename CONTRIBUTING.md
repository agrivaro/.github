# Contributing

How a change reaches an agrivaro repository. This file is the default for every
agrivaro repository that has no `CONTRIBUTING.md` of its own, and the guide for
this repository itself. It owns no product rule: each repository's own
`AGENTS.md` and `README.md` do, and when they disagree with this file they are
right.

## In any agrivaro repository

The repositories are maintained by Groeneveld-ICT BV. Contributions come from
the team and the people working with it, through pull requests.

- Read the repository's `AGENTS.md` and `README.md` before the first change.
- The product repositories integrate on `development`; `main` is where a release
  or a publication happens. Branch off `development` unless the repository says
  otherwise, and open the pull request against it. GitHub proposes `main` as the
  base, so say so explicitly: `gh pr create --base development`.
- Fill in the pull request template, including its Brand section when the change
  touches the UI, brand assets or platform metadata.
- Write code, commit messages and pull request descriptions in English.
- Never commit a secret, a credential or customer data.
- Report a vulnerability as [`SECURITY.md`](SECURITY.md) describes, never in an
  issue or a pull request.

## In this repository

This repository is public and has one long-lived branch, `main`.

- Branch off `main`, name the branch `<kind>/<what-it-does>`, and open the pull
  request against `main`.
- Read [`AGENTS.md`](AGENTS.md) first. It lists what may never be written in a
  public repository, and whom each file reaches.

Before you open the pull request:

- [ ] Nothing in the diff is confidential: no customer names, prices,
      environment names, internal URLs, known security gaps or credentials.
- [ ] A change to `PULL_REQUEST_TEMPLATE.md`, `SECURITY.md` or
      `CONTRIBUTING.md` still holds in every repository that inherits it.
- [ ] A change to the brand guard has been run from a consumer repository and
      seen to fail once, and its pins match `agrivaro/branding`
      ([`TESTING.md`](TESTING.md)).
- [ ] The description says which verification steps were done.

```bash
gh pr create --base main
```

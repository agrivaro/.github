# Security policy

How to report a vulnerability in anything Groeneveld-ICT BV builds under the
agrivaro organization. This file is the default for every agrivaro repository
that has no `SECURITY.md` of its own; where a repository has one, that file is
more specific and wins.

## Reporting a vulnerability

Email **info@groeneveld-ict.nl** and start the subject with "Security".

Do not report it in a GitHub issue, a pull request, a discussion, a commit
message or a public channel. Everyone with access reads those, and a hosted
customer environment stays exposed until it runs a fixed release. Please keep
the details confidential until we confirm that a fix has been released.

Include what you can of:

- the product, repository or hosted environment affected
- the version or commit
- how to reproduce it, and what an attacker gains: which data, which customer
  environment, and which role or access they need to start from
- whether real data was, or may have been, exposed

## An exposed secret

A key, password, connection string or token in a commit, a log, a screenshot or
a chat is a vulnerability. Report it at once. Rotation comes before clean-up,
because removing a secret from git history does not revoke it.

## Testing

Do not probe a customer environment or Groeneveld-ICT's cloud resources without
written agreement from Groeneveld-ICT and that customer. Read, change or keep no
more data than it takes to show the problem, and do not degrade the service for
anyone else. A repository's own `SECURITY.md` says how to test against a local
copy of it.

## This repository

`agrivaro/.github` is public and holds the organization defaults and the
reusable brand guard, which runs in other repositories' CI. Report, for
example, a way for a caller's input to execute as code, or a change that would
make the workflow read a secret or write to the calling repository.

## What happens next

Groeneveld-ICT handles the report in the affected repository, through that
repository's own release process. A hosted customer environment is protected
once it runs the fixed release, not when the fix is merged.

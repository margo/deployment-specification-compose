# Contributing

## New Features

Contributions to Margo are welcome. Maintainers are responsible for maintaining
accepted additions, so proposed enhancements are evaluated against both their
value and ongoing maintenance cost.

## Bug Fixes

Bug and security fixes take priority. Follow the [security policy](SECURITY.md)
when reporting a vulnerability; do not disclose it in a public issue.

## Contribution Checklist

* Cover specification contributions with a Corporate CLA or Individual CLA
* Add tests for executable code changes, where applicable
* Include appropriate copyright headers in new files; see [LICENSE](LICENSE)
* Add a `Signed-off-by` trailer to every commit to certify the Developer's Certificate of Origin
* Structure commits as small, logically separate changes
* Keep each commit usable; do not introduce temporary breakages
* Base changes on the latest `pre-draft` branch
* Review Markdown rendering and verify links before submitting documentation changes

### Signing the CLA

If you have not signed the Individual CLA, your organization has not signed the
Corporate CLA, or your account is not authorized by your organization, the
[LFX EasyCLA bot](https://easycla.lfx.linuxfoundation.org/#/) will guide you through
the authorization steps.

To confirm coverage before submitting a contribution, open a pull request at
[Margo EasyCLA](https://github.com/margo/EasyCLA).

### Sign Your Work

The sign-off is a line at the end of the commit message:

```text
Signed-off-by: Random J Developer <random@developer.example.org>
```

It certifies that you wrote the contribution or otherwise have the right to
submit it as an open-source contribution. Check with your employer when
contributing work you do not own personally.

Use `git commit -s` to add your configured name and email automatically.
For an existing commit, use `git commit --amend --no-edit -s`. Rewriting an
already published commit changes its ID and requires coordination with maintainers.

Git does not use `commit.signoff=true` to add this trailer automatically.
The workspace enables VS Code's `git.alwaysSignOff` setting for commits made
through its Git integration. Command-line commits still require `-s`.

A `Signed-off-by` trailer is separate from a cryptographic commit signature.
SSH or GPG commit signing, when configured locally, does not replace the trailer.

### Developer's Certificate of Origin 1.1

```text
By making a contribution to this project, I certify that:

    (a) The contribution was created in whole or in part by me and I
        have the right to submit it under the open source license
        indicated in the file; or

    (b) The contribution is based upon previous work that, to the best
        of my knowledge, is covered under an appropriate open source
        license and I have the right under that license to submit that
        work with modifications, whether created in whole or in part
        by me, under the same open source license (unless I am
        permitted to submit under a different license), as indicated
        in the file; or

    (c) The contribution was provided directly to me by some other
        person who certified (a), (b) or (c) and I have not modified
        it.

    (d) I understand and agree that this project and the contribution
        are public and that a record of the contribution (including all
        personal information I submit with it, including my sign-off) is
        maintained indefinitely and may be redistributed consistent with
        this project or the open source license(s) involved.
```

## Contribution Integration Process

1. Create a pull request against `pre-draft` on
   [this repository](https://github.com/margo/deployment-specification-compose/pulls).
2. Complete the EasyCLA check and applicable automated checks.
3. Allow at least seven days for review, as checked by the PR Age Check workflow.
4. Obtain maintainer approval before merging.

The PR Age Check runs when a pull request is opened, updated with new commits,
or marked ready for review. It does not rerun automatically after seven days.
After the waiting period, rerun the failed check from the Actions interface
or update the pull request with a new commit.

## Documentation

This repository currently contains Markdown documentation and GitHub community
configuration. It does not yet provide a documentation build, dependency manifest,
development container, or LinkML generation scripts.

Preview Markdown changes and check relative links locally. If documentation
generation tooling is added, document its setup and validation commands here.

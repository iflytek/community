# Pull Request Guidelines

<!-- toc -->
- [Before You Submit a Pull Request](#before-you-submit-a-pull-request)
- [The Pull Request Process](#the-pull-request-process)
  - [Marking Unfinished Pull Requests](#marking-unfinished-pull-requests)
- [Why Was My Pull Request Closed?](#why-was-my-pull-request-closed)
- [Why Is My Pull Request Not Getting Reviewed?](#why-is-my-pull-request-not-getting-reviewed)
- [Best Practices for Faster Reviews](#best-practices-for-faster-reviews)
  - [Smaller Is Better](#smaller-is-better)
  - [Comments Matter](#comments-matter)
  - [Test](#test)
  - [Squashing](#squashing)
  - [Commit Message Guidelines](#commit-message-guidelines)
  - [Common Sense and Courtesy](#common-sense-and-courtesy)
  - [Large or Automatic Edits](#large-or-automatic-edits)
<!-- /toc -->

This doc explains the process and best practices for submitting a pull request to the Iflytek Opensource Community.

## Before You Submit a Pull Request

First-time contributors should head to the [Contributor Guide](../CONTRIBUTING.md) to get started.

Make sure your pull request adheres to our best practices. These include following project conventions, making small pull requests, and commenting thoroughly.

## The Pull Request Process

Merging a pull request requires the following steps to be completed:

1. Open a pull request against the appropriate branch
2. Follow the [CLA](../CLA.md) process (prerequisite)
3. Pass all automated checks
4. Get approvals from reviewers and code owners

### Marking Unfinished Pull Requests

If you want to solicit reviews before your pull request is complete, you can hold it to prevent premature merging:

1. Add `WIP` or `[WIP]` prefix to your pull request title
2. Or add a comment with `/hold`

While either is present, your pull request will not be considered for merging.

## Why Was My Pull Request Closed?

Pull requests that are inactive for more than **90 days** will be closed. Closed pull requests are easy to recreate, and little work is lost by closing a PR that subsequently needs to be reopened. We want to:

* Maintain a clean project
* Remove old PRs that would be difficult to rebase
* Encourage code velocity

## Why Is My Pull Request Not Getting Reviewed?

A few factors affect how long your pull request might wait for review:

* **PR is too big** — Large PRs are intimidating to review. Break them up.
* **Release cycle timing** — During stabilization periods, review may slow down.
* **Wrong reviewers** — Make sure the right people are assigned.

If you've followed best practices and still aren't getting reviews:

1. Ask for a reviewer to be assigned: comment with the PR link and tag a maintainer
2. Ping the assignee on the PR comment stream
3. Reach out via our [communication channels](../README.md#contact)
4. If you've fixed all review issues and haven't heard back, comment with "please take another look" (PTAL)

## Best Practices for Faster Reviews

### Smaller Is Better

Small commits and small pull requests get reviewed faster and are more likely to be correct than big ones.

* **Break up commits** — Group logically distinct ideas into separate commits
* **Break up PRs** — If you can extract whole ideas, send them as separate PRs
* **Don't mix concerns** — Put fixes and new features in different PRs

We'd rather have 10 small, obvious pull requests than 1 unreviewable monolith.

### Comments Matter

In your code, if someone might not understand why you did something, comment it. Many code-review comments are about this exact issue.

If you think there's something we could follow up on, add a `TODO`.

### Test

Nothing is more frustrating than starting a review, only to find that the tests are inadequate or absent. If you don't know how to test your change, please ask — we'll be happy to help.

### Squashing

During review, put fixups in new commits rather than amending. We may ask you to clean up commits at the very end (typically when the PR is tagged `LGTM`).

Each commit should have a good title line (<70 characters) and include a description paragraph.

### Commit Message Guidelines

* Use imperative mood: "Fix bug" not "Fixed bug" or "Fixes bug"
* Limit the subject line to 70 characters
* Separate subject from body with a blank line
* Explain **what** and **why**, not **how**

### Common Sense and Courtesy

Be polite, be responsive, and communicate openly with your reviewers. If you disagree with feedback, explain your reasoning clearly.

### Large or Automatic Edits

Large-scale refactoring or automated changes (e.g., running a linter across the whole repo) should be discussed with maintainers before submitting. Break such changes into per-SIG or per-module PRs rather than one sweeping change.

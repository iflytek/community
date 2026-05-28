# Issue Guidelines

<!-- toc -->
- [Before You Open an Issue](#before-you-open-an-issue)
- [Opening an Issue](#opening-an-issue)
- [Issue Triage Process](#issue-triage-process)
  - [What Is Triaging?](#what-is-triaging)
  - [How We Triage](#how-we-triage)
- [Issue Lifecycle](#issue-lifecycle)
- [Issue Labels](#issue-labels)
<!-- /toc -->

## Before You Open an Issue

* Search [existing issues](https://github.com/iflytek/community/issues) to avoid duplicates
* Check our [documentation](../README.md) for answers
* Reach out via our [communication channels](../README.md#contact) for support requests

## Opening an Issue

When opening an issue, please include:

1. **Clear title** — Describe the problem in one line
2. **Description** — What happened, what you expected, steps to reproduce
3. **Environment** — Version, OS, relevant configuration
4. **Screenshots/logs** — If applicable, include supporting evidence

Use our [issue templates](../.github/ISSUE_TEMPLATE/) when available.

## Issue Triage Process

### What Is Triaging?

Issue triage is the process by which maintainers review and categorize incoming GitHub issues, then organize them to be actioned. This involves categorizing issues by priority, ownership, and type (bug, feature, etc.).

### How We Triage

1. **Review** — Maintainers review newly created issues
2. **Categorize** — Apply appropriate labels (`kind/`, `priority/`, `sig/`)
3. **Assign** — Route to the right maintainer or team
4. **Follow up** — Ensure issues are being addressed within the release cycle

Maintainers use the `/triage accepted` label to indicate an issue has been reviewed and accepted for action.

## Issue Lifecycle

1. **Open** — Issue is created with description
2. **Triage** — Maintainers review and label
3. **In Progress** — Someone is actively working on it
4. **Resolved** — Fix is merged or issue is closed with explanation

Issues with no activity for **90 days** may be closed. Issues can be reopened if still relevant.

## Issue Labels

| Label Prefix | Purpose | Examples |
|---|---|---|
| `kind/` | Type of issue | `kind/bug`, `kind/feature`, `kind/documentation` |
| `priority/` | Urgency | `priority/critical`, `priority/high`, `priority/medium`, `priority/low` |
| `sig/` | Responsible group | `sig/architecture`, `sig/docs` |
| `triage/` | Triage status | `triage/accepted`, `triage/needs-information` |

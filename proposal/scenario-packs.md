# Repository Proposal: scenario-packs

## Metadata

- Target Repository Name: scenario-packs
- Primary Authors: @XiaoSeS
- Initial Maintainers: @dongmucat (primary), @XiaoSeS
- Requested Repository Admins: @dongmucat, @XiaoSeS
- Tracking Issue: https://github.com/iflytek/community/issues/24
- Proposed Landscape: Application Platforms & Harness / Protocols and Interoperability (3.1.2)

The maintainer list is confirmed by the project team. Organization membership and
the template's minimum two-year maintenance commitment remain to be confirmed by
the maintainers during review.

<!-- toc -->
- [1. Abstract](#1-abstract)
- [2. Motivation](#2-motivation)
  - [2.1 Pain Points](#21-pain-points)
  - [2.2 Use Cases](#22-use-cases)
  - [2.3 Why a Separate Repository?](#23-why-a-separate-repository)
- [3. Scope &amp; Goals](#3-scope--goals)
  - [3.1 Primary Goals](#31-primary-goals)
  - [3.2 Non-Goals](#32-non-goals)
- [4. Architecture &amp; Dependencies](#4-architecture--dependencies)
  - [4.1 Core Components](#41-core-components)
  - [4.2 Dependencies](#42-dependencies)
- [5. Governance &amp; Maintenance](#5-governance--maintenance)
- [6. Release Policy](#6-release-policy)
- [7. CI/CD &amp; Security](#7-cicd--security)
- [8. Repository Configuration](#8-repository-configuration)
- [9. Risks &amp; Alternatives](#9-risks--alternatives)
- [10. Timeline](#10-timeline)
- [11. References](#11-references)
<!-- /toc -->

## 1. Abstract

Scenario Packs provides open scenario-pack specifications, an AI authoring Skill,
a standalone compiler, and reusable business examples for Astron OS. Its users
are business-flow developers, agent-application developers, and platform
integrators. They can use their own AI tools to turn a business process into
versioned Workflow, runtime Skill, Program, and Schema resources, test them
locally, and build a deterministic ZIP for acceptance on a compatible OS
deployment. The project keeps authoring resources separate from OS services and
distinguishes a valid declaration, a passing local program test, and an accepted
business run.

## 2. Motivation

### 2.1 Pain Points

- A process described only in prompts leaves its input and output rules, stage
  transitions, and human confirmation requirements scattered across instructions.
  Changes are difficult to validate together.
- Pack authors must coordinate Workflow declarations, Programs, Schemas,
  identities, and approval receipts. A package can compile while its business
  process still fails at runtime.
- Authoring tools tied to the full platform source or an internal environment
  make independent trials, reproductions, and contributions harder.

### 2.2 Use Cases

The repository includes examples for the following uses. These examples illustrate
reuse; they are not evidence of production adoption or broad user demand.

1. **Requirement clarification and confirmation.** Turn a request into a draft,
   collect missing details, and save the revision confirmed by its creator.
2. **Equipment request approval.** Let the applicant confirm the request before a
   designated approver decides it, with two separate gates bound to the revision.
3. **Submission and review.** Let a collaborator submit materials in one managed
   task and let the creator accept or return the delivery for revision.
4. **Custom pack authoring.** Start from the standalone Skill's unimplemented
   skeleton, write the program, Schemas, workflow, and business tests, then compile
   the pack locally before testing it on the intended OS deployment.

### 2.3 Why a Separate Repository?

The maintained assets are author-facing specifications, tools, and examples, with
their own contributors and release cycle. A separate repository lets authors read
and test those assets without obtaining the complete OS implementation. Keeping
the specifications, compiler, and examples together also makes contract changes
reviewable in one place. Shared runtime capabilities remain explicit contracts
with the target OS.

## 3. Scope & Goals

### 3.1 Primary Goals

- Maintain Source/ZIP v1 and the author contracts for Workflow/Stage execution,
  Program data handoff, and human collaboration.
- Provide a copyable AI authoring Skill with a minimal skeleton that has no
  implemented business logic.
- Provide a compiler using only Node.js built-ins for path and resource checks,
  Schema reference closure, selected author-contract checks, deterministic ZIPs,
  and content digests.
- Maintain three official business examples, a separate teaching example, and
  tests using synthetic data.
- Report local checks, program tests, compilation, and OS business acceptance
  separately, while developing the candidate public runtime protocol by module.

### 3.2 Non-Goals

- Shipping Astron OS, Studio, a model service, a full multi-command CLI, or a
  production runtime in this repository.
- Giving pack Programs their own network, filesystem, credential, or subprocess
  access. Programs use the target OS's context/model/tool interface.
- Treating compilation, local substitutes, or candidate conformance vectors as
  certification of OS compatibility.
- Expanding external business Tool integrations or cross-repository OS CI in this
  proposal.

## 4. Architecture & Dependencies

### 4.1 Core Components

| Component | Responsibility |
| --- | --- |
| `docs/`, `schemas/` | Author specifications, Source and runtime contracts, machine Schemas, compatibility and validation guidance |
| `skills/scenario-pack-author/` | Copyable authoring Skill, minimal skeleton, selected references, and compiler; both the directory and ZIP include LICENSE and NOTICE |
| `tools/scenario-pack-source-v1/` | Source v1 validation, resource collection, deterministic compilation, and digests |
| `examples/` | Requirement clarification, equipment approval, and submission/review examples with separate program tests |
| `test/`, `tools/test/` | Candidate conformance tests, OS acceptance drivers, and offline regressions; excluded from runtime Source and the standalone Skill |

A business pack's Source contains only declared runtime resources. Ordinary
tests, authoring documentation, and generated archives stay outside it. The target
OS provides identity, permissions, persistence, model access, human tasks,
approvals, execution, and recovery.

### 4.2 Dependencies

- **Authoring and compilation:** Node.js 22 or later. The compiler and standalone
  Skill tools require no third-party npm runtime dependencies.
- **Repository tests:** locked Ajv and Playwright development dependencies; some
  conformance tests also require Python 3.
- **OS acceptance:** the selected driver determines Linux, browser, authorized
  target, and test-host requirements. Enhanced failure/recovery acceptance needs
  additional host capabilities.
- **Running a pack:** a target OS implementing its declared execution capabilities.
  Models and human interaction are needed only when the pack declares them;
  deterministic packs can use neither. The three official human-flow examples
  have additional capability requirements.

There is currently no fully accepted public OS compatibility matrix. The public
runtime protocol design remains a draft.

## 5. Governance & Maintenance

- **Maintainers:** @dongmucat leads daily maintenance, coordination, release
  planning, and release checks. @XiaoSeS contributes as an author and co-maintainer,
  including cross-review. Both maintainers will confirm organization membership
  and the required minimum two-year commitment during proposal review.
- **Review:** use `main`, `dev`, and feature/fix branches as documented in the
  contribution guide. The organization repository is intended to require
  non-author Committer approval and passing CI. Synchronize OWNERS and platform
  permissions when the repository is established; the local single-maintainer
  arrangement is not presented as already meeting that organization setup.
- **Community growth:** accept documentation fixes, example adaptations,
  reproducible issues, and synthetic test cases. New business examples must state
  their required capabilities, independent expectations, and actual validation.
- **Exit strategy:** if maintenance stops, announce it in the README, stop taking
  new feature work, and seek a transfer through the community process. Archive the
  repository if no successor is available, retaining version records and support
  boundaries.

## 6. Release Policy

- Use Semantic Versioning. The current `0.1.6` is an unreleased development
  preview; archive-format and author-contract versions are managed separately.
- Review patch needs monthly and plan releases quarterly. Choose version changes
  according to compatibility impact; do not overwrite a published version.
- Distribute source through the organization repository and standalone Skill ZIPs
  through GitHub Releases, with changelogs, LICENSE, NOTICE, and SHA-256 digests.
  There is no public download yet, and automated publishing is outside this PR.
- Publish OS/client compatibility statements only for capabilities and versions
  actually accepted. No general runtime compatibility or GA date is promised.

## 7. CI/CD & Security

- **CI configuration:** Node.js 22/24 matrix running the root `npm test`, Skill
  synchronization checks, three example compilations, and a check that builds do
  not alter tracked files. Actual organization CI execution and branch protection
  remain to be verified after setup.
- **Recorded local validation:** the latest local working-tree run passed 413
  regression tests; the standalone-distribution subset passed 5 tests; all three
  official examples compiled. These results include uncommitted local preparation
  and are not results for a published source commit or remote CI run.
- **Acceptance boundary:** the examples still report `authorValidation:
  incomplete` for checks requiring additional module or OS validation. Compilation
  and local substitutes do not establish business acceptance. No new remote OS
  acceptance was performed for this repository request.
- **Distribution:** the current standalone Skill has 30 files, with LICENSE and
  NOTICE matching the repository copies. Its local ZIP SHA-256 is
  `5bdbc1015e123e0f981428c57316dba35aa2f41491b1ab711e7613d93f4e4a70`.
  This identifies a local artifact, not a public release.
- **License and open scope:** Apache License 2.0. NOTICE describes the reference
  compiler and adapted workflow material; the lockfile records development
  dependencies. The project team has confirmed that the proposed public protocol,
  authoring resources, compiler, and synthetic examples contain no sensitive
  business data.
- **Credentials:** checks of the current source and standalone Skill archive found
  no real passwords, API keys, tokens, or private keys requiring removal. Test
  placeholders and runtime-generated values remain. This does not claim a full
  Git-history audit.
- **Reporting:** use the community's private reporting channel in
  [SECURITY.md](../SECURITY.md). A project security contact remains to be designated
  during review. Publish a redacted summary after a fix and coordinated disclosure.
- **Review follow-up:** provide an accessible source/PoC snapshot and bind the
  validation date, commands, and results to that snapshot in the tracking issue.
  Confirm which organization security checks are available before describing them
  as active gates.

## 8. Repository Configuration

The checkboxes describe requested settings, not an existing repository's state.

- [x] Enable Issues
- [x] Enable Discussions
- [ ] Enable Wiki
- [ ] Transfer existing external repo (not applicable; this is a new repository)

Create a new public `scenario-packs` repository in the iflytek organization, with
@dongmucat as primary maintainer and @XiaoSeS as co-maintainer. Request repository
administrator access for both maintainers.
Use `main` as the default branch and configure branch protection and required
checks after creation. The proposed name follows the community template's
lowercase-hyphen convention; if reviewers require another repository name, assess
that separately from existing runtime pack identifiers.

## 9. Risks & Alternatives

| Risk | Mitigation |
| --- | --- |
| Users mistake compilation or substitute tests for a completed business run | Separate static checks, program tests, compilation, and OS acceptance in documentation and reports |
| OS, client, and example capabilities differ by version | Keep explicit capability requirements and establish support from fixed-version acceptance evidence |
| Specifications, compiler, and copied Skill resources drift | Maintain one source, explicit generation inputs, synchronization checks, and directory/ZIP byte comparisons |
| Model output or a stale approval is treated as authorization | Leave identity and approval facts to the OS; example Programs check revisions, actors, and correlated receipts |
| Maintenance capacity becomes unavailable | Share maintenance, with @dongmucat leading daily work, and use the transfer/archive strategy above |

Alternatives include keeping the material in the OS repository, distributing only
the authoring Skill, or using a personal repository. The OS repository increases
the material authors need to obtain and navigate. A Skill alone cannot house the
full specifications, regression suites, and example lifecycle. An organization
repository provides shared governance while retaining a small standalone Skill
for day-to-day authoring.

## 10. Timeline

These are proposed targets for maintainer confirmation. T is the date the proposal
is accepted and the required maintainer and repository details are settled.

- **Proposal review:** submit this proposal linked to #24; confirm maintainer
  eligibility and commitments, designate the project security contact, and
  provide accessible source/PoC review material.
- **Alpha development:** `0.1.6` already exists as a local preview. Target the
  initial organization-repository setup within five working days after T, including
  source preparation, ownership and license files, CI, and branch protection.
- **Preview iteration:** target one round of external authoring feedback in the
  first month after T, updating tutorials and distribution instructions from
  observed results.
- **Beta/GA:** no date committed. Propose stable releases and supported capability
  sets after obtaining publicly available target versions and the required
  acceptance evidence.

## 11. References

- [Repository creation request](https://github.com/iflytek/community/issues/24)
- [Repository proposal template](TEMPLATE.md)
- [Community governance](../governance.md)
- [Contribution guide](../CONTRIBUTING.md)
- [Security policy](../SECURITY.md)
- Source/PoC review location: to be added to #24. The review materials include the
  project README, author specifications, compatibility and test guides, OWNERS,
  CONTRIBUTING, and CHANGELOG. No existing external repository transfer is
  requested, and no external adoption evidence is claimed.

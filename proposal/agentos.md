# Repository Proposal: agentos

## Metadata

- Target Repository Name: `agentos`
- Primary Authors: @XiaoSeS, @dongmucat
- Initial Maintainers: @XiaoSeS, @dongmucat (iflytek Org Members)
- Tracking Issue: https://github.com/iflytek/community/issues/26
- Owning Organization: Developer Ecosystem R&D Department, Consumer Open Platform Business Group (iFLYTEK)

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

AgentOS is an **enterprise collaboration runtime and governance platform**. It brings agents into the workflows an enterprise already runs: an employee, an agent and a business system work the same matter, and one place holds the record of what was decided and on what basis. Work is modeled as a **Case** — a durable record of one business matter — instead of a conversation; the process that advances a Case is an **immutable, versioned Scenario Pack**; and every action an agent or a person takes is authorized against the current actor, the assignment and an execution lease before it is written. Target users are platform and IT teams inside an organization who already run agents against internal systems and hit the same three walls: nothing auditable survives the run, authority is granted to a model instead of a person, and changing a process silently rewrites the meaning of work already in flight. AgentOS answers those with a Hub that owns business facts, a workflow worker that makes waiting and recovery durable, and a runtime manager that executes central tasks and returns evidence rather than conclusions. It ships as five deployables, a shared contract package generating a 157-path OpenAPI 3.1 document, and a self-hosting path on systemd with PostgreSQL — no hosted service is required.

## 2. Motivation

### 2.1 Pain Points

Agent frameworks in the open source ecosystem are strong at the loop — plan, call a tool, observe, repeat — and silent about what happens to the organization afterwards. The gap is not model capability; it is the absence of a fact model and an authority model.

| Current approach | Cost |
|---|---|
| Conversation-centric agent framework | The transcript is the only record. There is no object a manager can approve, no state an auditor can query, and no way to resume work after a restart |
| Agent holds a broad API token | The model becomes the authority. A prompt injection is indistinguishable from an instruction, and revoking access means revoking the whole run |
| Process logic hard-coded in prompts or code | Changing a step changes the meaning of every in-flight case, with no version boundary to explain the past |
| Memory written straight from the model | Unvetted statements become "facts"; scope, sensitivity and review status are lost |
| Workflow engine alone (Temporal, Camunda, etc.) | Durable execution and recoverability are solved, but business facts, approvals, artifact lineage and external-write authorization are not |

AgentOS was built to sit under existing systems rather than replace them: the enterprise keeps its CRM, ticket system and identity provider, and gains a layer that records what was decided, by whom, under which authorization, with which process version.

### 2.2 Use Cases

1. **Delegating a research or drafting task to an agent, with a human decision in the middle.** An operator opens a Case, a Scenario Pack collects inputs, the runtime manager executes the stage, a person approves the result, and the approved artifact is what leaves the system. Every step is queryable and every approval names a real actor.
2. **Multi-person collection with a deadline.** A Case fans out tasks to named assignees, waits on a workflow timer, escalates by deadline policy, and closes with a per-person completion record instead of a spreadsheet.
3. **Composing business processes without redeploying.** A team publishes a new Scenario Pack version and mounts it on a project; existing Cases keep running on the version they were bound to, and the platform does not restart.
4. **Self-hosted private deployment.** A customer installs the five services on a single host with systemd, points them at PostgreSQL, and runs the same protocol that its independently released clients use.

### 2.3 Why a Separate Repository?

- **Independent release cycle.** Five deployables (Hub API, Workflow Worker, Runtime Manager, Control Web, docs site) plus thirteen shared packages version and deploy together, on a cadence that has nothing to do with any other iflytek repository.
- **It owns a public protocol.** The employee-workbench API (`studio` paths) and the cross-scenario call contract are consumed by clients that are released independently. That compatibility surface needs its own fixture baselines, versioned deprecation policy and review gate; burying it inside another repository would make those gates unenforceable.
- **Distinct security posture.** Approvals, execution leases, signed artifact URLs, external-write authorization and idempotency boundaries are the product, not a side concern. Secret scanning, dependency-license gating and architecture guards are easier to audit in isolation.
- **Clear domain boundary.** AgentOS is a governance and runtime substrate; it does not extend any existing iflytek repository, and business processes live in separate Scenario Pack repositories rather than here.

## 3. Scope & Goals

### 3.1 Primary Goals

1. A Case-first fact model in which Case, Task, Approval, Artifact, authorization and audit are stored by one authority (Hub), and model output cannot write business facts or grant itself authority.
2. Immutable, versioned Scenario Packs: an existing Case is always interpreted by the pack version it was bound to; the current repository state never silently replaces a historical contract.
3. Explicit authority separation — visibility is not permission. Approvals check the current actor, task operations check assignee, assignment and execution lease, external writes check a precise authorization and an idempotency boundary.
4. Durable, recoverable process execution on Temporal, including deadline policy, fan-out and reconciliation after restart.
5. Governed long-term memory: a single `MemoryItem` authority, scoped to project, organization or none, with extraction and retrieval kept replaceable and injection funnelled through a context bundle.
6. A self-hostable distribution: systemd units, PostgreSQL-backed persistence, an S3-compatible artifact store, and a lightweight local mode (embedded PGlite, noop workflow) that runs the Hub and console without external services.
7. A published protocol: contract-first Zod definitions generating an OpenAPI 3.1 document (157 paths) with route-parity and conformance tests.

### 3.2 Non-Goals

- **Not a model project.** No weights, training code or fine-tuning recipes. Model access is a provider adapter behind the runtime manager.
- **Not a chat product.** There is no general conversational assistant surface; the operator console is for governance and case handling.
- **Not a hosted multi-tenant service.** The repository ships software to self-host; operating a public SaaS is out of scope.
- **Not the client.** Workbench clients — the desktop client and the DSH plugin — live in separate repositories. This repository maintains the protocol they consume, together with historical-compatibility baselines.
- **Not a business-process library.** Scenario Packs are examples and extension points; customer and internal business processes are not shipped here. Scenario Pack contents are covered by a separate open-source effort owned by another team — this proposal covers the platform, the contract and the authoring toolchain only. The pack format is a stable extension point, so that work can proceed independently without changing this repository.
- **Not a replacement for a workflow engine.** Temporal remains the durable-execution substrate; AgentOS adds the fact, authority and artifact layers above it.

## 4. Architecture & Dependencies

### 4.1 Core Components

| Component | Path | Role |
|---|---|---|
| Control Web | `apps/control-web` | Next.js operator console: organization and project configuration, Scenario Pack mounting, Case and audit views. Holds only the browser session and calls the Hub with the user's identity |
| Hub API | `apps/hub-api` | Fastify service and single authority for business facts. Authenticates the actor, checks permission, executes domain transactions, serves read models, and hands work to orchestration and execution |
| Workflow Worker | `apps/workflow-worker` | Temporal worker running `caseWorkflow()`: waits for task results, approvals and recovery conditions, and advances the Case through activities. Also hosts the scheduled-job poller |
| Runtime Manager | `apps/runtime-manager` | Manages executions, leases, isolated workspaces and execution adapters; runs a Program or invokes a provider and returns results with trustworthy execution evidence |
| Docs site | `apps/docs-site` | Documentation build used to publish the guides and generated OpenAPI |
| Contracts | `packages/contracts` | Cross-process data definitions with runtime validation, the `studio-api` protocol, and OpenAPI 3.1 generation |
| Database | `packages/db` | Drizzle ORM schema, migrations and repositories over PostgreSQL, with embedded PGlite for the lightweight mode |
| Platform core | `packages/platform-core` | Generic domain rules and decisions kept out of the Hub, Worker and Runtime Manager |
| Policy engine | `packages/policy-engine` | Authorization evaluation for actions, resources and external writes |
| Memory service / client | `packages/memory-service`, `packages/memory-client` | The only path for long-term memory writes; extraction, indexing and retrieval stay behind this boundary |
| Action gateway | `packages/action-gateway` | External write authorization, idempotency and capability adapters |
| Scheduling | `packages/scheduling` | Deadline and schedule computation shared by Hub and Worker |
| Workflow shared | `packages/workflow-shared` | Types and helpers shared between Hub and Worker for activities and Case progress |
| Runtime profiles | `packages/runtime-profiles` | Provider and tool profiles describing what an execution may use |
| Shared UI | `packages/ui` | Console component library (shadcn/ui-based, attributed in `THIRD_PARTY_NOTICES.md`) |
| Scenario Pack compiler | `tools/scenario-pack-compiler` | Authoring toolchain: source, workflow JSON, schemas, skills and programs compiled into an importable, immutable archive |

Control flow: a client or operator calls the Hub, which records facts, evaluates permission and starts or signals a workflow. Temporal schedules the Worker, the Worker dispatches activities, and the Runtime Manager executes against authorized providers and tools. Evidence, artifacts and results return through the Hub, which remains the only writer of business facts.

### 4.2 Dependencies

- **Runtime**: Node.js ≥ 26 (the current LTS line) and pnpm 10.32. CI runs the declared floor, so the minimum is tested rather than assumed.
- **Persistence**: PostgreSQL (primary) or embedded PGlite for the lightweight mode; schema managed by versioned migrations.
- **Durable execution**: Temporal (server plus `@temporalio/{client,worker,workflow}`). The lightweight mode substitutes a noop workflow.
- **Artifact storage**: any S3-compatible object store (`@aws-sdk/client-s3`, presigned URLs).
- **Key libraries**: Fastify, Drizzle ORM, Zod, `jose` (JWT), Next.js, React, `@xyflow/react` (flow view), `nodemailer`.
- **Licenses**: production dependency tree is predominantly Apache-2.0 (136) and MIT (287), with ISC, BSD-2/3-Clause, 0BSD, MIT-0 and Unlicense. Two require explicit notice: `@img/sharp-libvips-*` (LGPL-3.0-or-later, unmodified platform-specific native binaries loaded through sharp) and `caniuse-lite` (CC-BY-4.0 data). Both are declared in `THIRD_PARTY_NOTICES.md`; a CI license allowlist enforces the policy for new dependencies.
- **Bundled third-party source**: `packages/ui` adapts shadcn/ui (MIT) component implementations, with the upstream license text retained in `THIRD_PARTY_NOTICES.md`.

## 5. Governance & Maintenance

- **Maintainers**: @XiaoSeS and @dongmucat, both iflytek org members and the original authors, committing to maintain the repository for at least two years.
- **Decision making**: lazy consensus on pull requests, with at least one maintainer approval required for merge and `main` protected against direct pushes. Changes to the security boundary — actor resolution, authority evaluation, external-write authorization, artifact URL signing, execution leases — require two maintainer approvals.
- **Branch model**: `main` (stable, no direct commits), `dev`, `feature/*`, `bugfix/*` and `hotfix/*`, with lower-case semantic naming; tags are `vMAJOR.MINOR.PATCH`.
- **Versioning and process contracts**: Scenario Pack contracts are immutable and version-bound; changes to the workbench protocol follow the versioned-API and deprecation rules documented in the repository, with fixtures for previous client behavior retained as the compatibility baseline.
- **Community growth**: labeled `good first issue` work in three areas that do not require deep protocol knowledge — documentation and translations, provider and tool adapters, and example Scenario Packs. Issue triage within one week; new contributors are pointed at the contribution guide, which documents branch naming, the CLA and the review expectations.
- **Exit strategy**: if active maintenance stops, maintainers will announce it in the README, stop accepting feature work, and either transfer maintainership through the community process or archive the repository with a migration note for self-hosters.

## 6. Release Policy

- **Versioning**: Semantic Versioning. The first public release is `1.0.0`; breaking changes to the published workbench protocol are released only through a versioned API or an explicit negotiation mechanism, never by silently changing existing endpoints.
- **Cadence**: monthly patch releases (second Wednesday) and quarterly minor releases; hotfixes as needed for security and data-integrity defects. Each release updates `CHANGELOG.md`.
- **Gate before release**: unit and integration tests, type checking, lint and formatting, architecture guards, documentation validation, OpenAPI parity, secret scan and dependency license check must all pass; the release commit is reviewed by a second maintainer.
- **Distribution**: GitHub Releases with changelogs and source archives. The repository is not published to npm — it is server software deployed from source, with systemd units and configuration templates in `infra/`.

## 7. CI/CD & Security

- **Tests**: 806 test files covering unit rules, repository and migration behavior, HTTP routes, contract conformance, protocol compatibility, fault injection, artifact lifecycle, rolling upgrade, PostgreSQL concurrency and operational runbooks.
- **CI (GitHub Actions, every push and pull request)**: frozen-lockfile install, type checking, ESLint, formatting, dependency-cruiser architecture guards, documentation validation, unit tests, protocol conformance and OpenAPI route parity.
- **Supply chain**: `gitleaks` secret scan over the full history, dependency license allowlist, SPDX header coverage check, and Dependabot for npm and Actions.
- **Security model**: short-lived JWT access tokens with opaque, rotatable refresh tokens; one-time administrator-issued binding tokens with session epochs for the workbench protocol; execution leases preventing concurrent task ownership; presigned, expiring artifact URLs; external writes gated by explicit authorization and idempotency keys. Credentials, full business text and signed URLs are kept out of logs and model prompts by construction and by test.
- **Vulnerability reporting**: GitHub private vulnerability reporting plus `security@iflytek.com`, with acknowledgement within five working days, as described in `SECURITY.md`.

## 8. Repository Configuration

- [x] Enable Issues
- [x] Enable Discussions
- [ ] Enable Wiki
- [ ] Transfer existing external repo (if applicable)

**License and ownership.** Apache License 2.0, with `Copyright 2026 iFlytek Co., Ltd.` recorded in `LICENSE` and `NOTICE`. The project's intellectual property is held by iFLYTEK; all contributors sign the community CLA before their changes can be merged. Third-party source and assets retain their own licenses, declared in `THIRD_PARTY_NOTICES.md`.

The public repository will be published as a **fresh initial commit**; internal development history is not transferred. Version history for users is maintained in `CHANGELOG.md`. Codes of conduct, CLA and security policy follow the community-wide documents under `iflytek/community`.

## 9. Risks & Alternatives

| Risk | Mitigation |
|---|---|
| Internal environment details (private hostnames, personal names, internal project identifiers) leak through test fixtures and documentation | Full-tree scan before publication with explicit replacement rules; `gitleaks` and a custom identifier check in CI; fresh initial commit so history cannot leak |
| Operational weight: Temporal and PostgreSQL raise the bar for first-time users | Lightweight local mode (embedded PGlite, noop workflow) runs Hub and console with two commands; the full topology is documented as an optional path with systemd units |
| Protocol compatibility burden across independently released clients | Immutable fixtures for historical client behavior, route-parity and conformance tests in CI, and a documented deprecation policy |
| Scope creep toward shipping business processes | Non-goal stated explicitly; only example packs live here, and the compiler keeps process authoring outside the core repository |
| Maintainer attrition | At least two committed maintainers, documented exit strategy, and issue triage responsibility assigned per module through `OWNERS` |
| A governance layer is easy to misread as "the model decides" | The authority model is documented in the architecture guide and enforced by tests that assert rejections *and* the absence of side effects |

**Alternatives considered**

- *Contribute into an existing iflytek repository*: the five deployables, the published protocol and the security boundary have no natural host; coupling them to another project's release cycle would break the compatibility guarantees clients depend on.
- *Ship as a plugin to an existing agent framework*: the core value is the fact and authority model *below* the agent loop. A plugin would inherit the host's transcript-as-record model and could not enforce leases, approvals or version-bound processes.
- *Personal or team-internal repository*: the project was built by an iflytek team and the IP is held by iFLYTEK; organizational hosting is what gives it the governance, CLA and security process external contributors require.

## 10. Timeline

The first public release is the finished product, not a preview: the repository opens at `v1.0.0` after the sanitization and standardization work is complete. There is no public alpha or beta stage, and no intermediate release that users would have to migrate away from.

| Milestone | Target | Exit criteria |
|---|---|---|
| Tracking issue filed | 2026-11 | Issue opened in `iflytek/community`; this proposal linked from it |
| Proposal review | 2026-11 | Reviewed by the community maintainers and the owning team; no open objections on the tracking issue |
| Sanitization and standardization | 2026-12 | Internal identifiers removed; `LICENSE`, `NOTICE`, `OWNERS`, `CHANGELOG.md`, `CODE_OF_CONDUCT.md` and both READMEs in place; SPDX headers applied; CI, secret scan and license gates green |
| Self-hosting rehearsal | 2026-12 | `docs/operations/deployment.md` executed end to end on a clean host with PostgreSQL, Temporal and an S3-compatible store; no undocumented steps |
| **First public release (`v1.0.0`)** | 2026-12 | Clean initial commit containing the adjusted codebase; tagged `v1.0.0`; release notes and `CHANGELOG.md` published |

After `v1.0.0`, releases follow the policy in section 6: monthly patch releases and quarterly minor releases.

## 11. References

- Tracking issue: https://github.com/iflytek/community/issues/26
- Architecture and code map: `docs/architecture.md`
- Governance and authority model: `docs/governance.md`
- Scenario Pack authoring: `docs/scenarios.md`
- Workbench protocol: `docs/api/README.md`, `docs/guides/astron-studio-openapi.json`
- Self-hosting: `docs/operations/deployment.md`
- Testing and verification boundaries: `docs/testing.md`
- Third-party notices: `THIRD_PARTY_NOTICES.md`

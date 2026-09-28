# Repository Proposal: dreamflash

## Metadata
- Target Repository Name: dreamflash
- Primary Authors: @HuntercodeT, @ShuiYidi, @jczeal, @xianshujun
- Initial Maintainers: @HuntercodeT, @ShuiYidi, @jczeal, @xianshujun (iflytek Org Members)
- Tracking Issue: https://github.com/iflytek/community/issues/22

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

DreamFlash turns visual judgement about a running web page into reviewable code
changes. A developer points at a component in their live Vite app; DreamFlash
resolves it to its source file and line, generates several renderable redesign
options from that precise context, lets the user try one on by dragging it onto the
page, and writes the chosen option back to the real files with HMR, a unified diff
and multi-step undo. Its first users are frontend, full-stack and independent
developers who maintain an existing web project and own its code, but have not yet
settled on a visual answer; designers and product owners join as reviewers who can
point at problems and approve a direction. The core value is that **the plan the
user previews, the plan they compare, and the plan written to disk are the same
object**, and the model never writes code: it returns a constrained design spec that
a local planner compiles into edits limited to the files the component already uses.

## 2. Motivation

### 2.1 Pain Points

"This hero section looks dated" is a complete and correct judgement, but acting on
it requires translating it into "`HeroSection` in `src/components/home/HeroSection.tsx`,
change the palette and type scale in `hero.module.css`". Today that translation is
expensive:

| Current approach | Cost |
|---|---|
| Describe the problem to an AI assistant in prose | The model guesses the target: wrong component, collateral layout breakage, and no clear record of what changed |
| File a ticket with a frontend engineer | One round of communication, scheduling and review for a border-radius tweak |
| Redesign in a design tool, then port by hand | Two sources of truth that drift apart on every iteration |

AI coding assistants are strong at "change this function" and weak at "this page
looks wrong". The gap is not model capability but context: prose cannot reliably
identify which component on a running page the user means, and a model without
geometry, computed styles and the real source slice produces changes that land in
the wrong file.

We found no open source project that uses **visual selection in a running dev
server** as the context-acquisition mechanism for code generation, with a verified
same-plan path from preview to disk. Adjacent projects are design-to-code exporters
(one-way, no existing codebase), DevTools-style inspectors (no generation, no
write-back), or prose-driven assistants (no selection).

### 2.2 Use Cases

1. **Refreshing a dated product page.** A developer opens their marketing site,
   clicks the hero, compares five directions rendered on their own component,
   applies one, reviews the diff and commits it.
2. **Cross-role design review.** A product manager points at a section they dislike
   and picks between options on the real page, instead of describing the problem
   in a ticket; the developer receives a reviewable diff rather than a screenshot.
3. **Constrained iteration with an AI assistant.** A developer tells the built-in
   assistant "tone it down, two or three colors at most"; the request is injected
   into the same constrained generation path, and every result is still a
   previewable, undoable change.
4. **Starting from an existing public page.** (Experimental) A user rebuilds a
   public page they are authorized to use into an editable Vite working copy, then
   iterates on it with the same loop.

### 2.3 Why a Separate Repository?

- **Clear domain boundary.** DreamFlash is a self-contained developer tool
  (workbench UI, local bridge service, Vite plugin, model proxy) and does not extend
  any existing iflytek repository.
- **Independent release cycle.** It ships as an npm workspace with its own
  publishable Vite plugin and versioning.
- **Independent maintainers.** The four authors who built it will maintain it.
- **Distinct security posture.** It runs a local service that installs and runs
  projects and writes files; its trust model and CI gates (secret scan, license
  gate) are specific to it and are easier to audit in isolation.

## 3. Scope & Goals

### 3.1 Primary Goals

1. Component-level selection with source mapping for React and Vue Vite projects,
   without modifying user source files (injection lives in a removable `.od/`
   directory).
2. Constrained, multi-option redesign generation from precise selection context,
   degrading to local candidates when no model is configured.
3. A single verified change plan shared by preview, comparison and write-back,
   gated by `previewId` + `planHash`.
4. A reviewable change ledger: unified diff, before/after comparison, multi-step
   undo/redo, project export.
5. Model-agnostic operation over any OpenAI-compatible endpoint, including local
   or self-hosted models.
6. English localization of the UI and public API documentation.

### 3.2 Non-Goals

- **Not a design tool or page builder.** DreamFlash edits existing projects; it
  does not create pages from scratch or replace Figma-class tools.
- **Not a general code assistant.** The model never emits CSS, code or file paths.
- **Not a website cloner.** The experimental URL entry produces an editable working
  copy of a single public page the user is authorized to use; it does not fetch
  original repositories, login state, backends or business logic, and is not an
  internet-facing crawling service.
- **Not a hosted multi-tenant service.** It is a local developer tool; running it
  as a shared network service is out of scope.
- No frameworks beyond Vite-based React and Vue in the initial scope.

## 4. Architecture & Dependencies

### 4.1 Core Components

| Component | Path | Role |
|---|---|---|
| Workbench | `apps/workbench` | React 19 UI: canvas iframe, selection, recommendation cards, drag-to-apply, diff and history |
| Local Bridge | `apps/local-bridge` | Local service on `127.0.0.1:8787`: import (ZIP / demo / URL), install, run, source resolution, change planning, write-back, backups, undo/redo |
| Vite plugin | `packages/vite-plugin-dreamflash` | Source instrumentation of React JSX roots and Vue SFC template roots; ships a standalone file injected into target projects |
| Inspector runtime | `packages/inspector-runtime` | IIFE injected into the target page for hover, selection and overlays |
| Contracts | `packages/contracts` | Shared types (`SelectionContext`, `DesignSpec`, `ChangePlan`) and the effect catalog |
| Recommendation renderer | `packages/recommendation-renderer` | Preview components for recommendation cards |
| Model proxy | `server/` | Local service on `127.0.0.1:8788`: dual-track constrained generation over an OpenAI-compatible endpoint |

Data flow: selection in the page → the bridge resolves a `SelectionContext` and a
deterministic `ChangeScope` → the model proxy returns `DesignSpec`s → the bridge
compiles each into a `ChangePlan` and a preview → on apply, the verified plan is
written to disk and Vite HMR updates the canvas.

### 4.2 Dependencies

- **Runtime**: Node.js ≥ 22.6, npm. Tested in CI on Node 22 and 24.
- **Key libraries**: `@babel/parser`, `@babel/traverse`, `magic-string` (source
  instrumentation); `adm-zip`, `diff` (bridge); React 19, `motion`, `lucide-react`
  (UI); Vite and Tailwind CSS (build).
- **Licenses**: all dependencies are MIT, ISC, BSD-3-Clause, 0BSD or Apache-2.0,
  plus MPL-2.0 `lightningcss` as an unmodified transitive build dependency.
  Enforced in CI by a license allowlist.
- **Optional external services**: any OpenAI-compatible model endpoint (default
  OpenRouter); a Ditto API for the experimental URL entry. Neither is required to
  run the core loop.
- **Bundled third-party material**: the effect catalog includes MIT-licensed
  material from uiverse-io/galaxy, animate.css v3.7.2, css-loaders and SpinKit,
  with per-entry attribution and full license texts in `THIRD_PARTY_NOTICES.md`.

## 5. Governance & Maintenance

- **Maintainers**: @HuntercodeT, @ShuiYidi, @jczeal, @xianshujun, all iflytek org
  members and the original authors, committing to maintain the project for at
  least two years.
- **Decision making**: lazy consensus among maintainers on pull requests; changes
  to the security boundary (origin/host policy, path confinement, write gating)
  require review by two maintainers.
- **Community growth**: `good first issue` labels on localization, framework
  adapters and effect catalog entries; English README, CONTRIBUTING guide and
  explicit conventions for new contributors; issue triage within one week.
- **Exit strategy**: if active maintenance stops, maintainers will announce it in
  the README, stop accepting feature work, and either transfer maintainership to
  willing contributors through the community process or archive the repository.

## 6. Release Policy

- **Versioning**: Semantic Versioning. `0.x` while the design-spec schema and
  write-back marker format may still change; `1.0.0` once they are stable.
- **Cadence**: as needed during `0.x`; roughly monthly thereafter.
- **Distribution**: GitHub Releases with changelogs; `vite-plugin-dreamflash`
  published to npm once its standalone API is stable.

## 7. CI/CD & Security

- **Tests**: 158 unit tests across the bridge, workbench, inspector runtime and Vite
  plugin, 60 model-proxy contract tests, and 6 executable self-check suites
  covering session state, diff, path confinement, selection promotion,
  recommendation and hover language.
- **CI** (GitHub Actions, every push and pull request): build, typecheck, unit
  tests and self-checks on Node 22 and 24; lint; gitleaks secret scan over full
  history; dependency license allowlist. Dependabot for npm and Actions.
- **Security model**: bridge and model proxy bind to loopback only; every request
  is checked against a `Host` allowlist (DNS rebinding) and an `Origin` allowlist
  (CSRF); paths from target projects are confined to the project root; writes
  require a verified preview. Documented in `SECURITY.md`, including known
  limitations (project code executes on install, no inter-process authentication).
- **Vulnerability reporting**: GitHub private vulnerability reporting and
  security@iflytek.com, per the organization policy.

## 8. Repository Configuration
- [x] Enable Issues
- [x] Enable Discussions
- [ ] Enable Wiki
- [ ] Transfer existing external repo (if applicable)

The code will be published as a fresh initial commit; internal development history
is not transferred.

## 9. Risks & Alternatives

| Risk | Mitigation |
|---|---|
| Opening a project runs its `npm install` and dev server, which executes untrusted code | Documented prominently in README and SECURITY.md; the tool is scoped to projects the user already trusts |
| Local service could be driven by malicious web pages | Loopback binding, Host and Origin allowlists on both services, preview-gated writes; covered by regression tests |
| Third-party material in the effect catalog | Per-entry attribution, MIT-only sources, full license texts, contribution rule requiring compatible licenses |
| Remote models receive project source context | Bounded, allowlisted context; documented; local or self-hosted endpoints supported |
| Experimental URL entry misused as a crawler | Single-page scope, address checks, marked experimental; documented as not an SSRF boundary |
| Maintainer attrition | Four maintainers; exit strategy above |

**Alternatives considered**

- *Contributing to an existing repository*: no iflytek repository covers
  selection-driven frontend code generation; merging it elsewhere would couple
  unrelated release cycles and security reviews.
- *Shipping as a plugin to an IDE or AI assistant*: the core value depends on a
  live canvas, in-page selection and a local bridge that installs and runs
  projects, which do not fit an editor plugin surface.
- *Personal repository*: the project was built by an iflytek team, IP is held by
  iFLYTEK, and organizational hosting gives it the governance and security process
  it needs.

## 10. Timeline
- Proposal review: 2026-09
- Alpha development: v0.1.0 public release after the proposal is accepted (target 2026-10)
- Beta/GA milestones: English UI and published Vite plugin in v0.2 (target 2026-Q4); v1.0 once the design-spec schema and write-back format are frozen (target 2027-H1)

## 11. References
- Tracking issue: https://github.com/iflytek/community/issues/22
- Repository: https://github.com/iflytek/dreamflash
- Upstream for the experimental URL entry: https://github.com/ion-design/ditto.site

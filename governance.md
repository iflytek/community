# Community Governance

<!-- toc -->
- [Principles](#principles)
- [Code of Conduct](#code-of-conduct)
- [Values](#values)
- [Community Membership](#community-membership)
- [Community Groups](#community-groups)
  - [SIGs](#sigs)
    - [Subprojects](#subprojects)
  - [Working Groups](#working-groups)
  - [Committees](#committees)
- [Cross-project Communication and Coordination](#cross-project-communication-and-coordination)
- [Repository Guidelines](#repository-guidelines)
- [CLA](#cla)
<!-- /toc -->

This document describes the governance structure for the Iflytek Opensource Community.

## Principles

The Iflytek Opensource Community adheres to the following principles:

* **Open**: Our projects are open source. See repository guidelines and CLA below.
* **Welcoming and respectful**: See [Code of Conduct](code-of-conduct.md).
* **Transparent and accessible**: Work and collaboration should be done in public.
* **Merit**: Ideas and contributions are accepted according to their technical merit and alignment with project objectives and scope.

## Code of Conduct

The community abides by the [Code of Conduct](code-of-conduct.md). Here is an excerpt:

> As contributors and maintainers of this project, and in the interest of fostering an open and welcoming community, we pledge to respect all people who contribute through reporting issues, posting feature requests, updating documentation, submitting pull requests or patches, and other activities.

We value our community tremendously and aim to cultivate a friendly and collaborative environment for all contributors and users.

## Values

Our community values are documented in [values.md](values.md).

## Community Membership

See the [community membership](community-membership.md) document for details on membership levels and responsibilities.

## Community Groups

The project is organized into the following types of groups:

* **Special Interest Groups (SIGs)**
  * Subprojects
* **Working Groups (WGs)**
* **Committees**

### SIGs

The community is organized primarily into Special Interest Groups, or SIGs. Each SIG is comprised of members from multiple companies and organizations, with a common purpose of advancing the project with respect to a specific topic, such as Networking or Documentation.

Our goal is to enable a distributed decision structure and code ownership, as well as providing focused forums for getting work done, making decisions, and onboarding new contributors. Every identifiable subpart of the project (e.g., GitHub org, repository, subdirectory, API, test, issue, PR) is intended to be owned by some SIG.

SIGs must have at least one and ideally two SIG chairs at any given time. SIG chairs are organizers and facilitators, responsible for the operation of the SIG and for communication and coordination with other SIGs and the broader community.

Each SIG must have a charter that specifies its scope (topics, subsystems, code repos and directories), responsibilities, areas of authority, how members and roles of authority/leadership are selected, how decisions are made, and how conflicts are resolved.

A primary reason that SIGs exist is as forums for collaboration. Much work in a SIG should stay local within that SIG. However, SIGs must communicate in the open, ensure other SIGs and community members can find notes of meetings, discussions, designs, and decisions, and periodically communicate a high-level summary of the SIG's work to the community.

#### Subprojects

Specific work efforts within SIGs are divided into **subprojects**. Every part of the code and documentation must be owned by some subproject. Subproject owners act as technical leaders: responsible for vision and direction, overall design, and approving changes.

### Working Groups

Working groups are formed to address issues that cross SIG boundaries. They are temporary groups used to facilitate topics of discussion that are in scope for the project but span multiple SIGs. Working groups do not own any code or other long-term artifacts. They can report back and act through involved SIGs.

### Committees

Some topics, such as Security or Code of Conduct, require discretion. Whereas SIGs are voluntary groups which operate in the open and anyone can join, committees do not have open membership and do not always operate in the open.

The chief maintainers can form committees as needed, for bounded or unbounded duration. Membership of a committee is decided by the chief maintainers. Like a SIG, a committee has a charter and a chair, and will report periodically.

## Cross-project Communication and Coordination

While most work shouldn't require expensive coordination with other SIGs, there will be efforts (features, refactoring, etc.) that cross SIG boundaries. In this case, it is expected that the SIGs coordinate with each other and come to mutually agreed solutions. In some cases, it may make sense to form a Working Group for joint work.

On the other hand, several SIGs may have project-wide impact, for example Release, Testing, or Architecture. Even those that do not may sometimes need to make changes or impose new processes or conventions that affect other SIGs. In these cases, project-wide communication processes will need to be followed.

## Repository Guidelines

All new repositories under the community GitHub organization should follow standard project conventions and be approved by the chief maintainers.

## CLA

All contributors must sign the CLA, as described in [CLA.md](CLA.md).

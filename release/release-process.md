# Release Process

This document describes the release process for the Iflytek Opensource Community.

<!-- toc -->
- [Overview](#overview)
- [Release Cycle](#release-cycle)
- [Release Steps](#release-steps)
  - [Preparation](#preparation)
  - [Create Release Branch](#create-release-branch)
  - [Tag the Release](#tag-the-release)
  - [Publish Release Notes](#publish-release-notes)
- [Patch Releases](#patch-releases)
  - [When to Create a Patch Release](#when-to-create-a-patch-release)
  - [Patch Release Process](#patch-release-process)
<!-- /toc -->

## Overview

Each release is built from the `main` branch and tagged with a version number following the [versioning scheme](release-versioning.md).

## Release Cycle

Releases are made on a regular schedule, approximately every **3 months**. The release process includes:

1. **Planning** — Define scope and features for the upcoming release
2. **Development** — Implement features and fixes on `main`
3. **Stabilization** — Focus on bug fixes and documentation
4. **Release** — Tag, publish, and announce

## Release Steps

### Preparation

1. Create a tracking issue for the release
2. Announce the release timeline to the community
3. Ensure all planned features and fixes are merged
4. Update documentation and release notes

### Create Release Branch

When the code is ready for release:

1. Create a release branch from `main`:
   ```bash
   git checkout -b release-1.0 main
   ```
2. Push the branch to the remote:
   ```bash
   git push origin release-1.0
   ```

### Tag the Release

1. Switch to the release branch:
   ```bash
   git checkout release-1.0
   ```
2. Create an annotated tag:
   ```bash
   git tag -a v1.0.0 -m "Release v1.0.0"
   ```
3. Push the tag:
   ```bash
   git push origin v1.0.0
   ```

### Publish Release Notes

1. Create a GitHub Release from the tag
2. Include a changelog of significant changes
3. Announce the release to the community

## Patch Releases

Patch releases are made from the corresponding release branch and contain only bug fixes.

### When to Create a Patch Release

- Critical bug fixes
- Security fixes
- Documentation corrections

### Patch Release Process

1. Cherry-pick or merge fixes to the release branch
2. Update the VERSION file
3. Create a new patch tag:
   ```bash
   git tag -a v1.0.1 -m "Release v1.0.1"
   ```
4. Push the tag and publish the release

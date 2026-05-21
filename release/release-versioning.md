# Release Versioning

<!-- toc -->
- [Semantic Versioning](#semantic-versioning)
- [Release Tags](#release-tags)
- [Pre-release Versions](#pre-release-versions)
- [Release Branches](#release-branches)
- [Supported Versions](#supported-versions)
<!-- /toc -->

This document describes the versioning scheme used by the Iflytek Opensource Community.

## Semantic Versioning

All releases follow [Semantic Versioning](https://semver.org/) (SemVer) format:

```
MAJOR.MINOR.PATCH
```

- **MAJOR**: Incompatible API or breaking changes
- **MINOR**: New features, backward compatible
- **PATCH**: Bug fixes, backward compatible

## Release Tags

Release tags follow the format `vMAJOR.MINOR.PATCH`:

- `v1.0.0` — Initial stable release
- `v1.1.0` — Minor release with new features
- `v1.0.1` — Patch release with bug fixes

## Pre-release Versions

Pre-release versions are suffixed with a hyphen and identifier:

- `v1.0.0-alpha.1` — First alpha release
- `v1.0.0-beta.1` — First beta release
- `v1.0.0-rc.1` — First release candidate

## Release Branches

Each major.minor release has a corresponding release branch:

- `release-1.0` — Branch for v1.0.x releases
- `release-1.1` — Branch for v1.1.x releases

The `main` branch tracks the latest development version.

## Supported Versions

The community typically supports the latest **3** minor releases with patch updates. Older versions are considered end-of-life and will not receive updates.

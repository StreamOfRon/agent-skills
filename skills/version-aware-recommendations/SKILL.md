---
name: version-aware-recommendations
description: Use when recommending, installing, deploying, or pinning software versions, or listing features and capabilities of any external software, library, or tool
---

# Version-Aware Recommendations

**Core rule: never recommend a version or feature set from internal memory. Always verify against current, external sources.**

Agent memory of software versions drifts within months. Feature sets change, major releases land, and "latest stable" becomes wrong. Every version recommendation must be grounded in a live lookup.

## When to Use

- Recommending a language runtime, framework, library, or tool version
- Installing or deploying software (apt, pip, npm, docker, homebrew, etc.)
- Pinning a dependency to a specific version
- Listing features, capabilities, or requirements of external software
- Comparing software options ("Which database should we use?")
- Setting up CI/CD toolchains, base images, or SDK targets

**NOT needed for:** project-internal versions (your own app version, internal monorepo packages).

## Procedure

1. **Identify the software** — name the exact package, image, or tool.
2. **Search the official source** — use `web_search` or `read` the official URL. Primary sources: project website, GitHub/GitLab releases, Docker Hub, package registry (PyPI, npm, crates.io, Maven), Homebrew formula.
3. **Determine latest stable** — filter out alpha/beta/RC/pre-release tags. Identify the latest stable release version.
4. **Verify current documentation** — read the current docs for the identified version. Features, defaults, and deprecations change; do not rely on memory.
5. **Recommend with evidence** — state the version and link to the source where you found it. Include the feature set from current docs, not memory.

### Pre-release Exception

**Default to stable.** Pre-releases (alpha, beta, RC, nightly, canary, -dev, -preview) are rejected unless one exception applies:

- **Required feature gap:** A specific feature or bug fix exists only in a pre-release and is required by the current requirements. State which feature and why stable is insufficient.
- **No recent stable:** The project has no stable release in the last 12 months, or is early-stage / abandoned. State the project's release history and why pre-release is the best available option.

If an exception applies, the agent must explicitly state:
1. Which exception applies
2. Why stable is not suitable
3. The pre-release version being recommended
4. The risk of depending on pre-release software

### Version Pinning

When pinning to a non-latest version, document the rationale:

| Rationale | Example |
|-----------|---------|
| Compatibility constraint | "Pinned to v3.2 because v4.0 drops support for Node < 18" |
| Known regression | "Avoid v2.7.1 — upstream issue #1234 breaks our use case; v2.7.0 works" |
| Reproducibility | "Pinned to v1.4.3 to match existing CI and production" |
| EOL / security | "Pinned to v5.x LTS; v6.x is unsupported for our platform" |

**Never pin silently.** The pin and its reason must be visible in code comments, lock files, or documentation.

## Pitfalls

- **Memory drift:** "Python 3.11 is latest" — wrong; 3.12 or 3.13 may be current. Always look up.
- **Feature hallucination:** Listing capabilities from outdated memory. Read the current docs.
- **Pre-release as stable:** "v2.0.0-rc.1" is not stable. Filter pre-releases unless an exception applies.
- **Silent pinning:** Pinning `flask==2.2.0` without explaining why. Future agents (or you) will not know the constraint.
- **Partial verification:** Looking up the version but guessing features. Both version AND feature set need live verification.
- **Secondary sources:** Blog posts, Stack Overflow, or third-party articles may be outdated. Always confirm against the official source.

## Verification

After making a recommendation, confirm:
- The version number matches what's on the official source (link included)
- Features listed match current documentation, not memory
- If pre-release: exception is stated with justification
- If pinned: rationale is documented inline

Quick check before proceeding:
```
Is this version number from a live lookup? If no → stop, search.
Am I listing features from memory? If yes → stop, read docs.
Is this a pre-release without documented justification? If yes → stop, either find stable or document exception.
Is this pin documented with a reason? If no → stop, add rationale.
```

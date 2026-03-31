# ADR-0002: Trunk-Based Development Branching Strategy

**Date:** 2026-03-31
**Status:** Accepted
**Deciders:** Development Team
**Technical Area:** Infrastructure / Branching Strategy

## Context

As a small, fast-moving team working in a monorepo, we need a simple and efficient branching strategy. Our goal is to maximize developer productivity, reduce merge complexity, and ensure our main branch is always in a releasable state. We need to formalize the process outlined in `CONTRIBUTING.md` to ensure a single source of truth and rapid integration of new code.

## Decision

We will adopt a **Trunk-Based Development (TBD)** strategy.

1.  **The Trunk:** The `main` branch is the single source of truth, or "the trunk". It must always be stable and releasable.
2.  **Direct Commits:** For very small, trivial changes, developers can commit directly to `main`.
3.  **Short-Lived Feature Branches:** For most work (features, bug fixes), developers will create short-lived branches from `main`. These branches should be merged back into `main` via a Pull Request (PR) as quickly as possible, ideally within 1-2 days.
4.  **Pull Requests for Review:** All feature branches must be merged into `main` through a PR to facilitate code review and automated checks.
5.  **Feature Flags:** Incomplete features merged into `main` must be hidden behind feature flags to ensure the trunk remains releasable.

This decision formalizes our process to be more aligned with TBD principles, prioritizing a healthy trunk over long-running feature branches.

## Alternatives Considered

### Alternative 1: Git Flow

**Description:** A more complex model with long-lived branches like `develop`, `main`, and supporting branches for `feature`, `release`, and `hotfix`.

**Pros:**
- Provides a very structured process for scheduled releases.
- Excellent for projects with long release cycles and a need to support multiple versions in production.

**Cons:**
- Unnecessary overhead for a small team that aims to practice continuous delivery.
- The `develop` branch can diverge significantly from `main`, leading to "merge hell".
- Slower integration of features into the main codebase.

**Why not chosen:** Git Flow's complexity is not justified for our project's current stage and size. It would slow down our development velocity.

### Alternative 2: GitHub Flow

**Description:** A simpler model where developers create feature branches from `main` and merge them back after review. This is the model we have been loosely following.

**Pros:**
- Simple to understand and use.
- Supports continuous delivery and frequent releases.

**Cons:**
- Less strict about the lifespan of feature branches, which can lead to them becoming long-running and difficult to merge.

**Why not chosen:** While very similar to our chosen approach, we are explicitly adopting the "Trunk-Based Development" name and philosophy to emphasize the importance of keeping branches extremely short-lived and `main` as the central point of development. This is more of a refinement than a rejection of GitHub Flow.

## Consequences

### Positive
- **Reduced Merge Conflicts:** With all developers working off a single, up-to-date `main` branch, merge conflicts become infrequent and easier to resolve.
- **Continuous Integration & Delivery:** This model is a core enabler of CI/CD, as every commit to `main` can potentially be a new release.
- **Simplicity:** The branching model is extremely simple, reducing cognitive overhead for the team.
- **Code Visibility:** All work is visible on the trunk, improving team collaboration.

### Negative
- **Requires Discipline:** The team must be disciplined about keeping `main` stable. A broken trunk blocks everyone.
- **Heavy Reliance on Automation:** Requires a robust and fast automated test suite to catch bugs before they are merged into the trunk.
- **Need for Feature Flags:** To keep `main` releasable, incomplete features must be hidden behind feature flags, which adds some complexity to the application code.

## Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| `main` branch becomes unstable or broken | Medium | High | - Enforce mandatory CI checks (build, lint, test) on all PRs before merging.<br>- Code review process to catch issues.<br>- Encourage developers to run tests locally before pushing. |
| Incomplete features are released to users | Low | High | Implement a robust feature flag system and ensure it is used for any user-facing feature that takes more than a day to complete. |

## Review Date

**Next review:** After 6 months of development.
This decision should be revisited if the team grows significantly (e.g., to multiple sub-teams) or if our release process requires supporting multiple versions simultaneously.

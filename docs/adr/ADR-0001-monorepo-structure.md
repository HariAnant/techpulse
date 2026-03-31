# ADR-0001: Monorepo Structure for TechPulse Project

**Date:** 2026-03-27
**Status:** Accepted
**Deciders:** Development Team (Sprint 1 — Day 1 decision)
**Technical Area:** Infrastructure / Repository Architecture

## Context

We are starting a new full-stack project named "TechPulse" which will consist of three main components: a web frontend, a backend API, and shared utilities/types that are common to both.

The team currently consists of 3-5 developers, and the project is in its early stages. We need to decide how to organize the code — whether to keep everything in a single repository (monorepo), create separate repositories for each component (polyrepo), or choose a hybrid approach.

This decision is important because:
- Shared code (types, validation logic, constants) will be used in both the web and API components.
- For a small team, managing too many repositories is an overhead.
- The initial CI/CD pipeline setup is simpler for a single repository.
- We also need to consider scalability as the team grows in the future.

## Decision

We will adopt a **monorepo structure** with the following layout:

```
techpulse/
├── apps/
│   ├── web/          # React/Next.js frontend
│   └── api/          # Node.js/Express backend API
├── packages/
│   └── shared/       # Shared types, utils, constants
├── .gitignore
├── .github/
│   └── PULL_REQUEST_TEMPLATE.md
├── CONTRIBUTING.md
└── README.md
```

For now, we will not use a dedicated monorepo tool (like Nx or Turborepo). We will start with a plain folder structure. We will add tooling when build times or dependency management become painful.

## Alternatives Considered

### Alternative 1: Polyrepo (Separate Repositories)

**Description:** A separate GitHub repository for each component — `techpulse-web`, `techpulse-api`, `techpulse-shared`. The shared code would be published as an npm package and installed in both repositories.

**Pros:**
- Each repository has independent versioning and deployment.
- Access control is granular — frontend developers don't need access to the backend repository.
- The CI/CD pipeline runs only for the relevant code; other repositories are not affected.

**Cons:**
- Making changes to shared code becomes painful — it requires a PR in the shared repo, publishing the package, and then bumping the version in both consumer repos.
- Managing 3 repositories for 3-5 developers is unnecessary overhead.
- Cross-repository changes require coordinating multiple PRs, which is slow for a small team.
- The local development setup requires using `npm link` or similar hacks.

**Why not chosen:** The team is small, and the shared code will change frequently in the early stages. Creating 3 PRs for every shared change in a polyrepo would drastically slow down development speed.

### Alternative 2: Monorepo with Nx/Turborepo Tooling

**Description:** A monorepo structure with dedicated tooling like Nx or Turborepo, which provides intelligent caching, affected-only builds, and dependency graph management.

**Pros:**
- Build times are significantly reduced due to smart caching.
- Commands like `nx affected` only test/build the code that has changed.
- Built-in code generation and scaffolding.
- Provides dependency graph visualization and enforcement.

**Cons:**
- There is a learning curve — the team would need to learn the configuration for Nx/Turborepo.
- The project is currently too small to justify the tooling overhead.
- Adds configuration complexity (`nx.json`, `turbo.json`, workspace configs).
- Debugging build issues requires understanding the extra layer of tooling.

**Why not chosen:** The project currently has only 2 apps and 1 shared package. The real benefit of Nx/Turborepo comes when there are 5+ packages and build times are in minutes. We can add this in the future without major restructuring — we are keeping this option open.

### Alternative 3: Git Submodules

**Description:** Maintain separate repos but include `techpulse-shared` as a git submodule in the web and api repos.

**Pros:**
- Retains the benefit of independent repositories.
- Shared code is maintained in one place.

**Cons:**
- Git submodules are notoriously confusing — forgetting to update the submodule after a `git pull` is a common mistake.
- The setup is painful for new developers.
- Merge conflicts in submodule pointer changes are tricky.
- Submodules are also generally avoided in the industry for application code.

**Why not chosen:** The complexity of Git submodules is unjustified for a small team. It also inherits all the downsides of a polyrepo with added git complexity.

## Consequences

### Positive
- A change in the shared code (`packages/shared/`) is reflected for both the web and api in a single PR — no version bump dance.
- A new developer only needs to clone one repository — `git clone` and they are ready.
- One `.gitignore`, one PR template, one `CONTRIBUTING.md` — consistency by default.
- Code reviews show the full picture — if an API change is accompanied by a frontend update, everything is visible in a single PR.

### Negative
- As the project grows, `git clone` and `git status` may become slow (with hundreds of files).
- The CI pipeline tests everything on every PR, even if only the frontend changed (until path-based filtering is implemented).
- If a team member works only on the frontend, they will see the noise from the backend code in the repository.
- The repository size will grow large over time because everything is in one place.

### Neutral
- Branch protection rules will apply to the entire repository — separate rules for frontend and backend will not be possible.
- This can be partially managed with a code ownership (`CODEOWNERS`) file.

## Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| CI pipeline becomes slow because everything is built on every PR | Medium | Medium | Use path-based triggers in GitHub Actions (e.g., `paths: ['apps/web/**']`) |
| Repository size becomes very large | Low | Low | Use Git LFS for large assets, maintain a strict `.gitignore`, keep images/assets on a CDN |
| Code ownership becomes unclear as the team grows | Medium | Medium | Add a `CODEOWNERS` file to define who reviews PRs for `apps/web/`, etc. |
| The need for build tooling arises later and migration is painful | Low | Medium | The folder structure (`apps/` + `packages/`) is kept compatible with Nx/Turborepo, so migration will be straightforward |
| Developers mistakenly make changes in another team's code | Low | Low | This will be caught by the code review process and CODEOWNERS; in a small team, this is more of a feature than a bug |

## Review Date

**Next review:** After 3 months of development or when the team size grows to 8+.

This decision should be revisited if:
- CI build times consistently exceed 5 minutes.
- The team splits into dedicated frontend and backend sub-teams with independent release cycles.
- The count of shared packages exceeds 3, and manual dependency management becomes painful.
- A new team member repeatedly complains that the repository is difficult to navigate.

## References

- [Monorepo vs Polyrepo — Thoughtworks Tech Radar](https://www.thoughtworks.com/radar/techniques/monorepos)
- [Nx Documentation — Why Monorepos](https://nx.dev/concepts/why-monorepos)
- [GitHub — About CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- Sprint 1 Task: "Git Repository Setup & Branching Strategy"

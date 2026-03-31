# Contributing to Our Project

We love your input! We want to make contributing to this project as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing new features
- Becoming a maintainer

## We Develop with Github
We use github to host code, to track issues and feature requests, as well as accept pull requests.

## Our Development Process

To ensure consistency and quality, we follow these development practices.

### Branching Strategy

All of our development is based on the [GitHub Flow](https://guides.github.com/introduction/flow/index.html).

1.  **`main` branch:** This is the primary branch, representing the latest stable version of our project. All development branches are created from `main`.
2.  **Feature Branches:** All new work, whether it's a feature, bugfix, or chore, should be done on a separate branch.

### Branch Naming Convention

We use a consistent naming convention for our branches to make them easy to understand. Please name your branches using this format:

`type/short-description`

-   **type:** Describes the purpose of the branch.
    -   `feat`: A new feature for the user.
    -   `fix`: A bug fix for the user.
    -   `chore`: Repository maintenance, tooling, or configuration changes.
    -   `docs`: Changes to documentation.
    -   `refactor`: Refactoring production code.
-   **short-description:** A few words describing the change, separated by hyphens.

**Examples:**
- `feat/add-user-login`
- `fix/header-alignment-issue`
- `chore/update-gitignore`

### Commit Convention

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification. This helps us maintain a clear and descriptive commit history and enables automated changelog generation.

Each commit message should follow this format:

`type(scope): subject`

-   **type:** Must be one of the types listed in the branch naming section (`feat`, `fix`, `chore`, etc.).
-   **(scope):** An optional scope to provide additional contextual information (e.g., `api`, `ui`).
-   **subject:** A short, imperative-tense description of the change.

**Example:**
`feat(auth): implement password reset endpoint`

### Code Review and Pull Request Process

All code changes happen through Pull Requests (PRs).

1.  **Open a Pull Request:** Once your work is ready, open a PR from your feature branch to the `main` branch.
2.  **Describe Your PR:** Write a clear title and description for your PR, explaining the "what" and "why" of your changes.
3.  **Get a Review:** Request a review from at least one other team member. All PRs must be reviewed and approved before merging.
4.  **Automated Checks:** All PRs must pass our automated CI checks (linting, tests, build).
5.  **Merge:** Once the PR is approved and all checks are green, it can be merged into `main`. Please use the **Squash and Merge** option to keep our Git history clean.

## Any contributions you make will be under the MIT Software License
In short, when you submit code changes, your submissions are understood to be under the same [MIT License](http://choosealicense.com/licenses/mit/) that covers the project. Feel free to contact the maintainers if that's a concern.

## Report bugs using Github's [issues](../../issues)
We use GitHub issues to track public bugs. Report a bug by [opening a new issue](); it's that easy!

## Write bug reports with detail, background, and sample code

**Great Bug Reports** tend to have:

- A quick summary and/or background
- Steps to reproduce
  - Be specific!
  - Give sample code if you can.
- What you expected would happen
- What actually happens
- Notes (possibly including why you think this might be happening, or stuff you tried that didn't work)

People *love* thorough bug reports. I'm not even kidding.

## Use a Consistent Coding Style
* 2 spaces for indentation rather than tabs
* You can try running `npm run lint` for style unification

## License
By contributing, you agree that your contributions will be licensed under its MIT License.

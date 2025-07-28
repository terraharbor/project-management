# Contribution Guidelines

This document outlines the contribution guidelines and conventions used for the TerraHarbor project. Understanding and following these guidelines will help maintain a consistent and high-quality codebase and ensure smooth collaboration among contributors.

## Table of Contents

- [Contribution Guidelines](#contribution-guidelines)
  - [Table of Contents](#table-of-contents)
  - [Development Process](#development-process)
  - [Commit messages](#commit-messages)
  - [Releases](#releases)
  - [Labels for Issues and Pull Requests](#labels-for-issues-and-pull-requests)
    - [`type` labels](#type-labels)
    - [`status` labels](#status-labels)
    - [`priority` labels](#priority-labels)
    - [`effort` labels](#effort-labels)
    - [`work` labels](#work-labels)
    - [Other labels](#other-labels)

## Development Process

An usual development process for a feature or bug fix in the TerraHarbor project involves the following steps:

1. **Create an issue**
   * Before starting work on a new feature or bug fix, create an issue in the respective repository. This helps in tracking the work and discussing the requirements with other contributors.
   * The repository's workflows will automatically add the issue to the project board, where it can be prioritized and assigned to a milestone.
   * There is no automatic assignment of labels to issues, so you will need to manually add labels to the issue to categorize it appropriately. More information on labels can be found in the [Issues and Pull Requests](#issues-and-pull-requests) section below.

2. **Create a branch**
   * Create a new branch for your work based on the `main` branch.
   * The branch name should be somewhat descriptive of the work being done, ideally following the format `feat/short-description` for new features or `fix/short-description` for bug fixes. This helps in identifying the purpose of the branch at a glance.

3. **Make changes**: Make the necessary changes to the codebase on your branch.

4. **Test your changes**
   * Ensure that your changes are well-tested and do not break existing functionality.
   * If you are adding new features, consider writing unit tests to cover the new functionality.
   * Run the existing unit tests to ensure that they pass with your changes.
   * Workflows and linters will run automatically on pull requests to ensure the tests are passing before merging.
   * You can also use the provided [`docker-compose-local.yaml`](https://github.com/terraharbor/infrastructure/blob/main/docker-compose/docker-compose-local.yaml) file to run the project locally and test your changes in a local environment.

5. **Commit changes**
   * Commit your changes with a clear and descriptive commit message that follows the [Conventional Commits](#commit-messages) specification.
   * Ensure that your commit messages are meaningful and provide context for the changes made.
   * The commit message cannot be longer than 70 characters, but if you find that further explanation is needed, you can add a longer description in the body of the commit message. The body should be separated from the subject line by a blank line.

6. **Push your branch and create a pull request**
   * Push your branch to the remote repository.
   * Create a pull request (PR) from your branch to the `main` branch.
   * If the PR is a work in progress, you can mark it as a draft PR. This is useful if you want to get early feedback on your changes without notifying all reviewers that the PR is ready for review.
   * In the PR description, provide a summary of the changes made and reference the issue that this PR addresses (e.g., "Closes #123"). You can also reference the issue in the right sidebar.
   * Categorize the PR by adding appropriate labels (see the [Issues and Pull Requests](#issues-and-pull-requests) section below for more information on labels).
   * Upon creating the PR, the workflows will run automatically to ensure that the code passes all tests and the commit messages follow the [Conventional Commits](#commit-messages) specification.
   * If the repository contains a `Dockerfile`, the image will also be built on the PR, in order to check the build passes, however it will not be pushed to the registry until the PR is merged.

7. **Review and merge**
   * Once you consider the PR ready for review, move it to the `To Review` column on the project board and mark it as ready for review (i.e. remove the draft status).
   * The PR will be assigned to the appropriate reviewers based on the `CODEOWNERS` file in the repository. Reviewers will be notified of the PR and can provide feedback.

8. **Address feedback**
   * If reviewers provide feedback, address the comments and make necessary changes to the code.
   * Push the changes to your branch, and the PR will automatically update with the new commits.
   * You might need to move the PR to the `To Review` column on the project board if it was moved to the `In Progress` column while you were addressing feedback.

9.  **Merge the PR**
    * Once the PR is approved and all checks have passed, you can merge the PR into the `main` branch.
    * If making a merge commit, ensure that the commit message follows the [Conventional Commits](#commit-messages) specification. Tipically, GitHub generates a commit message like `Merge pull request #123 from user/branch-name`, which does not follow the specification. You can edit the commit message in the merge dialog to something like `chore: merge pull request #123 from user/branch-name`.
    * Alternatively, you can use the "Squash and merge" option to combine all commits into a single commit with a meaningful message that follows the [Conventional Commits](#commit-messages) specification.
    * **If the PR contains multiple changes, we recommend using the normal merge commit option, as it preserves the history of the changes made in the branch and generates a more meaningful changelog upon release.**

## Commit messages

We follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification for commit messages. This helps in automating the release process and generating changelogs.

The commit message should be structured as follows:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Typical `<type>` values include:

- `feat`: a new feature
- `fix`: a bug fix
- `docs`: documentation only changes
- `style`: changes that do not affect the meaning of the code (white-space, formatting, etc.)
- `refactor`: a code change that neither fixes a bug nor adds a feature
- `perf`: a code change that improves performance
- `test`: adding missing or correcting existing tests
- `chore`: miscellaneous changes that do not modify source or test files (e.g. merge commits, dependency updates, etc.)
- `build`: changes that affect the build system or external dependencies (e.g. Dockerfile)
- `ci`: changes to our CI configuration files and scripts (e.g. GitHub Actions workflows)

## Releases

We follow [Semantic Versioning](https://semver.org/) for versioning our releases. The version number is in the format `MAJOR.MINOR.PATCH`, where:
- **MAJOR** version changes indicate incompatible API changes,
- **MINOR** version changes indicate backward-compatible functionality,
- **PATCH** version changes indicate backward-compatible bug fixes.

The official documentation for the semantic versioning can be found at [semver.org](https://semver.org/).

> [!IMPORTANT]
> As you can see from the semantic release specification, the version bump for each release is determined by the type of changes made in the codebase between releases. Therefore, it is important to follow the commit message conventions outlined in the [Commit messages](#commit-messages) section to ensure that the versioning is accurate and meaningful.

<!-- TODO Maybe talk about how the automatic release process works -->

## Labels for Issues and Pull Requests

We tried to keep the issue and pull request labels consistent across all repositories in the TerraHarbor project. Default labels have been added to each repository, using the Terraform code on the [github-org-management](https://github.com/TerraHarbor/github-org-management) repository.

The following subsections describe each group of labels and their intended use.

> [!NOTE]
> These default sane labels are mostly based on the this reference [post](https://seantrane.com/posts/logical-colorful-github-labels-18230/) and are adapted from the respective [GitHub repository](https://github.com/seantrane/github-label-presets).

> [!TIP]
> Use at least the `type`, `status`, and `priority` labels to categorize issues and pull requests. The `effort` and `work` labels can also be useful for estimating the complexity and nature of the work involved, but they are optional and may be added later after discussing the issue among the team.

### `type` labels

These labels categorize issues and pull requests by the nature of the work involved. Use them to clarify what kind of change is being proposed or discussed.

- **type: bug** – Something isn't working as expected or is broken.
- **type: chore** – Routine tasks, such as reorganizing files or other maintenance.
- **type: discussion** – Questions, proposals, or items that require discussion.
- **type: docs** – Changes or additions to documentation.
- **type: feature** – Brand new functionality, features, pages, or workflows.
- **type: fix** – Improvements or iterations on existing features or infrastructure.
- **type: security** – Security-related issues or vulnerabilities.
- **type: testing** – Related to testing and quality assurance.

### `status` labels

These labels indicate the current state or progress of an issue or pull request.

- **state: approved** – Approved to proceed; ready for implementation or merge.
- **state: blocked** – Progress is blocked by another issue, dependency, or requirement.
- **state: pending** – Waiting for requirements, dependencies, data, or more information.
- **state: inactive** – No action needed or possible (e.g., fixed elsewhere, out of scope).

### `priority` labels

These labels help indicate the urgency or importance of an issue or pull request.

- **priority: now** – Needs immediate attention; should be addressed as soon as possible.
- **priority: soon** – Should be addressed soon, but not urgent.
- **priority: later** – Can be addressed at a later time; not currently a priority.

### `effort` labels

These labels estimate the amount of effort required to complete an issue or pull request, using a Fibonacci scale.

- **effort: 1** – Minimal effort required.
- **effort: 2** – Small effort required.
- **effort: 3** – Moderate effort required.
- **effort: 5** – Significant effort required.
- **effort: 8** – Large effort required.
- **effort: 13** – Very large or complex effort required.

### `work` labels

These labels describe the nature of the work required, based on the [Cynefin framework](https://en.wikipedia.org/wiki/Cynefin_framework).

- **work: obvious** – The situation is clear; best practices apply.
- **work: complicated** – The situation requires analysis; good practices apply.
- **work: complex** – The situation is unpredictable; emergent practices apply.
- **work: chaotic** – The situation is turbulent or novel; novel practices apply.

### Other labels

These labels provide additional context or automation hints for issues and pull requests.

- **breaking** – Changes that break backwards compatibility.
- **good first issue** – Issue that is good for newcomers, first-time contributors of the project.
- **help** – An issue where community contributions are welcome.
- **renovate** – Used by the Renovate bot for dependency update automation.
- **autorelease: pending** – Indicates a release is pending (used by the Release Please automation).
- **autorelease: tagged** – Indicates a release has been tagged (used by the Release Please automation).

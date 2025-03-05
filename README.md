---
title: Development Guideline
description: Guideline for developing the HPCS Portal
published: true
date: 2025-02-24T04:52:15.275Z
tags: 
editor: markdown
dateCreated: 2025-02-20T04:21:01.835Z
---

# Dev Guideline

[TOC]

## Write Visually

```mermaid
journey
    title Information Delivering Efficiency
    section Text
      Poor text: 1
      Nice text: 3
      Fanstastic text: 4
    section Figure
      Poor pics: 3
      Nice pics: 5
      Fantastic pics: 7
```
Figures worth more, especially when they are well designed.


### The Right Figure Type

```mermaid
quadrantChart
    title "Choosing the Right Mermaid Figure Type"
    x-axis "Purpose: Data/Process → Structure/Architecture"
    y-axis "Complexity: Simple → Complex"
    quadrant-1 "Complex Structure/Architecture"
    quadrant-2 "Complex Data/Process"
    quadrant-3 "Simple Data/Process"
    quadrant-4 "Simple Structure/Architecture"

    "Flowchart": [0.4, 0.7]
    "Sequence": [0.3, 0.6]
    "Class D.": [0.7, 0.65]
    "State D.": [0.45, 0.85]
    "Entity Relationship D.": [0.75, 0.3]
    "User Journey": [0.05, 0.25]
    "Gantt": [0.25, 0.9]
    "Pie Chart": [0.1, 0.1]
    "Requirement D.": [0.8, 0.6]
    "Gitgraph": [0.25, 0.3]
    "C4 D.": [0.6, 0.7]
    "Mindmaps": [0.6, 0.2]
    "Timeline": [0.25, 0.85]
    "Sankey": [0.35, 0.85]
    "XY Chart": [0.1, 0.15]
    "Block D.": [0.85, 0.15]
    "Kanban": [0.1, 0.8]
    "Architecture": [0.8, 0.8]
```

Here are some extended notes on creating figures in markdown:
- For the ease of editing, use `mermaid` when possible. Fallback to `plantuml` if necessary. Use picture files or draw.io only when the above two options are not available. Attach the code generating the picture if possible.
- The wiki.js integrated mermaid is not the latest version. If you need a new feature, please use the [kroki](https://kroki.io/) service to render the mermaid diagram. Check the source code of this page for examples.

## Document CHANGES Even Tiny

Collaboration means we need to develop on other's shoulder. Document every change when you push, even if it is tiny. 

```mermaid
---
title: Components of a GOOD GIT PUSH
---
erDiagram
    GOOD_GIT_PUSH ||--o{ CODE : contains
    GOOD_GIT_PUSH ||--o{ CHANGELOG : contains
    GOOD_GIT_PUSH ||--o{ DOCUMENT : contains
    GOOD_GIT_PUSH {
        commit short_description

    }
    CODE {
        header license_contact_date_description
        code code
        comment comment
    }
    CHANGELOG {
        version_date stamp_of_change
        Added new_feature
        Changed changed_feature
        Fixed bug_fixed
    }
    DOCUMENT {
        api_name api_name
        input format_input
        output format_output
    }
```

A satisfying git push should contain the following components:
- **Code**: The code you wrote. 
  - It should be well commented when being pushing to the `dev` branch.
  - **Every functional file** (e.g. `.py`, `.js`) should have a header with the license, contact, date, and description.
- **Changelog**: What's new in this push?
  - All changes should be appended to the `CHANGELOG.md` file.
  - The changelog should be in the following format:
    ```markdown
    ## [Version] - [Date]
    ### Added
    - New feature
    ### Changed
    - Changed feature
    ### Fixed
    - Bug fixed
    ```
- **Document**: The API/function you wrote.
  - If the project is small, the document can be written in the `README.md` file.
  - Otherwise, create markdown files in the `docs` folder.
  - For each **external exposing** APIs or functions, document the following:
    - API name and description
    - Input format and example
    - Output format and example
- **Commit message**: A brief description of the change. Refer to [this post](https://github.blog/developer-skills/github/write-better-commits-build-better-projects/).
    - Recommend to use the `{type}({!scope}): {subject}` format.
      - `type`: The type of change. `feat`(feature), `fix`(bug fix), `docs`(documentation), `style`(formatting), `refactor`(refactoring), `perf`(performance), `test`(testing), `chore`(other changes), `merge`(merging branches).
      - `scope`: The scope of the change. Optional. It can be a module, a file, or a function.
      - `subject`: A brief description of the change. It should be in the imperative mood and start with a verb. For example, `add`, `fix`, `update`, `remove`, etc.
      - Example commit messages: `feat(api): add new API for user login`, `fix(auth): fix bug in user login`, `docs(api): update API document for user login`, `style(auth): format code for user login`, `refactor(auth): refactor code for user login`, `perf(auth): improve performance for user login`, `test(auth): add test for user login`, `chore(auth): update dependencies for user login`.

## Branch Strategy

Ideally, three branches are enough for the project:
- `dev`: The development branch. All changes should be pushed to this branch.
- `main`: The main branch. This branch should be stable and ready for production.
- `hotfix`: The hotfix branch. The branch take care of bugs found during production. It should be merged to `main` and `dev` branches.

```mermaid
---
title: Three-branch strategy
---
gitGraph
  commit id: "init"
  branch hotfix
  branch develop
  checkout develop
  commit id: "feat: add new feature"
  commit id: "test: add test for new feature"
  checkout hotfix
  commit id: "fix: fix bug in main"
  checkout main
  merge hotfix id: "merge hotfix to main"
  checkout develop
  merge hotfix id: "merge hotfix to develop"
  commit id: "fix: fix bug in new feature"
  commit id: "fix: fix bug in new feature"
  checkout main
  merge develop
  commit id: "release" type: HIGHLIGHT tag: "v1.0.0"
  checkout develop
  commit id: "feat: add another feature"
```

Then, why do people use `release` branch? The benefits are:
- for **testing**: The `release` branch can be used for testing the release candidate before merging to `main`.
- for **continuous development**: It's easy to revisit the release branch and do bug fixing. Especially when the main branch is far ahead of the target release branch.

A with-release branch strategy is shown below:
```mermaid
---
title: With-release branch strategy
---
gitGraph
    commit id: "Initial commit"
    branch release/v1.1.0
    branch hotfix
    branch develop
    checkout develop
    commit id: "feat: add new feature"
    commit id: "test: add test for new feature"
    
    checkout main
    checkout hotfix
    commit id: "fix: critical bug in production"
    
    checkout main
    merge hotfix tag: "v1.0.1" id: "Merge hotfix into main"
    
    checkout develop
    merge hotfix id: "Merge hotfix into develop"
    
    checkout develop
    commit id: "fix: bug in new feature"
    commit id: "fix: regression fix"
    
    checkout main
    checkout release/v1.1.0
    merge develop id: "Prepare release candidate"
    commit id: "chore: update version" tag: "RC1"
    
    checkout main
    merge release/v1.1.0 tag: "v1.1.0" type: HIGHLIGHT
    
    checkout develop
    merge release/v1.1.0 id: "Sync release changes"
    
    checkout develop
    commit id: "feat: add another feature"
```

Two above branch srategies are legit in our development. The three-branch strategy is simple and easy to understand. The with-release branch strategy is more complex but provides more flexibility and control over the release process.

In this project, we will use the three-branch strategy. I give the following reasons:
1. The project documents a development guideline. We only have one guideline at a time. The development is quite linear.
2. When it comes to bug fixing. We fix bases in the previous commit. 

Actually, the `hotfix` branch is not necessary in this repo. We can just use the `dev` branch to fix bugs. But I prefer to keep the `hotfix` branch for the sake of clarity and simplicity. The `hotfix` branch is a good practice for future projects.

## Development, Code Review & Merge

```mermaid
journey
    title Code Review and Merge Process

    section 1. Development
      Write Code: 5: Developer
      Write Unit Tests: 3: Developer
      Local Testing: 4: Developer

    section 2. Pull/Merge Request
      Create PR: 2: Developer
      Code Review: 5: Reviewer, Maintainer
      Address Comments: 3: Developer

    section 3. Merge
      Approve PR: 1: Maintainer
      Merge to Main: 2: Maintainer
      Release: 4: Maintainer
```

A typical code review and merge process consists of three steps:

1. **Development**: Write code and unit tests. Run local tests to ensure the code works as expected.
2. **Pull/Merge Request**: Create a pull/merge request. The reviewer will review the code and provide comments. The developer will address the comments and update the code. The maintainer can be the reviewer, the colleagues can also peer review the code.
3. **Merge**: The maintainer will approve the pull/merge request and merge the code to the main branch. The maintainer will also release the new version of the code. The release process can be automated using CI/CD.

## Development Guidelines and Commands

To contribute to development, follow these steps:

**1. Create a local development branch:**

```
git checkout dev
git pull origin dev  # Ensure the latest changes
git checkout -b feature/<feature-name>
```

**2. Security Fixes Check**: Run [[Snyk](https://easternsawwhet.stjude.org/en/projects/Snyk-Integration)] in the local branch within the IDE to check for security vulnerabilities before committing changes.

**3. Unit Testing**: Write and execute unit tests to validate individual components. Ensure test coverage meets the required threshold.

**4. Commit:**

```
git add .
git commit -m "Added new feature: <feature-description>"
```

**5. Push the branch to remote:**

```
git push origin feature/<feature-name>
```

**6. Merge changes back into dev:** Open a merge request on GitLab targeting dev. After approval, merge the branch.

**7. Delete the feature branch (optional but recommended):**

```
git branch -d feature/<feature-name>
git push origin --delete feature/<feature-name>
```

## Miscellaneous Scenarios During Development

### 1. Stashing Changes Before Switching Branches
**Scenario:** You have uncommitted changes but need to switch branches.

```
git stash
git checkout another-branch
```

To apply the stashed changes later:
```
git stash pop
```

### 2. Merge
**Scenario:** You want to combine changes from one branch into another without rewriting history.

```
git checkout dev
git pull origin dev
git merge feature/<feature-name>
git push origin dev
```

### 3. Rebasing
**Scenario:** You need to update your feature branch with the latest changes from a different branch before pushing. Applies your changes on top of another branch to maintain a linear history.

**1. Ensure you are on your feature branch**
```
git checkout feature-branch
```

**2. Fetch the latest changes**
```
git fetch origin
```

**3. Rebase with main (or any different branch)**
```
git rebase origin/main
```

**4. If conflicts occur, resolve them manually, then continue:**
```
git rebase --continue
```

**5. If you want to abort the rebase process:**
```
git rebase --abort
```

### 4. Cherry-Picking
**Scenario:** You want to apply a specific commit from one branch to another without merging everything.

```
git cherry-pick <commit-hash>
```
---
title: Development Guideline
description: Guideline for developing the HPCS Portal
published: true
date: 2025-02-20T04:52:15.275Z
tags: 
editor: markdown
dateCreated: 2025-02-20T04:21:01.835Z
---

# Dev Guideline

## Always Visualize your idea

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
Figures are worth a thousand words. Use them to explain your idea. 

But how to choose the right figure?

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

- Use `mermaid` when possible. Fallback to `plantuml` if necessary.
*The wiki.js integrated mermaid is not the latest version. If you need a new feature, please use the [kroki](https://kroki.io/) service to render the mermaid diagram. Check the source code of this page for examples.*

## Always Document the CODE & CHANGES

Be a thoughtful developer. 

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
      - `type`: The type of change. `feat`(feature), `fix`(bug fix), `docs`(documentation), `style`(formatting), `refactor`(refactoring), `perf`(performance), `test`(testing), `chore`(other changes).
      - `scope`: The scope of the change. Optional. It can be a module, a file, or a function.
      - `subject`: A brief description of the change. It should be in the imperative mood and start with a verb. For example, `add`, `fix`, `update`, `remove`, etc.
      - Example commit messages: `feat(api): add new API for user login`, `fix(auth): fix bug in user login`, `docs(api): update API document for user login`, `style(auth): format code for user login`, `refactor(auth): refactor code for user login`, `perf(auth): improve performance for user login`, `test(auth): add test for user login`, `chore(auth): update dependencies for user login`.







```mermaid
---
title: Example Git diagram
---
gitGraph
   commit id: "init"
   commit
   branch develop
   checkout develop
   commit
   commit
   checkout main
   merge develop
   commit
   commit
```


```mermaid
gantt
    dateFormat  YYYY-MM-DD
    title       HPC Portal Roadmap
    excludes    weekends

    section Planning
    Requirements Gathering              :done,    req1, 2025-03-01,2025-03-07
    Define Metrics                      :done,    req2, 2025-03-08, 2025-03-14
    Project Kickoff                     :milestone, kickoff, 2025-03-15, 0d

    section Development
    Design Portal Architecture          :done,    des1, 2025-03-16, 2025-03-22
    Develop Compute Metrics Module      :active,  dev1, 2025-03-23, 10d
    Develop Storage Metrics Module      :         dev2, after dev1, 10d
    Integrate Compute and Storage Modules :        dev3, after dev2, 7d

    section Testing
    Unit Testing                        :         test1, after dev3, 7d
    Integration Testing                 :         test2, after test1, 7d
    User Acceptance Testing             :         test3, after test2, 10d

    section Deployment
    Prepare Deployment Plan             :         dep1, after test3, 5d
    Deploy to Staging                   :         dep2, after dep1, 3d
    Final Review and Sign-off           :milestone, final, after dep2, 0d
    Deploy to Production                :         dep3, after final, 3d

```

# Domain 2 - Source Control

> **Weight: 15-20%.** Git, branching strategies, pull-request policies, and repository hygiene. Heavy on "which branching model" and "which policy" questions.

---


## Domain mind map

```mermaid
mindmap
  root((Domain 2 - Source Control))
    Concept map
    Branching strategies
    Pull request policies Azure Repos
    GitHub equivalents
    Repository hygiene & scale
    Secret scanning & push protection
    Decision reference
    Common pitfalls
    Microsoft Learn
```

## Concept map

```mermaid
flowchart TD
    Root["Source Control"]
    Root --> Hosts["Hosting"]
    Root --> Branch["Branching strategy"]
    Root --> PR["Pull requests & policies"]
    Root --> Hygiene["Repo hygiene & scale"]

    Hosts --> ADO[Azure Repos Git]
    Hosts --> GH[GitHub Repos]
    Hosts --> TFVC["TFVC<br/>legacy centralized"]

    Branch --> Trunk[Trunk-based development]
    Branch --> GHFlow["GitHub Flow<br/>main + short-lived"]
    Branch --> GitFlow["GitFlow<br/>develop + release + hotfix"]
    Branch --> Release["Release Flow<br/>Microsoft style"]

    PR --> Pol[Branch policies]
    PR --> Reviewers[Required reviewers]
    PR --> Build[Build validation]
    PR --> Path[Path filters & CODEOWNERS]
    PR --> Linked[Linked work items]
    PR --> Status[Status checks]

    Hygiene --> LFS[Git LFS]
    Hygiene --> Sub[Submodules]
    Hygiene --> Mono[Monorepo vs polyrepo]
    Hygiene --> Sparse["Sparse checkout / partial clone"]
    Hygiene --> Hooks["Git hooks / pre-commit"]
    Hygiene --> Secret["Secret scanning + push protection"]
```

---

## Branching strategies

```mermaid
flowchart LR
    A[Pick a strategy] --> Q1{Continuous deploy<br/>multiple times/day?}
    Q1 -- yes --> Trunk["Trunk-based<br/>or GitHub Flow"]
    Q1 -- no --> Q2{Need parallel release<br/>versions in production?}
    Q2 -- yes --> GitFlow["GitFlow<br/>develop + release + hotfix"]
    Q2 -- no --> Q3{Single mainline<br/>+ feature branches OK?}
    Q3 -- yes --> GHFlow["GitHub Flow<br/>main + feature"]
    Q3 -- no --> Release["Release Flow<br/>main + release/* topic branches"]
```

| Strategy | Long-lived branches | Best for |
|---|---|---|
| **Trunk-based** | `main` only | High-frequency CI/CD, feature flags |
| **GitHub Flow** | `main` only, short-lived feature branches | SaaS / web / continuous deployment |
| **Release Flow** | `main` + `release/*` topic | Microsoft-style, ship trains |
| **GitFlow** | `main`, `develop`, `release/*`, `hotfix/*` | Versioned products, multi-version support |

> Modern default: **trunk-based with short-lived feature branches and feature flags**. GitFlow is increasingly considered legacy.

---

## Pull request policies (Azure Repos)

```mermaid
flowchart TD
    PR[New pull request to main] --> P1[Min reviewers >= N]
    PR --> P2[Linked work items required]
    PR --> P3[Comment resolution required]
    PR --> P4[Build validation pipeline]
    PR --> P5[Status checks from external services]
    PR --> P6["Required reviewers by path<br/>= CODEOWNERS"]
    PR --> P7[Auto-include reviewers]
    PR --> P8["Limit merge types<br/>squash / rebase / merge commit"]
    P1 & P2 & P3 & P4 & P5 & P6 -->|all green| Merge[Merge allowed]
```

- "Required" policies block merge; "Optional" policies just inform.
- **Build validation** triggers a CI pipeline against the PR's merged commit; it can be set to expire after N hours of inactivity.
- **Path filters** scope a policy or required-reviewer rule to specific folders (e.g. `/infra/*` requires the platform team).

---

## GitHub equivalents

| Azure Repos | GitHub |
|---|---|
| Branch policy | **Branch protection rule** / **rulesets** |
| Required reviewers (path) | **CODEOWNERS** + required reviews |
| Build validation | **Required status checks** (Actions / external) |
| Linked work items required | Linked issues (informational only) |
| Auto-complete | Auto-merge |
| Squash / rebase / merge | Allowed merge methods (repo settings) |

---

## Repository hygiene & scale

```mermaid
flowchart LR
    Big["Repo getting big / slow"] --> Q{Why?}
    Q -- Large binaries --> LFS["Git LFS<br/>track *.psd *.bin etc."]
    Q -- Multiple products --> Mono["Monorepo + sparse checkout<br/>or split to polyrepo"]
    Q -- Shared code --> Sub["Submodules<br/>or package feed"]
    Q -- Long history --> Filter["git filter-repo<br/>or shallow clone"]
    Q -- Many contributors --> Hooks["Pre-commit hooks<br/>+ branch policies"]
```

- **`.gitignore`** = paths Git ignores; **`.gitattributes`** = per-path normalization (line endings, LFS, diff/merge driver).
- **CODEOWNERS** lives at repo root or `.github/` (GitHub) / `.azuredevops/` (Azure Repos) and assigns mandatory reviewers by path glob.
- **Submodules** pin a commit of another repo; **subtrees** copy history in. Both fragile vs. simply consuming a versioned package from a feed.

---

## Secret scanning & push protection

```mermaid
flowchart LR
    Dev[Developer pushes commit] --> Hook[Git push]
    Hook --> Push[Push protection]
    Push -- secret detected --> Block["Push blocked<br/>(developer must remove or bypass with reason)"]
    Push -- clean --> Server[Server accepts push]
    Server --> Scan["Secret scanning on default + all branches"]
    Scan -- finding --> Alert["Alert + notify owner / partner"]
    Alert --> Rotate["Rotate the credential<br/>history rewrite is NOT enough"]
```

- GitHub: **GitHub Advanced Security** (now part of **GitHub Secret Protection** + **GitHub Code Security**) provides secret scanning + push protection.
- Azure DevOps: **Advanced Security for Azure DevOps** (paid per active committer) brings the same features into Azure Repos.
- Azure Defender for DevOps surfaces secret-scanning alerts from connected GitHub / Azure DevOps orgs in **Microsoft Defender for Cloud**.

---

## Decision reference

| When you see... | Pick... | Why |
|---|---|---|
| "Multiple times-per-day deploys" | **Trunk-based** + feature flags | Avoids long merges |
| "Maintain v1 and v2 in parallel" | **GitFlow** with `release/*` branches | Hotfixes flow back to main |
| "Microsoft Engineering style" | **Release Flow** | One mainline + topic branches |
| "Block merge until reviewer approves" | **Required reviewers** policy | Hard gate |
| "Run unit tests on every PR" | **Build validation** policy | CI pipeline check |
| "Different teams own different folders" | **CODEOWNERS** + path policy | Auto-required reviewers |
| "Repo huge with binary assets" | **Git LFS** | Large file pointers |
| "Block credentials from being pushed" | **Push protection** (GHAS / Advanced Security) | Pre-receive block |
| "Detect leaked secrets in history" | **Secret scanning** | Server-side scan |

---

## Common pitfalls

- Using `git rebase` on a **shared branch** rewrites history others depend on - only rebase your own feature branch before PR.
- Merging a PR via the API or local push **bypasses branch policies** unless the policy is enforced at the server level (Azure Repos enforces; GitHub requires "include administrators" + rulesets).
- **`history rewrite` does not invalidate a leaked secret.** Rotate the credential.
- Submodules require `git clone --recurse-submodules` and `git submodule update --init` - easy to forget in CI.
- LFS files **count against LFS storage / bandwidth quotas separately** from the regular repo.

---

## Microsoft Learn

- [Choose a Git branch strategy](https://learn.microsoft.com/azure/devops/repos/git/git-branching-guidance)
- [Set branch policies (Azure Repos)](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
- [About protected branches (GitHub)](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [About CODEOWNERS](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Secret scanning](https://docs.github.com/code-security/secret-scanning/about-secret-scanning)
- [Advanced Security for Azure DevOps](https://learn.microsoft.com/azure/devops/repos/security/configure-github-advanced-security-features)

---

[<- Processes & Communications](01-processes-and-communications.md) - [Build & Release Pipelines ->](03-build-and-release-pipelines.md)

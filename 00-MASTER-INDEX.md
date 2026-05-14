# AZ-400 Visual Study Guide - Master Index

> **Designing and Implementing Microsoft DevOps Solutions**
> Concept-only study aid built from the official Microsoft Learn skills measured. Diagrams, decision trees, and original summaries - no exam questions reproduced.

**Skills outline:** https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-400

---

## How to use this guide

```mermaid
flowchart LR
    A["Start Here<br/>Master Index"] --> B["Read Mind Map<br/>below"]
    B --> C["Pick a Domain<br/>1->5"]
    C --> D[Study Diagrams]
    D --> E["Memorize<br/>Decision Trees"]
    E --> F["Decision Reference<br/>final review"]
    F --> G[Exam Ready]
```

---

## The 5 Exam Domains - Mind Map

```mermaid
mindmap
  root((AZ-400))
    Processes and Communications
      Agile and Work Tracking
        Azure Boards
        Backlogs Sprints
        Delivery Plans
        Wiki
      Stakeholder Communication
        Dashboards
        Notifications
        Microsoft Teams Integration
      Continuous Feedback
        Test and Feedback Extension
        Release Notes
    Source Control
      Repos and Branching
        Azure Repos Git
        GitHub Repos
        Branch Policies
        Pull Requests
        Trunk Based Development
        GitFlow vs GitHub Flow
      Repository Hygiene
        Git LFS
        Git submodules
        gitignore gitattributes
        CODEOWNERS
        Secret scanning
        Push protection
    Build and Release Pipelines
      Azure Pipelines
        YAML pipelines
        Stages Jobs Steps
        Templates and Extends
        Variable Groups
        Parameters
        Service Connections
        Self hosted vs Microsoft hosted agents
      GitHub Actions
        Workflows
        Reusable workflows
        Composite actions
        OIDC to Azure
      Continuous Delivery
        Environments and Approvals
        Deployment Strategies
        Blue Green
        Canary
        Rolling
        Feature Flags
      Package Management
        Azure Artifacts
        GitHub Packages
        Feed views Upstream sources
      Infrastructure as Code
        Bicep ARM
        Terraform
        Azure Deployment Environments
    Security and Compliance
      DevSecOps
        SAST DAST IAST
        SCA dependency scanning
        Container image scanning
        Secret scanning
      Microsoft Defender for DevOps
        GitHub Advanced Security
        Azure DevOps Advanced Security
      Identity and Secrets
        Microsoft Entra ID
        Workload identity OIDC
        Service Principals
        Managed Identity
        Azure Key Vault
      Compliance
        Audit logs
        Approvals and Gates
        Microsoft Purview
        Azure Policy in pipelines
    Instrumentation
      Observability Pillars
        Logs Metrics Traces
        OpenTelemetry
      Application Insights
        Live Metrics
        Application Map
        Availability tests
        Smart detection
      Azure Monitor and Log Analytics
        KQL queries
        Workbooks Dashboards
        Alerts Action Groups
        Diagnostic Settings
      SRE Practices
        SLI SLO SLA
        Error budgets
        Incident response
```

---

## Official Skills Weighting

```mermaid
pie title AZ-400 Skills Measured (midpoints)
  "Processes & Communications" : 11
  "Source Control" : 16
  "Build & Release Pipelines" : 43
  "Security & Compliance" : 13
  "Instrumentation" : 17
```

| Slice | Weight | Jump to chapter |
| --- | --- | --- |
| Processes & Communications | **10-15%** | [01 Processes & Communications](01-processes-and-communications.md) |
| Source Control | **15-20%** | [02 Source Control](02-source-control.md) |
| Build & Release Pipelines | **40-45%** | [03 Build & Release Pipelines](03-build-and-release-pipelines.md) |
| Security & Compliance | **10-15%** | [04 Security & Compliance](04-security-and-compliance.md) |
| Instrumentation | **15-20%** | [05 Instrumentation](05-instrumentation.md) |

> **Pipelines is ~43% of the exam.** Spend half your study time there.

---

## DevOps lifecycle - service emphasis map

```mermaid
flowchart LR
    PLAN[Plan] --> CODE[Code]
    CODE --> BUILD[Build]
    BUILD --> TEST[Test]
    TEST --> RELEASE[Release]
    RELEASE --> DEPLOY[Deploy]
    DEPLOY --> OPERATE[Operate]
    OPERATE --> MONITOR[Monitor]
    MONITOR --> PLAN

    PLAN --> P1[Azure Boards - GitHub Issues - Wiki]
    CODE --> C1[Azure Repos - GitHub Repos - Branch Policies]
    BUILD --> B1[Azure Pipelines - GitHub Actions]
    TEST --> T1[Test Plans - Pester - NUnit - Selenium]
    RELEASE --> R1[Environments - Approvals - Gates]
    DEPLOY --> D1[Bicep - Terraform - ADE - Deployment Slots]
    OPERATE --> O1[Key Vault - Managed Identity - Defender for DevOps]
    MONITOR --> M1[Application Insights - Log Analytics - Workbooks]
```

---

## The 6 Core Question Patterns in AZ-400

```mermaid
flowchart TD
    Q[Any AZ-400 question] --> P1
    Q --> P2
    Q --> P3
    Q --> P4
    Q --> P5
    Q --> P6

    P1["1 Choose pipeline construct<br/>(stages vs jobs vs steps)"]
    P2["2 Branching strategy<br/>(GitHub Flow vs GitFlow vs trunk)"]
    P3["3 Deployment strategy<br/>(blue/green vs canary vs rolling)"]
    P4["4 Secure secrets<br/>(Key Vault, OIDC, managed identity)"]
    P5["5 Reduce build time<br/>(caching, parallelism, self-hosted agents)"]
    P6["6 Detect issue early<br/>(App Insights, KQL alerts, smart detection)"]

    P1 --> R1["Stage = approval gate<br/>Job = agent boundary<br/>Step = task"]
    P2 --> R2["Trunk + short-lived branches<br/>= modern CI/CD default"]
    P3 --> R3["Blue/green = instant cutover<br/>Canary = % traffic<br/>Rolling = batch"]
    P4 --> R4["OIDC + workload identity =<br/>no stored secrets"]
    P5 --> R5["Cache task<br/>Matrix strategy<br/>Self-hosted for hot caches"]
    P6 --> R6["App Insights + smart detection<br/>+ alert action groups"]
```

---

## The "Magic Words" Translator

```mermaid
flowchart LR
    subgraph Triggers
      T1["'no stored secrets'"]
      T2["'short-lived branches'"]
      T3["'minimize agent cost'"]
      T4["'instant rollback'"]
      T5["'gradual rollout'"]
      T6["'find vulnerable dependencies'"]
      T7["'enforce code review'"]
      T8["'dynamic config without redeploy'"]
      T9["'reusable pipeline logic'"]
      T10["'audit who deployed what'"]
    end
    subgraph Answers
      A1["Workload identity federation (OIDC)"]
      A2["Trunk-based development"]
      A3["Microsoft-hosted agents (consumption)"]
      A4["Blue/green or deployment slot swap"]
      A5["Canary or feature flags"]
      A6["Dependabot / GHAS / Defender for DevOps"]
      A7["Branch policy: required reviewers + build validation"]
      A8["Azure App Configuration + Feature Flags"]
      A9["Pipeline templates + extends"]
      A10["Audit logs + Environment history"]
    end
    T1 --> A1
    T2 --> A2
    T3 --> A3
    T4 --> A4
    T5 --> A5
    T6 --> A6
    T7 --> A7
    T8 --> A8
    T9 --> A9
    T10 --> A10
```

---

## Domain files in this guide

| # | Domain | File | Focus |
|---|--------|------|-------|
| 1 | Processes & Communications | [01-processes-and-communications.md](01-processes-and-communications.md) | Boards, work tracking, dashboards, Teams |
| 2 | Source Control | [02-source-control.md](02-source-control.md) | Git, branching, PR policies, repo hygiene |
| 3 | Build & Release Pipelines | [03-build-and-release-pipelines.md](03-build-and-release-pipelines.md) | YAML, Actions, deployment strategies, IaC |
| 4 | Security & Compliance | [04-security-and-compliance.md](04-security-and-compliance.md) | DevSecOps, OIDC, Key Vault, Defender for DevOps |
| 5 | Instrumentation | [05-instrumentation.md](05-instrumentation.md) | App Insights, Log Analytics, KQL, SLI/SLO |
| | **Exam Decision Reference** | [05-exam-cheatsheet.md](05-exam-cheatsheet.md) | Decision trees + scenario keyword map |
| | **Concept & Reference Index** | [06-references.md](06-references.md) | Every concept linked to Microsoft Learn |
| + | **Extra Concepts** | [07-extra-az400-concepts.md](07-extra-az400-concepts.md) | Edge cases + DevOps philosophy |
| + | **Microsoft Learn Summaries** | [08-learn-summaries.md](08-learn-summaries.md) | Per-service overviews |
| + | **Architectures - AZ-400** | [09-arch-az400.md](09-arch-az400.md) | Reference DevOps architectures |

---

## Recommended study order (8 days)

```mermaid
gantt
    title Suggested 8-day plan
    dateFormat X
    axisFormat Day %d
    section Foundations
    Processes & Communications :a1, 0, 1d
    Source Control :a2, after a1, 1d
    section Pipelines (deepest)
    Build pipelines (CI) :b1, after a2, 1d
    Release pipelines (CD) :b2, after b1, 1d
    Deployment strategies + IaC :b3, after b2, 1d
    section Operations
    Security & Compliance :c1, after b3, 1d
    Instrumentation :c2, after c1, 1d
    section Final
    Decision reference + practice :d1, after c2, 1d
```

---

**Next:** open [01-processes-and-communications.md](01-processes-and-communications.md)

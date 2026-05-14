# Microsoft Learn - Module Summaries (AZ-400)

Curated 1-minute summaries of the official AZ-400 Learning Path modules. Read these for a fast refresh, then drill the linked module if any concept is shaky.

> Skills outline: <https://learn.microsoft.com/en-us/credentials/certifications/exams/az-400/>
> Full Learning Path: <https://learn.microsoft.com/en-us/training/courses/az-400t00>

---

## Domain 1 - Processes & Communications

### [Plan Agile with GitHub Projects and Azure Boards](https://learn.microsoft.com/en-us/training/modules/plan-agile-github-projects-azure-boards/)
- Map work items: epic -> feature -> user story -> task.
- Pick the **inherited** process (Basic / Agile / Scrum / CMMI) before customising - system processes are read-only.
- Connect Boards to GitHub via **GitHub repository connection** so commits/PRs link work items via `AB#123`.

### [Design and implement traceability](https://learn.microsoft.com/en-us/training/modules/design-implement-traceability/)
- Traceability = work item -> commit -> PR -> build -> release -> deployed env. Wire all 6 ends.
- Use Azure Boards **Delivery Plans** for cross-team road-mapping.
- Wiki-as-Code: store wiki in repo, version markdown alongside source.

---

## Domain 2 - Source Control

### [Develop a modern source control strategy](https://learn.microsoft.com/en-us/training/paths/az-400-develop-source-control-strategy/)
- Default to **trunk-based** with feature flags for SaaS.
- Mono-repo helps cross-cutting refactors; multi-repo helps team autonomy.
- Use **Git LFS** for binaries; never store secrets even encrypted.

### [Manage repositories at scale](https://learn.microsoft.com/en-us/training/paths/az-400-manage-git-repositories/)
- Branch policies enforce PR review, build validation, work item linking.
- CODEOWNERS auto-requests review on path matches.
- Sparse checkout / partial clone for huge repos.

---

## Domain 3 - Build & Release Pipelines

### [Implement CI with Azure Pipelines / GitHub Actions](https://learn.microsoft.com/en-us/training/paths/build-applications-with-azure-devops/)
- YAML pipelines are versioned with code; classic release pipelines are not - migrate.
- `extends:` template + required template check = governance you can audit.
- Use **template parameters** typed as `string` / `boolean` / `object` for compile-time validation.

### [Design a release strategy](https://learn.microsoft.com/en-us/training/paths/az-400-design-release-strategy/)
- Five strategies: blue-green, canary, rolling, recreate, A/B.
- **Blue-green** = atomic switch, full duplicate cost, zero downtime.
- **Canary** = progressive % shift, lowest blast radius, requires traffic-splitting (Front Door, Traffic Manager, AKS service mesh).

### [Set up release pipelines](https://learn.microsoft.com/en-us/training/paths/az-400-set-up-release-pipelines/)
- Environments hold approvals, gates, branch controls. Approvals in YAML are not enforceable.
- **Deployment jobs** unlock strategies: `runOnce`, `rolling`, `canary`.

### [Implement an appropriate deployment pattern](https://learn.microsoft.com/en-us/training/paths/az-400-implement-deployment-pattern/)
- Feature flags decouple deploy from release.
- Slot deployments in App Service give blue-green for free.
- For containers: use ACR + Kubernetes namespace per env, or Container Apps revisions.

### [Manage Azure Artifacts](https://learn.microsoft.com/en-us/training/modules/manage-artifacts-azure-pipelines/)
- Feed views: `@local` -> `@prerelease` -> `@release`. Promote to publish.
- **Upstream sources** cache npm/PyPI/NuGet/Maven so you scan once and survive outages.

---

## Domain 4 - Security & Compliance

### [Develop a security and compliance plan](https://learn.microsoft.com/en-us/training/paths/az-400-develop-security-compliance-plan/)
- Authenticate pipelines to Azure with **Workload Identity Federation (OIDC)** - no client secret.
- Store secrets in **Key Vault**; access via Managed Identity at runtime, or variable-group-linked-to-KV at pipeline time.

### [Implement security in your DevOps process](https://learn.microsoft.com/en-us/training/paths/az-400-implement-security/)
- Shift-left ladder: SAST -> SCA -> secret scan -> IaC scan -> DAST -> image scan.
- **GitHub Advanced Security** = CodeQL + secret scanning + dependency review (Dependabot).
- **Defender for DevOps** = unified governance signals across GitHub + ADO into Defender for Cloud.

### [Manage code quality and security policies with GitHub](https://learn.microsoft.com/en-us/training/paths/manage-code-quality-security/)
- Push protection blocks new secrets; secret scanning surfaces leaked ones.
- Code scanning runs CodeQL on PR; results gate merge.

---

## Domain 5 - Instrumentation

### [Implement an instrumentation strategy](https://learn.microsoft.com/en-us/training/paths/az-400-implement-instrumentation-strategy/)
- Use **workspace-based** Application Insights tied to a Log Analytics workspace; classic is being retired.
- Three pillars: metrics (numeric, fast), logs (KQL, deep), traces (distributed correlation).
- Send all telemetry via **OpenTelemetry SDK** for vendor neutrality.

### [Develop SRE practices](https://learn.microsoft.com/en-us/training/paths/az-400-develop-sre-strategy/)
- Define SLI -> SLO -> error budget. SLA is the customer contract.
- Burn-rate alerts catch incidents faster than threshold alerts.
- Postmortems are blameless and produce action items tracked in Boards.

### [Implement standards for monitoring and feedback](https://learn.microsoft.com/en-us/training/paths/az-400-implement-standards-monitoring-feedback/)
- **Action groups** bundle receivers; **alert processing rules** suppress/route alerts.
- **Workbooks** for shared dashboards; pin queries from Logs/Metrics.
- Diagnostic settings route platform logs to Log Analytics, Storage, Event Hub.

---

## Cross-cutting

### [Implement DevSecOps with Azure and GitHub](https://learn.microsoft.com/en-us/training/paths/devsecops-azure-github/)
- DevSecOps = security baked into every pipeline stage, not bolted on.
- Threat-model your pipelines just like you threat-model apps.

### [Govern your tech estate with Azure Policy](https://learn.microsoft.com/en-us/training/paths/govern-tech-estate-azure-policy/)
- Effects: Audit, Deny, DeployIfNotExists, Modify, Append, AuditIfNotExists.
- Initiative = bundle of policies; assign at MG/sub/RG scope.
- Run **compliance scan** on a schedule and remediate non-compliant via Policy remediation tasks.

---

## How to use this page

- Read top-to-bottom on day 1 to map every Microsoft Learn module to a domain.
- Re-read the bullets right before the exam - that's the recall scaffolding.
- Click any title to deep-dive when a concept is unclear.

# AZ-400 Exam Decision Reference (Cheatsheet)

> Single-page "when X then Y" lookup. Bullets and tables only - no prose.

---

## Pipeline structure

| Construct | One-liner |
|---|---|
| **Stage** | Approval boundary; can run in parallel unless `dependsOn` |
| **Job** | One agent, one workspace |
| **Step / Task** | Sequential within a job |
| **Template (`include`)** | Reuse YAML fragments |
| **Template (`extends`)** | Governance: caller must conform |
| **Variable group** | Shared variables, optionally Key Vault-linked |
| **Environment** | Hosts approvals + history; required for deployment jobs |

---

## Branching strategy

| Need | Strategy |
|---|---|
| Multi-deploy/day | **Trunk-based** + feature flags |
| Mainline + short-lived feature branches | **GitHub Flow** |
| Microsoft-style topic branches | **Release Flow** |
| Parallel released versions | **GitFlow** |

---

## Deployment strategy

| Goal | Strategy |
|---|---|
| Instant rollback | **Blue/green** (App Service slot swap) |
| Gradual exposure | **Canary** (5% -> 25% -> 100%) |
| Many machines, controlled batches | **Rolling** |
| Toggle in code, no redeploy | **Feature flags** (App Configuration) |

---

## Pipeline identity to Azure

| Capability | Pick |
|---|---|
| No stored secret | **Workload identity federation (OIDC)** - preferred |
| App reads secret at runtime | **Managed Identity -> Key Vault** |
| Legacy deploy where OIDC isn't possible | **Service principal + client secret in Key Vault**, rotate |
| Just-in-time elevated access | **Entra PIM**-eligible role |

---

## Agents

| Need | Pick |
|---|---|
| Cheap / no infra | **Microsoft-hosted** / **GitHub-hosted** |
| Custom tools, private network | **Self-hosted agent** |
| Auto-scale + clean image | **Azure VMSS agent pool** |
| K8s-native auto-scale runners | **Actions Runner Controller (ARC)** |

---

## Source-control policies

| Need | Use |
|---|---|
| Block merge until reviewed | Required reviewers |
| Tests must pass | Build validation |
| Different teams own different folders | CODEOWNERS + path policy |
| Block secret push | Push protection (GHAS / Advanced Security) |
| Find leaked secrets | Secret scanning |
| Dependency CVEs | Dependabot / GHAS dependency review |
| Code-level vulns | CodeQL (SAST) |

---

## Package management

| Need | Use |
|---|---|
| Promote prerelease -> release | **Feed views** (`@prerelease`, `@release`) |
| Cache npm/NuGet from public | **Upstream sources** |
| Internal-only feed | Permissions on Azure Artifacts |
| OS-package style for binaries | Universal packages |

---

## IaC

| Need | Pick |
|---|---|
| Native Azure DSL | **Bicep** |
| Multi-cloud | **Terraform** |
| Real code | **Pulumi** |
| Self-service env from catalog | **Azure Deployment Environments** |
| Preview infra changes | `az deployment what-if` / `terraform plan` |

---

## Monitoring & alerting

| Need | Use |
|---|---|
| Real-time during deploy | **Live Metrics** |
| Auto-detect anomalies | **Smart Detection** |
| Synthetic uptime | **Availability tests** |
| Correlate across services | Central **Log Analytics workspace** + KQL |
| Stream to SIEM | Diagnostic settings -> **Event Hub** |
| Suppress during maint window | **Alert processing rule** |
| Mark deploys on charts | **Release annotations** |

---

## SRE & reliability

| Term | Meaning |
|---|---|
| **SLI** | Actual measurement |
| **SLO** | Internal target |
| **SLA** | External contract |
| **Error budget** | 1 - SLO |
| **MTTR / MTBF** | Recovery / between-failures time |

---

## Magic words -> answer

| Phrase | Answer |
|---|---|
| "no long-lived secrets" | Workload identity federation (OIDC) |
| "instant rollback" | App Service slot swap (blue/green) |
| "5% then 100%" | Canary |
| "feature on / off without redeploy" | Feature flags |
| "private network for build" | Self-hosted agent / VMSS pool |
| "promote build through quality gates" | Azure Artifacts feed views |
| "shared YAML across many pipelines" | Template (`extends` for governance) |
| "different teams own different folders" | CODEOWNERS + path policy |
| "audit who deployed prod" | Environment approvals + audit log |
| "dynamic config" | Azure App Configuration |
| "run security scan on every PR" | Branch policy build validation + SAST/SCA tasks |
| "preview infra change" | `what-if` / `plan` |
| "single security view across repos" | Defender for DevOps |

---

[<- Instrumentation](05-instrumentation.md) - [Concept & Reference Index ->](06-references.md)

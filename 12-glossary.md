# Glossary & Acronyms - AZ-400

> Quick lookup for the alphabet-soup that pervades the AZ-400 exam.

| Term | Expansion / meaning |
|---|---|
| **ACR** | Azure Container Registry |
| **ADE** | Azure Deployment Environments |
| **ADO** | Azure DevOps |
| **AKS** | Azure Kubernetes Service |
| **ARC** | Actions Runner Controller (GitHub self-hosted on Kubernetes) |
| **ARM** | Azure Resource Manager (deployment templates) |
| **Artifact** | A build output (binary, package, image) consumed by later stages |
| **Auto-complete** | Azure Repos PR setting to auto-merge once policies pass |
| **Bicep** | First-party Azure DSL that transpiles to ARM JSON |
| **Blue/green** | Two parallel envs; switch traffic for instant rollback |
| **Branch policy** | Azure Repos rule enforcing PR / build / reviewer requirements |
| **Branch protection** | GitHub equivalent of branch policy |
| **Canary** | Deployment that exposes a small % of users first |
| **CD** | Continuous Delivery (or Continuous Deployment if fully automated) |
| **CI** | Continuous Integration |
| **CMK** | Customer-Managed Key |
| **CodeQL** | GitHub's SAST engine |
| **CODEOWNERS** | File assigning reviewers by path glob |
| **Composite action** | Reusable steps packaged as a GitHub action |
| **Dapr** | Distributed Application Runtime |
| **DAST** | Dynamic Application Security Testing |
| **Defender for DevOps** | Cross-repo security findings hub in Defender for Cloud |
| **Dependabot** | GitHub-managed dependency update / vulnerability scanner |
| **Diagnostic setting** | Azure resource -> Log Analytics / Storage / Event Hub routing |
| **DORA metrics** | Deploy frequency, lead time, change failure rate, MTTR |
| **Environment** | Approval / variable scope target for deployment jobs |
| **Error budget** | 1 - SLO; allowed unreliability per period |
| **Feature flag** | Runtime toggle to enable / disable code paths |
| **Feed view** | Quality gate label on an Azure Artifacts feed (`@release`) |
| **GHAS** | GitHub Advanced Security |
| **GitFlow** | Branching model with `develop` + `release/*` + `hotfix/*` |
| **GitHub Flow** | Branching model: `main` + short-lived feature branches |
| **IaC** | Infrastructure as Code |
| **Inherited process** | Azure Boards process model that allows custom fields/states |
| **Job** | Azure Pipelines unit running on one agent |
| **Key Vault** | Azure secret / key / certificate store |
| **KQL** | Kusto Query Language (used by Log Analytics / App Insights) |
| **LAW** | Log Analytics Workspace |
| **LFS** | Large File Storage (Git) |
| **MI** | Managed Identity |
| **MSDB / SQL Migration** | Tools for moving SQL workloads |
| **MTTR / MTBF** | Mean Time To Recover / Between Failures |
| **OIDC** | OpenID Connect (used by workload identity federation) |
| **OpenTelemetry** | Vendor-neutral observability SDK |
| **PAT** | Personal Access Token |
| **PBI** | Product Backlog Item (Scrum process template) |
| **PIM** | Privileged Identity Management |
| **PR** | Pull Request |
| **PSRule for Azure** | IaC scanner for Bicep / ARM compliance |
| **Pulumi** | IaC tool using real programming languages |
| **Release annotation** | Marker on App Insights chart at deploy time |
| **Release Flow** | Microsoft's branching model: `main` + topic `release/*` |
| **Reusable workflow** | GitHub workflow callable from another via `workflow_call` |
| **Rolling deployment** | Update instances in batches |
| **Ruleset** | Modern GitHub repo / org rule (replaces protection rules) |
| **SAST** | Static Application Security Testing |
| **SCA** | Software Composition Analysis (dependency scanning) |
| **Secret scanning** | Server-side scan of repo content for credentials |
| **Self-hosted agent / runner** | Pipeline agent you operate |
| **Service connection** | Azure Pipelines auth target |
| **SLI / SLO / SLA** | Service-Level Indicator / Objective / Agreement |
| **SP** | Service Principal |
| **Stage** | Azure Pipelines major boundary, can carry approvals |
| **Stakeholder license** | Free Azure DevOps user (read + comment) |
| **Submodule** | Git pointer from one repo to a commit of another |
| **Template (extends)** | Pipeline template the caller must conform to (governance) |
| **Template (include)** | Pipeline template injected at the call site |
| **TFVC** | Team Foundation Version Control (legacy centralized) |
| **Trunk-based** | Single mainline; very short-lived branches + feature flags |
| **Upstream source** | Public registry proxied through an Azure Artifacts feed |
| **Variable group** | Reusable variable bundle, optionally Key Vault-linked |
| **VMSS agent pool** | Virtual Machine Scale Set used as an agent pool |
| **WIF / Workload Identity Federation** | OIDC-based passwordless cloud auth |
| **YAML pipeline** | Pipeline-as-code (replaces classic UI release) |

---

[<- Master Index](00-MASTER-INDEX.md)

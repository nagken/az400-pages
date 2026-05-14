# Concept & Reference Index - AZ-400

> Every concept in this guide linked to its Microsoft Learn / official-docs source.

---

## Processes & Communications

- [Choose a process (Agile / Scrum / CMMI / Basic)](https://learn.microsoft.com/azure/devops/boards/work-items/guidance/choose-process)
- [Plan & track work with Azure Boards](https://learn.microsoft.com/azure/devops/boards/get-started/what-is-azure-boards)
- [Delivery Plans](https://learn.microsoft.com/azure/devops/boards/plans/track-dependencies)
- [Dashboards & widgets](https://learn.microsoft.com/azure/devops/report/dashboards/)
- [Notification subscriptions](https://learn.microsoft.com/azure/devops/notifications/manage-your-personal-notifications)
- [Service hooks events](https://learn.microsoft.com/azure/devops/service-hooks/events)
- [Wikis](https://learn.microsoft.com/azure/devops/project/wiki/)
- [Test & Feedback extension](https://learn.microsoft.com/azure/devops/test/perform-exploratory-tests)

## Source Control

- [Choose a Git branch strategy](https://learn.microsoft.com/azure/devops/repos/git/git-branching-guidance)
- [Trunk-based development](https://learn.microsoft.com/devops/develop/how-microsoft-develops-devops)
- [Branch policies (Azure Repos)](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
- [Branch protection / rulesets (GitHub)](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository)
- [About CODEOWNERS](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Git LFS](https://git-lfs.com/)
- [Secret scanning (GitHub)](https://docs.github.com/code-security/secret-scanning/about-secret-scanning)
- [Push protection](https://docs.github.com/code-security/secret-scanning/protecting-pushes-with-secret-scanning)
- [Advanced Security for Azure DevOps](https://learn.microsoft.com/azure/devops/repos/security/configure-github-advanced-security-features)

## Build & Release Pipelines

- [Azure Pipelines YAML schema](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema/)
- [Stages, jobs, steps](https://learn.microsoft.com/azure/devops/pipelines/process/stages)
- [Templates: include vs extends](https://learn.microsoft.com/azure/devops/pipelines/process/templates)
- [Variables & expressions](https://learn.microsoft.com/azure/devops/pipelines/process/variables)
- [Variable groups & Library](https://learn.microsoft.com/azure/devops/pipelines/library/variable-groups)
- [Service connections](https://learn.microsoft.com/azure/devops/pipelines/library/service-endpoints)
- [Workload identity federation for service connections](https://learn.microsoft.com/azure/devops/pipelines/release/configure-workload-identity)
- [Microsoft-hosted agents](https://learn.microsoft.com/azure/devops/pipelines/agents/hosted)
- [Self-hosted agents](https://learn.microsoft.com/azure/devops/pipelines/agents/agents)
- [VMSS agent pools](https://learn.microsoft.com/azure/devops/pipelines/agents/scale-set-agents)
- [GitHub Actions docs](https://docs.github.com/actions)
- [Reusable workflows](https://docs.github.com/actions/using-workflows/reusing-workflows)
- [Composite actions](https://docs.github.com/actions/creating-actions/creating-a-composite-action)
- [GitHub Actions runners (self-hosted, ARC)](https://docs.github.com/actions/hosting-your-own-runners)
- [OIDC: GitHub Actions to Azure](https://learn.microsoft.com/azure/developer/github/connect-from-azure-openid-connect)
- [Environments & approvals (Azure Pipelines)](https://learn.microsoft.com/azure/devops/pipelines/process/environments)
- [Environments (GitHub Actions)](https://docs.github.com/actions/deployment/targeting-different-environments/using-environments-for-deployment)
- [Deployment strategies](https://learn.microsoft.com/azure/devops/pipelines/process/deployment-jobs)
- [Deployment slots (App Service)](https://learn.microsoft.com/azure/app-service/deploy-staging-slots)
- [Azure App Configuration & Feature Flags](https://learn.microsoft.com/azure/azure-app-configuration/concept-feature-management)
- [Azure Artifacts feeds, views, upstreams](https://learn.microsoft.com/azure/devops/artifacts/concepts/feeds)
- [GitHub Packages](https://docs.github.com/packages)
- [Bicep documentation](https://learn.microsoft.com/azure/azure-resource-manager/bicep/)
- [Terraform on Azure](https://learn.microsoft.com/azure/developer/terraform/)
- [Azure Deployment Environments](https://learn.microsoft.com/azure/deployment-environments/)

## Security & Compliance

- [DevSecOps in Azure](https://learn.microsoft.com/azure/architecture/solution-ideas/articles/devsecops-in-azure)
- [GitHub Advanced Security](https://docs.github.com/get-started/learning-about-github/about-github-advanced-security)
- [CodeQL code scanning](https://docs.github.com/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/about-code-scanning)
- [Dependabot alerts & version updates](https://docs.github.com/code-security/dependabot)
- [Microsoft Defender for DevOps](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-devops-introduction)
- [Microsoft Defender for Cloud](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-cloud-introduction)
- [Azure Key Vault overview](https://learn.microsoft.com/azure/key-vault/general/overview)
- [Managed identities for Azure resources](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview)
- [Workload identity federation](https://learn.microsoft.com/entra/workload-id/workload-identity-federation)
- [Microsoft Entra Privileged Identity Management (PIM)](https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure)
- [Azure Policy overview](https://learn.microsoft.com/azure/governance/policy/overview)
- [Azure DevOps audit log streaming](https://learn.microsoft.com/azure/devops/organizations/audit/auditing-streaming)
- [Microsoft Purview](https://learn.microsoft.com/purview/)

## Instrumentation

- [Application Insights overview](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)
- [Workspace-based Application Insights](https://learn.microsoft.com/azure/azure-monitor/app/create-workspace-resource)
- [OpenTelemetry with Azure Monitor](https://learn.microsoft.com/azure/azure-monitor/app/opentelemetry-overview)
- [Application Map](https://learn.microsoft.com/azure/azure-monitor/app/app-map)
- [Live Metrics](https://learn.microsoft.com/azure/azure-monitor/app/live-stream)
- [Availability tests](https://learn.microsoft.com/azure/azure-monitor/app/availability-overview)
- [Smart Detection](https://learn.microsoft.com/azure/azure-monitor/alerts/proactive-diagnostics)
- [Diagnostic settings](https://learn.microsoft.com/azure/azure-monitor/essentials/diagnostic-settings)
- [Azure Monitor alerts](https://learn.microsoft.com/azure/azure-monitor/alerts/alerts-overview)
- [Action groups](https://learn.microsoft.com/azure/azure-monitor/alerts/action-groups)
- [Alert processing rules](https://learn.microsoft.com/azure/azure-monitor/alerts/alerts-action-rules)
- [KQL quick reference](https://learn.microsoft.com/azure/data-explorer/kql-quick-reference)
- [Workbooks](https://learn.microsoft.com/azure/azure-monitor/visualize/workbooks-overview)
- [SRE foundation learning path](https://learn.microsoft.com/training/paths/sre-foundation/)

---

[<- Cheatsheet](05-exam-cheatsheet.md) - [Master Index ->](00-MASTER-INDEX.md)

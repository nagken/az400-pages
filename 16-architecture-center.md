# Azure Architecture Center - DevOps Reference Architectures

The Architecture Center is the single richest source of "what does Microsoft consider the right answer?" for AZ-400 scenario questions. Bookmark every link below.

> Top-level entry point: <https://learn.microsoft.com/en-us/azure/architecture/browse/?azure_categories=devops>

---

## CI/CD reference architectures

- **[CI/CD baseline architecture with Azure Pipelines](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/apps/devops-dotnet-baseline)** - full pattern with Azure Repos + Pipelines + App Service + Key Vault + App Insights. Memorise the wiring; exam questions mirror this.
- **[CI/CD for AKS](https://learn.microsoft.com/en-us/azure/architecture/guide/aks/aks-cicd-github-actions-and-gitops)** - GitHub Actions + GitOps (Flux) pattern; demonstrates pull-based deployment to Kubernetes.
- **[Azure DevOps for serverless apps](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/serverless/cicd-for-azure-functions)** - Functions + ARM/Bicep + slot swap.
- **[Build a Container CI/CD pipeline](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/cicd-for-containers)** - ACR + AKS + Pipelines including image scanning + signing.

## DevSecOps

- **[DevSecOps in GitHub](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/devsecops-in-github)** - CodeQL + Dependabot + secret scanning + Defender for DevOps in one diagram.
- **[DevSecOps in Azure](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/devsecops-in-azure)** - same pattern but ADO-flavored.
- **[DevSecOps for infrastructure as code](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/devsecops-infrastructure-as-code)** - IaC scanning + policy as code + drift.
- **[DevSecOps for AKS](https://learn.microsoft.com/en-us/azure/architecture/guide/aks/aks-devsecops)** - image admission, pod identity, secret rotation.

## Source control / repos

- **[Adopting Git at scale](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/apps/devops-with-aks)** - branching strategy decision tree.

## Release strategies

- **[Blue-green deployment with Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/deploy-staging-slots)** - slot swap.
- **[Canary release with Azure Front Door + Azure Spring Apps](https://learn.microsoft.com/en-us/azure/architecture/guide/spring-apps-multi-region/spring-apps-multi-region)** - progressive traffic shift pattern.
- **[Ring-based deployment](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/devops/ring-based-deployment)** - multi-region, gated rollout.

## Observability + SRE

- **[Monitoring a microservices architecture](https://learn.microsoft.com/en-us/azure/architecture/microservices/logging-monitoring)** - App Insights + LAW + alerts.
- **[Site reliability engineering on Azure](https://learn.microsoft.com/en-us/azure/site-reliability-engineering/)** - SLO/SLA/SLI + error budget framework.
- **[Workbooks pattern](https://learn.microsoft.com/en-us/azure/azure-monitor/visualize/workbooks-overview)** - shared dashboards.

## Hybrid + on-prem agents

- **[Hub-spoke for self-hosted build agents](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/devops/devops-self-hosted-agents)** - VMSS pool + private endpoints + firewall egress allowlist.
- **[Azure Arc for hybrid build infrastructure](https://learn.microsoft.com/en-us/azure/architecture/hybrid/arc-hybrid-kubernetes)** - agents on-prem managed via Arc.

## IaC + governance

- **[Bicep + GitHub Actions delivery](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-github-actions)** - what-if + apply gating.
- **[Enterprise scale landing zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)** - where Azure Policy + management groups + RBAC fit.
- **[Terraform module pattern](https://learn.microsoft.com/en-us/azure/developer/terraform/best-practices-end-to-end-testing)** - module + workspace + remote state.

## Disaster recovery

- **[Multi-region deployment for high availability](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)** - RTO/RPO patterns.
- **[Azure DevOps + ACR DR](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-disaster-recovery)** - geo-replicated registry.

---

## How to read these

For every architecture page, ask yourself **5 questions**:

1. What problem is this solving? (one sentence)
2. Which AZ-400 domain does it map to?
3. Which Azure services are connecting? (name them all)
4. What is the security boundary? (env, RBAC scope, network)
5. What is the failure mode if I remove one box?

If you can answer those 5, you can answer any scenario question on the exam.

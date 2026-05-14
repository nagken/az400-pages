# Architectures - AZ-400

End-to-end DevOps reference architectures that pull multiple AZ-400 domains together. Each one ties **source -> build -> release -> security -> instrumentation** into a single picture.

---

## 1. End-to-end CI/CD on Azure (App Service / Container Apps)

```mermaid
flowchart LR
    Dev[Developer] -->|git push| GH[GitHub repo<br/>main + feature branches]
    GH -->|branch policy / required check| PR[Pull request]
    PR -->|merged to main| GHA["GitHub Actions<br/>(or Azure Pipelines)"]
    GHA --> Build[Build + unit test]
    Build --> Scan["Security scans<br/>SAST / SCA / secrets / container"]
    Scan --> Pkg[Push image to ACR<br/>or artifact to Azure Artifacts]
    Pkg --> ENV1[Env: dev<br/>auto deploy]
    ENV1 -->|approval gate| ENV2[Env: test<br/>integration tests]
    ENV2 -->|approval + change ticket| ENV3[Env: prod<br/>blue-green or canary]
    ENV3 --> APP[App Service / Container Apps / AKS]
    APP --> AI[Application Insights]
    AI --> LA[Log Analytics workspace]
    LA --> ALERT[Alerts + action groups]
    ALERT --> ONCALL[On-call / Teams / PagerDuty]
    KV[(Azure Key Vault)] -.MI / OIDC.-> APP
    KV -.linked variable group.-> GHA
```

> **Domains touched:** Source Control + Build/Release + Security & Compliance + Instrumentation. Pipeline auths to Azure with **OIDC workload identity federation**, secrets pulled from Key Vault via Managed Identity.

---

## 2. Multi-stage YAML pipeline with environments + approvals

```mermaid
flowchart TD
    Trigger["main push / PR / scheduled"] --> S1[Stage: Build]
    S1 --> J1[Job: build]
    J1 --> Step1["Restore + compile + test"]
    Step1 --> Step2["Publish artifact"]
    S1 --> S2[Stage: DeployDev]
    S2 -->|environment dev<br/>no approval| Dev[Deploy to dev slot]
    Dev --> S3[Stage: DeployTest]
    S3 -->|environment test<br/>1 approver + smoke gate| Test[Deploy to test slot]
    Test --> S4[Stage: DeployProd]
    S4 -->|environment prod<br/>2 approvers + change-mgmt gate + business hours| Prod[Deploy to prod slot]
    Prod --> Swap["Slot swap or canary 10% --> 100%"]
    Swap --> Smoke[Post-deploy smoke + AI availability test]
    Smoke -->|fail| Rollback[Rollback / re-swap]
```

> **Key idea:** environments are the security boundary. Approvals, gates, and branch restrictions live on the **environment**, not the pipeline file.

---

## 3. Hub-and-spoke for self-hosted agents (private network)

```mermaid
flowchart LR
    subgraph Hub[VNet hub]
        Bastion[Azure Bastion]
        FW[Azure Firewall<br/>egress allowlist to ADO + GitHub]
    end
    subgraph Spoke[VNet spoke - agents]
        VMSS[Self-hosted agents on VMSS<br/>or Azure DevOps managed pool]
        ARC[Azure Arc-connected agents<br/>on-prem]
    end
    subgraph Targets[Target subscriptions]
        AKS[AKS]
        SQL[Azure SQL]
        WEB[App Service]
    end
    Spoke -->|peering| Hub
    Hub --> FW --> Internet[(ADO / GitHub / package feeds)]
    VMSS -->|private endpoints| AKS
    VMSS -->|private endpoints| SQL
    VMSS -->|private endpoints| WEB
    KV2[(Key Vault private endpoint)] -.MI.-> VMSS
```

> **Why:** keeps deployment traffic off the public internet, agents reach Azure resources through **private endpoints**, only egress to ADO/GitHub/feeds via the firewall.

---

## 4. Container CI/CD with image scanning

```mermaid
flowchart LR
    Code[Source repo] --> CI[CI pipeline]
    CI --> Build[docker build]
    Build --> ScanImg[Trivy / Defender for DevOps scan]
    ScanImg -->|critical CVE| Fail[Fail pipeline]
    ScanImg -->|clean| Sign["cosign sign image<br/>(supply-chain attestation)"]
    Sign --> Push[Push to Azure Container Registry]
    Push --> Tag["Tag: git SHA + semver"]
    Tag --> CD[CD pipeline]
    CD --> Deploy[Deploy to AKS / Container Apps]
    Deploy --> Verify["Cosign verify on admission<br/>(Gatekeeper / Ratify)"]
    Verify --> Run[Pods running]
    ACRT[ACR Tasks] -.base image update.-> Push
```

> **Supply chain:** sign on the way in (CI), verify on the way out (admission controller). Base-image updates auto-rebuild via **ACR Tasks**.

---

## 5. Secure pipeline identity to Azure (OIDC)

```mermaid
flowchart TD
    Repo[GitHub repo / ADO project] --> Workflow[Workflow / Pipeline]
    Workflow -->|requests token| OIDC[GitHub OIDC issuer<br/>or ADO OIDC]
    OIDC --> Entra[Microsoft Entra ID<br/>federated credential on App registration]
    Entra --> Token[Short-lived access token<br/>NO secret stored]
    Token --> ARM[Azure Resource Manager]
    ARM --> RBAC{Least-priv role}
    RBAC --> RG[Resource Group scope]
    KV3[(Key Vault)] -.MI.-> App[Deployed app]
```

> **Replace** all client-secret service principals with **federated credentials**. Subject claim is scoped to repo + branch / environment, so a compromised PR can't deploy to prod.

---

## 6. Observability stack (App Insights + Log Analytics + alerts)

```mermaid
flowchart LR
    App[App Service / AKS / Functions] -->|OpenTelemetry SDK| AI[Application Insights<br/>workspace-based]
    AI --> LAW[(Log Analytics workspace)]
    Diag[Diagnostic settings on Azure resources] --> LAW
    LAW --> KQL[KQL queries + workbooks]
    KQL --> Dash[Azure Dashboards / Grafana]
    LAW --> Alert[Alert rules<br/>metric / log / activity]
    Alert --> AG[Action Group<br/>email + Teams + webhook + ITSM]
    AG --> APR["Alert Processing Rule<br/>(suppression / routing)"]
    APR --> Pager[On-call rotation]
    AI --> SLO[SLI / SLO dashboards<br/>error budget burn rate]
```

> **One workspace per environment** is the typical pattern: `law-dev`, `law-prod`. App Insights resources point at the workspace so you can join app + infra logs in one KQL query.

---

## 7. IaC governance (Bicep + What-If + Azure Policy)

```mermaid
flowchart LR
    Code[Bicep / Terraform in repo] --> PR2[Pull request]
    PR2 --> Lint[bicep lint / tflint / checkov]
    Lint --> Plan["az deployment what-if<br/>or terraform plan"]
    Plan --> Review[Reviewer sees diff in PR]
    Review -->|approve + merge| Apply[CD: az deployment / terraform apply]
    Apply --> Pol[Azure Policy evaluation<br/>deny non-compliant]
    Pol -->|pass| Resources[Resources deployed]
    Pol -->|fail| Reject[Deployment rejected]
    Resources --> Drift[Drift detection schedule<br/>terraform plan / what-if cron]
    Drift -->|drift detected| Issue[Open GitHub issue]
```

> **Guardrail order:** lint -> static scan -> what-if/plan in PR -> apply -> policy admission -> drift detection. Each step catches a different class of mistake.

---

## 8. Disaster recovery for the DevOps platform itself

```mermaid
flowchart TD
    A[Azure DevOps org / GitHub org] --> B{What needs DR?}
    B --> Repos[Source code]
    B --> Pipes[Pipeline definitions]
    B --> Artif[Artifacts / packages]
    B --> Secr[Secrets]
    Repos --> Mirror["Mirror to second region repo<br/>or scheduled clone backup"]
    Pipes --> YAML["YAML in repo = versioned automatically"]
    Artif --> Geo["ACR geo-replication<br/>Azure Artifacts upstream cache"]
    Secr --> KVGeo["Key Vault soft-delete + purge protection<br/>+ geo-redundant backup"]
```

> **Lesson:** YAML pipelines are recoverable from the repo. **Classic release pipelines, variable groups, service connections, and PATs are NOT** - back them up separately (export JSON, document in IaC).

---

## How to use these diagrams

- Print or screenshot one before each study session and try to **explain every arrow out loud**.
- For each box, ask: "what AZ-400 domain owns this?" -> trains the magic-words mapping in the cheatsheet.
- The exam loves "which service belongs in box X?" questions. These diagrams give you a mental library of correct answers.

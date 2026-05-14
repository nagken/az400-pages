# Hands-On Labs - AZ-400

Free, exam-aligned labs you can run in your own subscription (most fit in the Azure free tier). Each lab maps to one or more AZ-400 domains.

> **Setup once:** Create a fresh resource group `rg-az400-labs` in `eastus` to keep cost contained. Delete the RG after each lab to clean up.

---

## Lab 1 - Build your first YAML pipeline (Domain: Build & Release)

**Goal:** push a Node/.NET sample app, build it, and publish an artifact.

1. Fork [microsoft/azure-pipelines-yaml-samples](https://github.com/microsoft/azure-pipelines-yaml).
2. In Azure DevOps, create a new pipeline -> "Existing YAML" -> point at `/samples/dotnet/dotnet.yml`.
3. Add a **secret variable** in the pipeline UI (e.g. `MY_SECRET`).
4. Add a step that does `echo $(MY_SECRET)` - confirm it shows `***`.
5. Commit a change to trigger the pipeline.

**You should be able to:** explain the difference between `${{ }}` and `$(  )`, why the secret is masked, and how triggers work.

---

## Lab 2 - Branch policies + CODEOWNERS (Domain: Source Control)

1. In your Azure Repos or GitHub repo, create a `CODEOWNERS` file with `* @<your-user>`.
2. Set the `main` branch policy: **Require pull request**, **Require 1 approver**, **Require check from CI**, **Require review from Code Owners**.
3. Open a PR that modifies a file - confirm you cannot merge without yourself approving.
4. Try to push directly to `main` - should be blocked.

**You should be able to:** describe what each policy enforces and the order they evaluate.

---

## Lab 3 - Workload Identity Federation (Domain: Security)

[Microsoft Learn module](https://learn.microsoft.com/en-us/training/modules/authenticate-apps-with-managed-identities/) covers MI; for OIDC see [Configure a federated identity credential](https://learn.microsoft.com/azure/active-directory/workload-identities/workload-identity-federation-create-trust).

1. Create a new App registration in Entra ID.
2. Add a **federated credential** with subject `repo:<user>/<repo>:ref:refs/heads/main`.
3. Assign the SP **Contributor** on `rg-az400-labs`.
4. In your GitHub repo create `.github/workflows/deploy.yml` using `azure/login@v2` with `client-id`, `tenant-id`, `subscription-id` and `permissions: id-token: write`.
5. Push and watch the workflow log in successfully - **no client secret stored**.

**You should be able to:** explain why OIDC is preferred over client secrets and what the federation `subject` claim restricts.

---

## Lab 4 - Multi-stage pipeline with environments + approvals (Domains: Build & Release + Security)

1. In Azure DevOps, create two **Environments**: `dev` (no approval) and `prod` (1 approver = you).
2. Author a YAML pipeline with stages `Build -> DeployDev -> DeployProd`, each `DeployProd` job uses `environment: prod`.
3. Run the pipeline; pause when it hits `prod`; approve via the UI.
4. Add a **branch control** check on the `prod` environment - only allow `main` branch.

**You should be able to:** describe environments as a security boundary and list at least 3 protection options (approvals, branches, business hours, gates).

---

## Lab 5 - Container CI/CD with image signing (Domain: Security + Build & Release)

1. Create an Azure Container Registry (`Basic` SKU is fine).
2. Build a Dockerfile in your repo; push the image with `az acr build`.
3. Add **Trivy** scan step in the pipeline - fail on `CRITICAL`.
4. Add **cosign sign** step using a key stored in Key Vault.
5. Deploy to **Azure Container Apps**, configure ACR pull via system-assigned MI.

**You should be able to:** trace an image from source -> CI -> registry -> runtime, and explain at which step each control runs.

---

## Lab 6 - Bicep + what-if + Azure Policy (Domain: Security + Build & Release)

1. Author a Bicep file deploying a Storage Account.
2. Add a custom **Azure Policy** assigned at the RG: deny storage with `supportsHttpsTrafficOnly = false`.
3. Author the Bicep with HTTPS = false -> run `az deployment group what-if` - should show the change.
4. Run actual deploy - should be **denied by Policy**.
5. Fix Bicep -> redeploy -> succeeds.

**You should be able to:** describe the diff between what-if (predicts) and Policy (enforces), and where each runs in the pipeline.

---

## Lab 7 - Application Insights + KQL alerts (Domain: Instrumentation)

1. Create a workspace-based App Insights in `rg-az400-labs`.
2. Deploy a sample web app (any) and add the App Insights SDK.
3. Generate some 500 errors (e.g. `/error` endpoint).
4. In Log Analytics, write KQL: `requests | where success == false | summarize count() by bin(timestamp, 5m), name`.
5. Save as **alert rule** with threshold > 5 in 5 min, action group sends email + Teams webhook.
6. Add an **alert processing rule** that suppresses alerts during your "maintenance window" tag value.

**You should be able to:** write a KQL aggregation, build an alert from a saved query, and explain alert processing rules.

---

## Lab 8 - GitHub Actions reusable workflow + matrix (Domain: Build & Release)

1. Create `.github/workflows/build.yml` in repo A with `workflow_call:` trigger and a matrix on `os: [ubuntu-latest, windows-latest]`.
2. In repo B, call repo A's workflow via `uses: <user>/<repoA>/.github/workflows/build.yml@main`.
3. Pin `actions/checkout@<sha>` instead of `@v4` - explain why pinning to SHA is more secure.

**You should be able to:** describe reusable workflows, matrix, and supply-chain protection via SHA pinning.

---

## Lab 9 - Self-hosted VMSS agent pool (Domain: Build & Release)

1. Create a VMSS in `rg-az400-labs` (B2s, 1 instance).
2. In Azure DevOps, **Project settings -> Agent pools -> Add -> Azure VMSS**.
3. Configure Azure DevOps to manage the scale set (auto-scale 0-3).
4. Run a pipeline pointing at the new pool; confirm the VMSS scales out.

**You should be able to:** explain when self-hosted is needed (private network, custom tooling, large image cache) and how the auto-scale works.

---

## Lab 10 - Drift detection (Domain: Security + Build & Release)

1. Take any Bicep deployment from earlier labs.
2. Manually edit the resource in the Portal (e.g. change a tag).
3. Add a **scheduled pipeline** that runs `az deployment group what-if` weekly.
4. On any non-empty diff, fail the pipeline and open a GitHub issue (action: `peter-evans/create-issue-from-file`).

**You should be able to:** explain why drift matters, and at least 2 ways to detect it (what-if/plan, Azure Policy compliance scan).

---

## Cleanup

```powershell
az group delete -n rg-az400-labs --yes --no-wait
```

> Run after each lab session so you don't burn budget overnight on a forgotten VMSS.

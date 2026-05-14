# Common Pitfalls - AZ-400

The exam loves "looks-right-but-is-wrong" answers. Each pitfall below is a trap I've personally hit or seen in practice questions. Memorise the **green column** verbatim.

---

## Source control

| Pitfall (looks right) | Why it's wrong | Correct |
|---|---|---|
| Use **GitFlow** for a SaaS web app shipping daily | Long-lived `develop` + `release` branches add merge debt; SaaS wants constant flow | **Trunk-based** with short-lived feature branches and feature flags |
| Push protection alone is enough for secrets | Push protection blocks **new** secrets; existing ones in history are still leaked | Push protection **+** secret scanning + remediate historical leaks (rotate + rewrite history) |
| Add a CODEOWNER as a branch policy reviewer | CODEOWNERS only auto-requests review - doesn't enforce | Enable **"Require review from Code Owners"** branch policy alongside the file |
| Squash merge breaks bisect | Bisect works fine on squash merges (one commit per PR is often easier to bisect) | Worry about **rebase merge** with no merge commit if you rely on first-parent history |
| Git LFS for source code | LFS is for binary assets, not source | Use LFS for media/binaries; keep source as plain Git |

---

## Build & release pipelines

| Pitfall | Why it's wrong | Correct |
|---|---|---|
| Put approvals in the YAML file with `condition:` | Conditions can be bypassed by editing YAML | Approvals live on the **environment** (Azure Pipelines) or **environment protection rules** (GitHub Actions) |
| Use `$(MyVar)` for a parameter in a template | `$( )` is runtime substitution - parameters are compile-time | Use `${{ parameters.MyVar }}` for parameters, `$(var)` for variables |
| Self-hosted agent on a single VM for HA | Single point of failure | **VMSS-backed agent pool** or managed pool with auto-scale |
| Azure Pipelines `extends:` template still allows step injection | Extends template can `validateSteps` and reject step lists | Use `extends` + a **required template check** in the protected branch policy |
| Service principal with client secret in variable group | Secret leaks via logs, expires, manual rotation | **Workload identity federation (OIDC)** - no secret stored |
| Deploy to all regions in parallel | One bad build kills every region simultaneously | **Ring-based / progressive** rollout across regions with bake time + health gates |
| "Canary = blue-green" | They are not the same | **Blue-green** = two full envs, switch traffic 0->100; **Canary** = small % of traffic -> progressive shift |

---

## Security & compliance

| Pitfall | Why it's wrong | Correct |
|---|---|---|
| Store secrets in pipeline variables marked "secret" | Still ends up in some logs, hard to rotate centrally | **Key Vault** + variable group linked to Key Vault, or Managed Identity at runtime |
| Grant `Owner` on subscription to the deployment SP | Massive blast radius | **Least-privilege** custom role at the **resource group** scope |
| Run SAST/DAST only on release branch | Bugs caught too late | **Shift left**: SAST on PR, SCA on PR, DAST on test env, container scan on build |
| Defender for DevOps replaces GHAS | They overlap but Defender adds Azure Policy/governance signals; GHAS adds CodeQL + secret scanning | Use **both** - they are complementary, not substitutes |
| Admin permissions in ADO == Owner in Azure | They are separate planes | ADO project admin != Azure RBAC; manage both explicitly |

---

## Instrumentation

| Pitfall | Why it's wrong | Correct |
|---|---|---|
| Classic Application Insights resource | Classic is being retired and can't join logs with infra in the same workspace | **Workspace-based** App Insights tied to a Log Analytics workspace |
| One Log Analytics workspace per app | Explodes cost + breaks cross-app KQL | **One workspace per environment** (or per region), namespace logs by `_ResourceId` |
| Set the alert action to email only | Email is unreliable for on-call | **Action group** with multiple receivers: ITSM + webhook + Teams + SMS |
| SLO of 100% | No error budget = no shipping | Pick **realistic SLO** (e.g. 99.9%) so you have budget to deploy fast |
| Sampling at 100% in prod | Cost balloons; ingestion limits | Use **adaptive sampling** + per-telemetry-type rules |

---

## Processes & communications

| Pitfall | Why it's wrong | Correct |
|---|---|---|
| Stakeholder license for someone editing backlog | Stakeholder is read-mostly + limited editing | **Basic** access level for board contributors |
| Service hooks for everything | Service hooks are point-to-point and noisy | **Teams Azure Boards / Pipelines integration** for chat-ops; service hooks for code that needs raw events |
| Inherited process can't be edited | You can edit **inherited** processes; **system** processes are locked | Use inherited (Agile/Scrum/Basic/CMMI inherited) and customise away |
| "Velocity = throughput" | Different measures | **Velocity** = story points per sprint; **Throughput** = items per unit time |

---

## Quick exam-day reminders

- Read every "select all that apply" twice - Microsoft loves trick distractors.
- If two answers are technically valid, prefer the **Microsoft-recommended / modern / OIDC / managed-identity** option.
- "Minimise admin overhead" -> pick the **PaaS/managed** option.
- "Minimise blast radius" -> pick the **least-privilege / most-scoped** option.
- "Minimise downtime" -> **blue-green or canary**, not in-place.
- "Deny non-compliant resources" -> **Azure Policy with Deny effect**, not just Audit.

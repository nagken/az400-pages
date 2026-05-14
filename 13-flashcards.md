# Flashcards - AZ-400

Quick recall drills. Click any card to flip. Cover the **Back** column with your hand and try to answer aloud before flipping.

<section class="fc-section" data-fc-title="All">
<div class="flashcard-grid">
  <div class="flashcard"><div class="fc-q">What does OIDC federation replace in pipelines?</div><div class="fc-a">Client-secret service principals. The pipeline gets a short-lived token from the OIDC issuer; no secret is stored.</div></div>
  <div class="flashcard"><div class="fc-q">Where do approvals live for Azure Pipelines?</div><div class="fc-a">On the <strong>Environment</strong>, not the YAML. Cannot be bypassed by editing the pipeline file.</div></div>
  <div class="flashcard"><div class="fc-q">${{ }} vs $( ) vs $[ ] in Azure Pipelines</div><div class="fc-a">${{ }} = compile-time (template/parameters), $( ) = runtime variable substitution, $[ ] = runtime expression (dependencies/outputs).</div></div>
  <div class="flashcard"><div class="fc-q">Azure Pipelines template: include vs extends</div><div class="fc-a">include = paste at this point, no validation. extends = whole pipeline derives from a template; can validateSteps + enforce required template check.</div></div>
  <div class="flashcard"><div class="fc-q">Blue-green vs canary</div><div class="fc-a">Blue-green = two full envs, switch traffic 0->100. Canary = % shift over time on the same service.</div></div>
  <div class="flashcard"><div class="fc-q">Trunk-based vs GitFlow</div><div class="fc-a">Trunk-based: short-lived branches, daily merge to main, feature flags. GitFlow: long-lived develop/release; suits versioned/boxed software, not SaaS.</div></div>
  <div class="flashcard"><div class="fc-q">CODEOWNERS effect</div><div class="fc-a">Auto-requests review from owners of changed paths. Combine with branch policy "Require review from Code Owners" to enforce.</div></div>
  <div class="flashcard"><div class="fc-q">Push protection vs secret scanning</div><div class="fc-a">Push protection blocks NEW secrets at push. Secret scanning detects existing secrets in history. Use both.</div></div>
  <div class="flashcard"><div class="fc-q">SAST / DAST / SCA / IAST</div><div class="fc-a">SAST = static code analysis. DAST = runtime/black-box. SCA = dependency vulnerabilities. IAST = instrumented runtime hybrid.</div></div>
  <div class="flashcard"><div class="fc-q">Workspace-based vs classic App Insights</div><div class="fc-a">Workspace-based stores telemetry in a Log Analytics workspace; classic stores it standalone (being retired). Always pick workspace-based.</div></div>
  <div class="flashcard"><div class="fc-q">SLI / SLO / SLA</div><div class="fc-a">SLI = measured indicator. SLO = internal target. SLA = customer-facing contract with consequences.</div></div>
  <div class="flashcard"><div class="fc-q">Error budget</div><div class="fc-a">1 - SLO. Time you can be down without breaking the SLO. Burn fast = pause feature work, focus on reliability.</div></div>
  <div class="flashcard"><div class="fc-q">DORA metrics (4)</div><div class="fc-a">Deployment frequency, lead time for changes, change failure rate, mean time to restore (MTTR).</div></div>
  <div class="flashcard"><div class="fc-q">Self-hosted agent HA</div><div class="fc-a">Use a <strong>VMSS-backed agent pool</strong> (or managed pool) - auto-scales, no single point of failure.</div></div>
  <div class="flashcard"><div class="fc-q">Feed views in Azure Artifacts</div><div class="fc-a">@local, @prerelease, @release. Promote packages between views; consumers pin to @release.</div></div>
  <div class="flashcard"><div class="fc-q">Upstream sources in Artifacts</div><div class="fc-a">Cache packages from public registries (npm, PyPI, NuGet, Maven). Survives upstream outages and lets you scan once.</div></div>
  <div class="flashcard"><div class="fc-q">Bicep what-if</div><div class="fc-a"><code>az deployment group what-if</code> - diff between desired template and current state. Run in PR for review.</div></div>
  <div class="flashcard"><div class="fc-q">Azure Policy effect: Deny vs Audit</div><div class="fc-a">Deny blocks deployment of non-compliant resources. Audit only logs. Always pair Deny with a remediation task for existing resources.</div></div>
  <div class="flashcard"><div class="fc-q">Service connection types in ADO</div><div class="fc-a">Azure Resource Manager (with Workload Identity / SP / MI), GitHub, Docker Registry, Kubernetes, generic. Prefer Workload Identity.</div></div>
  <div class="flashcard"><div class="fc-q">GitHub environments vs Azure Pipelines environments</div><div class="fc-a">Both gate deploys with approvals + secrets scoped to environment. GitHub environments also gate which branches can deploy to them.</div></div>
  <div class="flashcard"><div class="fc-q">Action group</div><div class="fc-a">Bundle of receivers (email, SMS, webhook, ITSM, Teams, runbook) reused across alert rules.</div></div>
  <div class="flashcard"><div class="fc-q">Alert processing rule</div><div class="fc-a">Suppress, route, or override action groups for alerts based on filters (e.g. silence during maintenance window).</div></div>
  <div class="flashcard"><div class="fc-q">OpenTelemetry</div><div class="fc-a">Vendor-neutral SDK + protocol for traces/metrics/logs. App Insights ingests OTel directly; replaces SDK lock-in.</div></div>
  <div class="flashcard"><div class="fc-q">Required reviewers vs required approvers</div><div class="fc-a">Reviewers (PR) approve code before merge. Approvers (environment) approve a pipeline run before deployment.</div></div>
</div>
</section>

---

> Tip: shuffle by reloading and revealing only every 3rd card. After two passes, write the ones you missed onto paper.

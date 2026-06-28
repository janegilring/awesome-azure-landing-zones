---
name: landing-zone-vending
description: "Technical skill for subscription (landing zone) vending: the automated, self-service process a platform team uses to request, deploy, and govern application landing zone subscriptions at scale. Covers the collect-data → initiate-platform-automation → create-subscription → post-deployment workflow, the Gitflow/parameter-file pattern, the deployment-pipeline task categories, and the platform-to-application-team handoff. Implementation uses the AVM sub-vending modules. WHEN: \"subscription vending\", \"landing zone vending\", \"vend a subscription\", \"self-service subscriptions\", \"automate subscription creation\", \"sub-vending module\", \"lz-vending module\", \"subscription parameter file\", \"subscription request pipeline\", \"platform to app team handoff\", \"provision governed subscriptions at scale\". DO NOT USE FOR: the management group hierarchy / subscription strategy itself (use caf-resource-organization); policy/initiative authoring (use caf-governance); deploying the platform foundation (use the alz-accelerator skill); billing account or tenant decisions (use caf-billing-entra-tenant)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Resource organization"
---

# Landing zone (subscription) vending

## Overview

**Subscription vending** standardizes how an organization requests, deploys, and governs
subscriptions so application teams get a ready-to-use **application landing zone** quickly while the
**platform team** retains governance. The platform team owns an automated pipeline that collects a
validated request, creates the subscription with infrastructure as code, applies networking,
identity, RBAC, policy, and a preliminary budget, and then **hands the subscription off** to the
application team. This is the implementation pattern that operationalizes *subscription
democratization* at scale; it builds on the resource organization and governance design areas and
deploys with the **AVM sub-vending modules**.

## When to use this skill

- Designing or reviewing a **self-service subscription vending** process.
- Defining the **data to collect** for a subscription request and how it is validated and approved.
- Building the **Gitflow + parameter-file** automation that creates subscriptions.
- Designing the **deployment pipeline** tasks (identity, governance, networking, budgets, reporting).
- Planning the **platform-team → application-team handoff** and ongoing governance.
- Implementing with the **Bicep** or **Terraform** sub-vending modules.

## Key concepts

- **Three automation tasks** (the article's workflow):
  1. **Collect data** — gather and validate the request (authorizer, cost center, connectivity
     needs, environments, data sensitivity) via an ITSM/Power Apps/portal tool; produce a trackable,
     approved request that triggers automation.
  2. **Initiate platform automation** — a **request pipeline** turns the approved request into a
     single **subscription parameter file** (JSON/YAML/TFVARS), opens a **pull request** into
     `main`; merging triggers deployment (**Gitflow**).
  3. **Create the subscription** — a **deployment pipeline** runs the IaC sub-vending module to
     create and configure the subscription.
- **One parameter file per subscription** — the subscription is the unit of deployment. For
  Terraform, use a **dedicated state file per subscription** to limit blast radius and speed up
  plan/apply.
- **Deployment pipeline task categories** — Identity (ownership, workload identities), Governance
  (management-group placement, RBAC, policy assignment, Defender for Cloud), Networking (VNet +
  peering to the regional hub), Budgets (preliminary budget), Reporting (IPAM commit, notify the app
  team).
- **Commercial agreement required** — programmatic subscription creation needs an EA/MCA billing
  scope; without one, the creation step is manual but everything else can still be automated.
- **Workload identity** — the pipeline authenticates with **managed identity or OIDC**, never stored
  secrets.
- **Handoff** — the platform team hands the configured subscription to the app team, which updates
  the budget and deploys the workload; the platform team keeps governance ownership.

## Decision guidance

### Collecting and validating requests

- Use a **data collection tool** (ITSM or low/no-code portal) that enforces business approval and
  captures everything the parameter file needs; **validate early** — problems are costly later.
- Interface with internal systems (identity, finance, security, **IPAM**) to enrich the request
  (e.g. reserve IP space) and create a **trackable** record.

### Automation (Gitflow + IaC)

- Generate **one parameter file per request** on a **new branch**, open a **pull request**, and run
  **linting/validation** (including IP-range availability); add a manual review gate if needed.
- **Merge to `main` triggers deployment.** Keep IaC modules platform-owned so governance is enforced
  by construction.

### Deployment pipeline scope

- Cover all task categories: **place** the subscription in the right management group, assign
  **RBAC** and **policy**, enable **Defender for Cloud**, deploy a **VNet + hub peering**, create a
  **preliminary budget**, and **report back** (IPAM, notify app team).
- Authenticate with **managed identity / OIDC**; scope the pipeline identity least-privilege.

### Post-deployment and ongoing governance

- After handoff, the **app team owns the budget and workload**; the **platform team owns governance**
  (e.g. moving the subscription between management groups as requirements change). Automate routine
  governance changes.

## Recommended resources

- Vending contract (input schema + trigger) — [`references/vending-contract.md`](references/vending-contract.md)
- Subscription vending implementation guidance (Architecture Center) — https://learn.microsoft.com/azure/architecture/landing-zones/subscription-vending
- Subscription vending overview (design area) — https://aka.ms/sub-vending
- Landing zone vending design area — https://aka.ms/lz-vending
- Bicep landing zone vending module — https://aka.ms/lz-vending/bicep
- Terraform landing zone vending module — https://aka.ms/lz-vending/tf
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-resource-organization` — the hierarchy and subscription strategy vending fills.
- Skill: `caf-governance` — the policy scope vended subscriptions inherit.
- Skill: `caf-network-topology-connectivity` — connecting vended VNets to the hub/vWAN.
- Skill: `caf-identity-access-management` — RBAC and workload identities applied at vending time.
- Skill: `caf-platform-automation-devops` — the pipelines and deployment identity behind vending.
- Skill: `alz-accelerator` — deploying the platform foundation that vending depends on.
- Agent: `landing-zone-vending` — drives the vending design and implementation end to end.

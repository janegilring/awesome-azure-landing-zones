---
name: caf-identity-access-management
description: "CAF/ALZ guidance for the Identity and access management design area: Microsoft Entra ID design, the identity subscription, RBAC and custom roles, Privileged Identity Management (PIM), Conditional Access, and hybrid identity for landing zones. WHEN: \"identity\", \"entra id\", \"azure ad\", \"rbac\", \"role assignment strategy\", \"custom role\", \"PIM\", \"privileged identity management\", \"conditional access\", \"managed identity\", \"service principal\", \"identity subscription\", \"domain controllers in azure\", \"hybrid identity\", \"least privilege\", \"break glass\". DO NOT USE FOR: choosing/creating the tenant or billing (use caf-billing-entra-tenant); finding a specific role for a resource and generating the assignment (use the azure-rbac skill); Entra app registration walkthroughs (use entra-app-registration); deploying the platform (use the alz-accelerator skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Identity and access management"
---

# Identity and access management

## Overview

Identity is the **primary security perimeter** of a cloud estate. This design area defines how
**Microsoft Entra ID** authenticates and authorizes access across the landing zone: how privileged
access is granted just-in-time, how RBAC is structured at scale, and where identity infrastructure
(e.g. domain controllers, Entra Connect) lives. In the conceptual ALZ architecture a dedicated
**Identity subscription** under the Platform management group hosts identity services.

## When to use this skill

- Designing the **RBAC model** (built-in vs custom roles, assignment scope at management-group level).
- Planning **privileged access** with **PIM** (eligible roles, approval, time-bound activation).
- Defining **Conditional Access** baselines and MFA for admins and users.
- Placing **identity infrastructure** (domain controllers, Entra Connect/Cloud Sync) in Azure.
- Choosing identities for workloads (**managed identities** over service principals/secrets).
- Establishing **emergency access** ("break-glass") accounts.

## Key concepts

- **Identity subscription** — platform subscription in the Identity management group for domain
  controllers, Entra Connect, and related identity services (when needed).
- **Azure RBAC vs Entra roles** — Azure RBAC controls access to *resources*; Entra roles control
  *directory* functions (e.g. Global Administrator). They are separate planes.
- **PIM (Privileged Identity Management)** — just-in-time, time-bound, approval-gated activation of
  privileged roles; pairs with access reviews.
- **Custom roles** — create only when no built-in role fits; over-customization is a maintenance
  burden. Assign roles at the **management group** scope for inheritance where appropriate.
- **Managed identity** — Azure-managed credential for workloads; prefer it over service principals
  with secrets to eliminate secret handling.
- **Conditional Access** — policy engine that enforces MFA, device, and risk conditions.
- **Hybrid identity** — synchronizing on-premises AD to Entra ID via Entra Connect / Cloud Sync.

## Decision guidance

### RBAC at scale

- Grant access at the **highest sensible scope** (management group → subscription → resource group)
  to use inheritance; avoid sprinkling resource-level assignments.
- Prefer **built-in roles**; introduce custom roles only for a real gap, and keep them few.
- Use **groups** (not individual users) for assignments to keep access reviewable.

### Privileged access

- Make privileged roles **eligible via PIM**, not permanently active; require approval and MFA on
  activation, and review regularly.
- Maintain at least **two break-glass accounts** excluded from Conditional Access blocking policies,
  with monitored sign-ins.

### Workload identity

- Use **managed identities** for Azure resources; reserve service principals for cases that need
  them and rotate/limit any secrets.

### Hybrid identity placement

- If domain controllers must run in Azure, place them in the **Identity subscription**, with
  network reachability to workloads that depend on them and tightly controlled access.

## Recommended resources

- Identity and access management design area — https://aka.ms/alz/iam
- Resource access management in CAF — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/considerations/
- Azure RBAC documentation — https://learn.microsoft.com/azure/role-based-access-control/overview
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-billing-entra-tenant` — choosing/securing the tenant this identity model runs in.
- Skill: `caf-security` — Zero Trust, Defender for Identity, and secrets management.
- Skill: `caf-governance` — policies that enforce identity guardrails.
- Skill: `caf-resource-organization` — where the Identity subscription sits in the hierarchy.
- Agent: `azure-governance` (planned) — enforces identity and access guardrails via policy.

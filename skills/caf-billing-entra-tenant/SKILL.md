---
name: caf-billing-entra-tenant
description: "CAF/ALZ guidance for the Azure billing and Microsoft Entra tenant design area: choosing and securing the Entra tenant, agreement and billing structure (EA/MCA/CSP), enrollment hierarchy, and single- vs multi-tenant decisions for a landing zone. WHEN: \"entra tenant\", \"azure ad tenant\", \"billing\", \"enterprise agreement\", \"EA enrollment\", \"microsoft customer agreement\", \"MCA billing account\", \"CSP\", \"billing scope\", \"invoice section\", \"multi-tenant\", \"new tenant\", \"tenant strategy\", \"who owns the tenant\". DO NOT USE FOR: in-tenant identity, RBAC, PIM, or Conditional Access (use caf-identity-access-management); management group / subscription layout (use caf-resource-organization); deploying the platform (use the alz-accelerator skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Azure billing and Microsoft Entra tenant"
---

# Azure billing and Microsoft Entra tenant

## Overview

This is the foundational design area: before any landing zone exists you must decide which
**Microsoft Entra tenant** is the identity and security boundary for the environment and how Azure
consumption is **billed** and **organized**. These choices are deliberately hard to reverse — the
tenant is the root of trust and the billing hierarchy frames cost ownership for the whole estate.
Most organizations should target a **single production tenant** and add tenants only for hard
isolation requirements.

## When to use this skill

- Deciding whether to use an **existing** Entra tenant or create a **new** one.
- Choosing and structuring the **billing agreement** (EA, MCA, or CSP) and its scopes.
- Planning the **enrollment / billing hierarchy** (departments, accounts, invoice sections).
- Evaluating a **single-tenant vs multi-tenant** strategy (e.g. dev/test isolation, M&A, sovereignty).
- Establishing **ownership and emergency access** for the tenant and billing accounts.

## Key concepts

- **Microsoft Entra tenant** — the identity boundary. One tenant can contain many subscriptions
  and management groups; trust, users, and Conditional Access are tenant-scoped.
- **Billing account** — the top of the billing hierarchy, tied to the agreement type:
  - **Enterprise Agreement (EA):** Billing account → Department → Account → Subscription.
  - **Microsoft Customer Agreement (MCA):** Billing account → Billing profile → Invoice section → Subscription.
  - **Cloud Solution Provider (CSP):** subscriptions provisioned and billed via a partner.
- **Billing roles** are separate from Azure RBAC — they govern who can create subscriptions, view
  invoices, and manage the agreement.
- **Tenant vs subscription:** billing lives with the agreement; the subscription is the unit of
  consumption that is associated to one tenant at a time.
- **Multi-tenant ALZ** — supported but adds operational cost (identity, governance, and tooling
  must span tenants). Use only when isolation is a genuine requirement.

## Decision guidance

### One tenant vs many

- **Default to one tenant** for production. It keeps identity, governance, and operations simple.
- Add a tenant only for **hard boundaries**: regulatory/sovereignty isolation, strict dev/test
  separation, partner/M&A scenarios, or independent security administration.
- If you go multi-tenant, plan **cross-tenant management** (Azure Lighthouse, per-tenant policy and
  monitoring) up front — tooling rarely spans tenants for free.

### Billing structure

- Map the **billing hierarchy to your cost-ownership model** (business units → departments/invoice
  sections) so showback/chargeback is natural.
- Decide **who can create subscriptions** and delegate the billing role to the platform team; this
  underpins subscription vending.
- For MCA, design **billing profiles and invoice sections** to match how finance wants invoices
  split before you scale.

### Tenant ownership and protection

- Assign clear **owners** for the tenant and billing account; secure them with emergency-access
  ("break-glass") accounts and strong MFA.
- Protect the **Global Administrator** and **billing owner** roles — these sit above Azure RBAC.

## Recommended resources

- Azure billing and Microsoft Entra tenant design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/azure-billing-ad-tenant
- ALZ and multiple Microsoft Entra tenants — https://aka.ms/alz/multitenant
- The eight design areas — https://aka.ms/alz/design/areas
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-identity-access-management` — what you configure *inside* the chosen tenant.
- Skill: `caf-resource-organization` — management groups and subscriptions under the tenant.
- Skill: `caf-governance` — guardrails applied once the hierarchy exists.
- Skill: `alz-accelerator` — bootstrapping the platform within the tenant.
- Agent: `azure-architect` (planned) — frames these foundational decisions end to end.

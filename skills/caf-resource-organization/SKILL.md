---
name: caf-resource-organization
description: "CAF/ALZ guidance for the Resource organization design area: the management group hierarchy, subscription strategy (subscription democratization), the conceptual ALZ management groups (Platform, Landing Zones, Decommissioned, Sandbox), resource groups, naming, and tagging. WHEN: \"management group\", \"management group hierarchy\", \"subscription strategy\", \"how many subscriptions\", \"subscription organization\", \"resource organization\", \"naming convention\", \"tagging strategy\", \"resource group structure\", \"corp vs online\", \"sandbox management group\", \"platform management group\", \"landing zones management group\". DO NOT USE FOR: requesting/creating individual landing-zone subscriptions at scale (use the landing-zone-vending agent); policy assignment design (use caf-governance); deploying the hierarchy (use the alz-accelerator skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Resource organization"
---

# Resource organization

## Overview

Resource organization defines the **management group hierarchy** and **subscription strategy** that
everything else hangs off — governance, security, networking, and cost all attach to this scaffold.
ALZ follows **subscription democratization**: subscriptions are the primary unit of management and
scale, handed to workload teams as managed environments rather than treated as scarce. Design the
hierarchy for **policy inheritance and operational clarity**, not to mirror the org chart.

## When to use this skill

- Designing the **management group hierarchy** for a new or evolving landing zone.
- Deciding the **subscription strategy** (per workload, per environment, per business unit).
- Placing platform vs application subscriptions (Connectivity, Identity, Management; Corp, Online).
- Defining **naming** and **tagging** conventions for consistent organization and cost reporting.
- Reviewing an existing flat/ad-hoc structure against the conceptual ALZ architecture.

## Key concepts

- **Conceptual ALZ management groups** (under the Intermediate root):
  - **Platform** — shared services, with child groups **Connectivity**, **Identity**, **Management**.
  - **Landing Zones** — application workloads, commonly split into **Corp** (private/hybrid) and
    **Online** (internet-facing).
  - **Decommissioned** — subscriptions being retired (cancelled, policy-constrained).
  - **Sandbox** — loosely governed space for experimentation, isolated from production.
- **Subscription democratization** — each workload (often per environment) gets its own
  subscription, used as a scale unit and management boundary.
- **Management group depth** — keep the hierarchy **shallow** (Azure supports up to 6 levels below
  root; ALZ rarely needs more than 3–4). Avoid mirroring the org chart.
- **Naming and tagging** — conventions that make resources identifiable and costs attributable
  (environment, owner, cost center, workload).

## Decision guidance

### Management group hierarchy

- Start from the **conceptual ALZ hierarchy** and tailor — don't invent a bespoke tree.
- Group by **policy and operational needs**, not by department; policies inherit downward, so
  structure to maximize useful inheritance and minimize exceptions.
- Keep it **shallow and stable**; reorganizing later moves many subscriptions and reassigns policy.

### Subscription strategy

- Use **subscriptions as scale units**: separate by workload and environment (prod/non-prod) for
  blast-radius isolation, quota headroom, and clear ownership.
- Place **platform** subscriptions (Connectivity, Identity, Management) under the Platform group and
  **application** subscriptions under Landing Zones (Corp/Online).
- Use **Sandbox** for experimentation and **Decommissioned** for orderly retirement.

### Naming and tagging

- Define naming **before** scaling; encode workload, environment, and region consistently.
- Mandate a **minimum tag set** (owner, cost center, environment) and enforce it with policy
  (see `caf-governance`).

## Recommended resources

- Resource organization design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/resource-org
- Conceptual ALZ architecture and management groups — https://aka.ms/alz/design/areas
- Subscription democratization (design principles) — https://aka.ms/alz/design/principles
- Azure service groups for governance — https://learn.microsoft.com/azure/governance/service-groups/overview
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-governance` — policies assigned across the management group hierarchy.
- Skill: `caf-network-topology-connectivity` — where the Connectivity subscription fits.
- Skill: `caf-identity-access-management` — where the Identity subscription fits.
- Skill: `caf-billing-entra-tenant` — the tenant/billing scope above the hierarchy.
- Agent: `landing-zone-vending` (planned) — creating subscriptions into this structure at scale.

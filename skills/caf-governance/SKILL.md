---
name: caf-governance
description: "CAF/ALZ guidance for the Governance design area: policy-driven guardrails with Azure Policy and initiatives, the ALZ default policies, compliance and regulatory frameworks, cost management, Enterprise Policy as Code (EPAC), and remediation. WHEN: \"governance design area\", \"azure policy\", \"policy initiative\", \"guardrails\", \"policy driven\", \"compliance\", \"regulatory compliance\", \"alz default policies\", \"deny/deployifnotexists\", \"remediation\", \"EPAC\", \"enterprise policy as code\", \"cost management\", \"budgets\", \"azgovviz\", \"governance visualizer\". DO NOT USE FOR: choosing a single RBAC role for a resource (use the azure-rbac skill); detailed cost-savings analysis of a live subscription (use the azure-cost-optimization skill); the management-group/subscription layout itself (use caf-resource-organization); deploying the platform (use the alz-accelerator skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Governance"
---

# Governance

## Overview

Governance turns design decisions into **enforced guardrails**. Using **Azure Policy** assigned
across the management group hierarchy, the platform mandates compliant configurations (audit, deny,
and auto-remediate) so workload teams operate freely *within* boundaries. ALZ ships a curated set of
**default policies** mapped to the design areas; the governance task is to adopt, tailor, and
operate them — often as **Enterprise Policy as Code (EPAC)** — alongside **cost** and **compliance**
controls.

## When to use this skill

- Designing the **policy strategy**: which initiatives, at which scopes, in which effect.
- Adopting and tailoring the **ALZ default policies** for your environment.
- Mapping controls to **regulatory/compliance frameworks** (e.g. ISO 27001, PCI, CIS).
- Operating policy as code with **EPAC** and planning **remediation** of non-compliant resources.
- Establishing **cost governance** (budgets, alerts, showback/chargeback).
- Visualizing the estate's governance posture (e.g. **Azure Governance Visualizer**).

## Key concepts

- **Azure Policy** — evaluates resources against rules. Effects include **Audit**, **Deny**,
  **DeployIfNotExists (DINE)**, and **Modify**; DINE/Modify enable auto-remediation.
- **Initiative (policy set)** — a bundle of policies assigned together (e.g. MCSB, ALZ defaults).
- **Assignment scope** — management group → subscription → resource group; assign high for
  inheritance, exempt narrowly where justified.
- **ALZ default policies** — opinionated guardrails aligned to the conceptual architecture and
  maintained in the ALZ reference implementations and Library.
- **Enterprise Policy as Code (EPAC)** — manage policy definitions, assignments, and exemptions
  through Git and pipelines for repeatability and review.
- **Cost management** — budgets, cost alerts, and tagging-driven allocation for accountability.

## Decision guidance

### Policy strategy

- Start from the **ALZ default policy set**; tailor rather than authoring from scratch.
- Assign initiatives at the **right management group** so inheritance does the work; reserve
  exemptions for genuine, documented exceptions.
- Choose effects deliberately: **Audit** to learn, **Deny** to prevent, **DINE/Modify** to enforce
  desired state and remediate.

### Policy as code and lifecycle

- Manage policy with **EPAC** (or the accelerator's pipelines) so changes are reviewed, versioned,
  and promoted across environments.
- Plan **remediation tasks** for DINE/Modify policies to bring existing resources into compliance.

### Cost and compliance

- Set **budgets and alerts** per scope; drive **showback/chargeback** from the tagging strategy.
- Map policy initiatives to required **compliance frameworks** and use Defender for Cloud /
  compliance dashboards for reporting.

## Recommended resources

- Governance design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/governance
- Adopt policy-driven guardrails — https://aka.ms/alz/adoptpolicy
- Policies in ALZ reference implementations — https://aka.ms/alz/policies
- Enterprise Policy as Code (EPAC) — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/policy-management/enterprise-policy-as-code
- Azure Governance Visualizer — https://aka.ms/alz/azgovviz
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-resource-organization` — the hierarchy policies are assigned across.
- Skill: `caf-security` — the security baseline (MCSB) enforced via policy.
- Skill: `caf-management` — AMBA and monitoring deployed at scale through DINE policies.
- Skill: `caf-platform-automation-devops` — pipelines that deliver policy as code.
- Agent: `azure-governance` — Azure Policy, AMBA, EPAC, and compliance operations.

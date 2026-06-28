---
name: alz-accelerator
description: "Technical skill for deploying an Azure landing zone with the official ALZ Accelerator. Covers the deployment phases (bootstrap → deploy), choosing the IaC variant (Terraform, Bicep on AVM, or Bicep classic), and migrating ALZ-Bicep classic to the AVM Bicep accelerator. Implementation specifics (Bicep/Terraform) live in references/. WHEN: \"ALZ accelerator\", \"deploy a landing zone\", \"bootstrap the accelerator\", \"alz bicep\", \"alz terraform\", \"avm platform landing zone\", \"starter module\", \"deploy ALZ with IaC\", \"phase 0/1/2/3\", \"bicep classic to avm\", \"migrate ALZ-Bicep\", \"which accelerator should I use\". DO NOT USE FOR: IaC-agnostic design-area decisions (use the caf-* skills); deploying an application workload to Azure (use the azure-deploy skill); subscription/landing zone vending modules (use the landing-zone-vending agent)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Platform automation and DevOps"
---

# ALZ Accelerator

## Overview

The **Azure Landing Zones Accelerator** turns the conceptual ALZ architecture into a deployable
platform: management groups, policies, the platform subscriptions (Connectivity, Identity,
Management), and the connectivity foundation, delivered as infrastructure as code. It ships in three
IaC flavors — **Terraform** and **Bicep** (both on **Azure Verified Modules**), plus the original
**ALZ-Bicep "classic"** framework. This skill is the **implementation layer**: it assumes the
architecture is already decided (the `caf-*` design-area skills) and focuses on *how* to bootstrap
and deploy it. Tool-specific detail lives in [`references/`](references/).

## When to use this skill

- Choosing between the **Terraform**, **Bicep (AVM)**, and **Bicep classic** accelerators.
- Running the **bootstrap** (repos, pipelines, deployment identity) before deploying the platform.
- Deploying and tailoring the **platform / starter modules** through the accelerator phases.
- Planning a migration from **ALZ-Bicep classic → Bicep (AVM)**.
- Keeping an accelerator deployment **up to date** with upstream module/AVM/policy releases.

## Key concepts

- **IaC variants:**
  - **`terraform`** — `Azure/alz-terraform-accelerator`, using **AVM for Terraform** (the platform
    landing zone module).
  - **`bicep`** — the current Bicep accelerator built on **Azure Verified Modules**.
  - **`bicep-classic`** — the original `Azure/ALZ-Bicep` framework (still supported; migrate to AVM
    Bicep when ready).
- **Phases** — the accelerator follows a phased path: **planning → prerequisites → getting started
  → deployment**. See [`references/phases.md`](references/phases.md).
- **Bootstrap** — one-time setup of the toolchain (Git repo, CI/CD, service connection, and a
  least-privilege **deployment identity** using OIDC/workload identity federation) that precedes
  platform deployment. See [`references/bootstrap.md`](references/bootstrap.md).
- **Starter / platform module** — the opinionated module that deploys the management groups,
  policies, and platform resources; you tailor it to your design.
- **Azure Verified Modules (AVM)** — Microsoft-verified building blocks. Reached via the `bicep`
  MCP server (`list_avm_metadata`) and the Terraform Registry; there is **no separate AVM server**.
- **Azure Landing Zones Library** — shared definitions (archetypes, policies) consumed by the
  accelerators.

## Decision guidance

### Choosing the IaC variant

- **Match team skills and existing investment.** Terraform-centric teams → the Terraform
  accelerator; Bicep/ARM teams → the Bicep (AVM) accelerator.
- **Greenfield Bicep → use Bicep (AVM)**, not classic. Reserve **`bicep-classic`** for existing
  ALZ-Bicep estates, and plan a migration to AVM.
- Both AVM variants are first-class; the choice is about tooling and skills, not capability. See
  [`references/bicep.md`](references/bicep.md) and [`references/terraform.md`](references/terraform.md).

### Sequencing the deployment

- Follow the **phases in order** ([`references/phases.md`](references/phases.md)); don't skip
  prerequisites (permissions, identities, providers).
- **Bootstrap first** with a secretless deployment identity
  ([`references/bootstrap.md`](references/bootstrap.md)), then deploy the platform module.
- **Plan/what-if before every apply**, gate production with approvals, and promote through
  environments.

### Migrating bicep-classic → bicep (AVM)

- Treat it as a **planned migration**, not an in-place swap: inventory current modules/policies, map
  them to AVM equivalents, and move incrementally with audit-first governance. See
  [`references/bicep-classic-migration.md`](references/bicep-classic-migration.md).

### Staying current

- Adopt upstream module/AVM/default-policy releases on a regular cadence and run drift detection
  (see `caf-platform-automation-devops` and https://aka.ms/alz/update).

## Recommended resources

- Azure Landing Zones Accelerator — https://aka.ms/alz/accelerator
- Terraform ALZ Accelerator — https://aka.ms/alz/tf/accelerator
- Bicep ALZ Accelerator — https://aka.ms/alz/bicep/accelerator
- Terraform AVM for Platform Landing Zone — https://aka.ms/alz/tf/module
- ALZ Bicep module repository — https://aka.ms/alz/bicep/repo
- Azure Landing Zones Library — https://aka.ms/alz/library
- Use infrastructure as code to update ALZ — https://aka.ms/alz/iac
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-platform-automation-devops` — the IaC/CI-CD practice this skill implements.
- Skill: `caf-resource-organization` — the hierarchy the accelerator deploys.
- Skill: `caf-governance` — the default policies bundled with the accelerator.
- Skill: `caf-network-topology-connectivity` — the connectivity foundation deployed by the platform module.
- Agent: `alz-accelerator-expert` — drives bootstrap and deployment end to end.
- Agent: `landing-zone-vending` — provisions workload subscriptions after the platform exists.

## References

- [`references/phases.md`](references/phases.md) — the accelerator deployment phases.
- [`references/bootstrap.md`](references/bootstrap.md) — bootstrap and deployment identity.
- [`references/bicep.md`](references/bicep.md) — the Bicep (AVM) accelerator.
- [`references/terraform.md`](references/terraform.md) — the Terraform accelerator.
- [`references/bicep-classic-migration.md`](references/bicep-classic-migration.md) — ALZ-Bicep classic → AVM Bicep.

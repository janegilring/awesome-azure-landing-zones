---
name: caf-platform-automation-devops
description: "CAF/ALZ guidance for the Platform automation and DevOps design area: infrastructure as code for the platform, the bootstrap/management of the deployment toolchain, CI/CD pipelines and Git workflow, identities for automation (managed identities, OIDC/workload identity federation), environments/promotion, and keeping the landing zone up to date. WHEN: \"platform automation\", \"devops design area\", \"infrastructure as code\", \"IaC strategy\", \"ci/cd pipeline\", \"github actions\", \"azure devops pipelines\", \"gitops\", \"deployment identity\", \"workload identity federation\", \"OIDC to azure\", \"keep alz up to date\", \"drift\", \"promote environments\", \"bootstrap automation\". DO NOT USE FOR: choosing between Bicep and Terraform accelerators or actual deployment steps (use the alz-accelerator skill); policy-as-code authoring (use caf-governance); deploying a specific app workload to Azure (use the azure-deploy skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Platform automation and DevOps"
---

# Platform automation and DevOps

## Overview

This design area makes the platform **repeatable and maintainable**: the landing zone is defined as
**infrastructure as code**, deployed through **CI/CD pipelines** with secure, secretless automation
identities, and **kept up to date** as Azure and the ALZ reference implementations evolve. It is
IaC-agnostic at the design level — the *practice* (Git workflow, environments, identities, drift
control) matters more than the specific tool; tool-specific steps live in the `alz-accelerator`
skill.

## When to use this skill

- Defining the **IaC strategy** and repository/branching model for the platform.
- Designing **CI/CD pipelines** (GitHub Actions or Azure DevOps) to deploy and update ALZ.
- Setting up **automation identities** with **managed identity** / **workload identity federation
  (OIDC)** instead of long-lived secrets.
- Planning **environments and promotion** (e.g. canary/management-group rings) and **drift** control.
- Establishing a process to **keep the landing zone current** with upstream module/policy updates.

## Key concepts

- **Platform as code** — the management groups, policies, networking, and baselines are defined in
  source control, reviewed, and deployed via pipelines — not click-ops.
- **Bootstrap** — the one-time setup of the deployment toolchain (repos, pipelines, service
  connections, deployment identity) before the platform itself is deployed.
- **Workload identity federation (OIDC)** — pipelines authenticate to Azure with short-lived,
  federated tokens; no stored secrets to rotate or leak.
- **Environments / promotion** — change flows through stages (plan → review → apply), often using
  management-group rings to limit blast radius.
- **Drift and updates** — detect configuration drift and consume upstream improvements (modules,
  AVM, default policies) on a maintained cadence (**keep ALZ up to date**).

## Decision guidance

### IaC and repository design

- Treat the platform as a **versioned product**: one source of truth, reviewed pull requests,
  tagged releases. Separate platform from workload pipelines.
- Pick the IaC tool that matches team skills and the chosen accelerator (Bicep or Terraform) — keep
  the *practice* consistent regardless (see `alz-accelerator` for specifics).

### Pipelines and identities

- Use **OIDC / workload identity federation** for pipeline-to-Azure auth; avoid stored service
  principal secrets entirely.
- Scope deployment identities with **least privilege** at the management-group level needed, not
  tenant-wide Owner.
- Gate production applies with **approvals** and run **plan/what-if** before apply.

### Keeping ALZ current

- Subscribe to upstream releases and adopt updates on a **regular cadence**; run drift detection so
  manual changes are caught and reconciled.
- Promote changes through **environment rings** to validate before broad rollout.

## Recommended resources

- Platform automation and DevOps design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/platform-automation-devops
- Use infrastructure as code to update ALZ — https://aka.ms/alz/iac
- Keep your Azure landing zone up to date — https://aka.ms/alz/update
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `alz-accelerator` — the Bicep/Terraform toolchains this practice drives.
- Skill: `caf-governance` — policy as code deployed through these pipelines.
- Skill: `caf-management` — operational baselines kept current via automation.
- Skill: `caf-identity-access-management` — identities and least-privilege for automation.
- Agent: `alz-accelerator-expert` (planned) — bootstrap and module deployment workflows.

---
name: caf-management
description: "CAF/ALZ guidance for the Management design area: platform operations baselines, the centralized Log Analytics workspace and Azure Monitor, Azure Monitor Baseline Alerts (AMBA), the Management subscription, backup and disaster recovery, patching/Update Manager, and the dev-test-prod management approach. WHEN: \"management design area\", \"monitoring baseline\", \"log analytics workspace\", \"azure monitor\", \"AMBA\", \"baseline alerts\", \"management subscription\", \"backup strategy\", \"disaster recovery\", \"business continuity\", \"BCDR\", \"update manager\", \"patching\", \"operational baseline\", \"platform operations\". DO NOT USE FOR: SIEM/threat detection with Sentinel or Defender (use caf-security); policy/compliance reporting (use caf-governance); debugging a live production incident (use the azure-diagnostics skill); deploying the platform (use the alz-accelerator skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Management"
---

# Management

## Overview

The Management design area defines the **operational baseline** for the platform: centralized
monitoring, alerting, backup, disaster recovery, and patching that workloads inherit. In the
conceptual ALZ architecture a dedicated **Management subscription** under the Platform management
group hosts the shared **Log Analytics workspace** and operational tooling. The aim is consistent,
**platform-provided observability and resiliency** so every landing zone starts with sensible
defaults rather than bespoke per-team operations.

## When to use this skill

- Designing the **centralized Log Analytics workspace** and Azure Monitor strategy.
- Deploying a **baseline alerting** capability — typically **Azure Monitor Baseline Alerts (AMBA)**.
- Planning **backup** and **disaster recovery** (BCDR) standards for platform and workloads.
- Defining **patching / update management** with Azure Update Manager.
- Differentiating operational rigor across **dev / test / prod** environments.

## Key concepts

- **Management subscription** — platform subscription hosting the shared Log Analytics workspace,
  Automation, and management tooling.
- **Centralized Log Analytics workspace** — single (or few) workspace(s) for platform and security
  logs; the foundation for Azure Monitor, Defender for Cloud, and Sentinel.
- **Azure Monitor Baseline Alerts (AMBA)** — Microsoft-maintained, opinionated alert definitions
  deployed at scale via policy, including an **AMBA-ALZ** pattern aligned to the management groups.
- **Backup and DR** — Azure Backup for data protection; Azure Site Recovery / region pairs and
  zone-redundancy for service continuity; defined RPO/RTO per tier.
- **Update Manager** — assess and orchestrate OS patching across machines.
- **Dev-test-prod (DTP) management** — match monitoring, backup, and patching intensity to
  environment criticality.

## Decision guidance

### Monitoring and alerting

- Centralize logs in the **Management subscription** workspace; decide retention by data type and
  cost. Avoid sprawling per-team workspaces unless isolation requires it.
- Adopt **AMBA (AMBA-ALZ)** for a maintained, policy-deployed alert baseline instead of hand-built
  alerts; tailor thresholds rather than starting from zero.

### Resiliency (BCDR)

- Set **RPO/RTO targets per workload tier** and choose protection accordingly (backup, zone
  redundancy, region-pair DR). Test restores, don't assume them.
- Provide platform-level backup defaults so workload teams inherit a safety net.

### Patching and environment tiers

- Standardize patching via **Update Manager**; schedule maintenance windows per environment.
- Apply **stronger operational controls in prod**, lighter in dev/test, using the DTP approach.

## Recommended resources

- Management design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/management
- Management for application environments (DTP) — https://aka.ms/alz/dtp
- Azure Monitor Baseline Alerts (AMBA) — https://aka.ms/amba
- AMBA ALZ pattern overview — https://aka.ms/amba/alz
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-security` — Defender and Sentinel build on the centralized workspace.
- Skill: `caf-governance` — policies that deploy AMBA and enforce monitoring at scale.
- Skill: `caf-resource-organization` — where the Management subscription sits in the hierarchy.
- Skill: `caf-platform-automation-devops` — pipelines that keep the management baseline current.
- Agent: `azure-architect` — incorporates the operations baseline into the overall design.

---
name: caf-security
description: "CAF/ALZ guidance for the Security design area: applying Zero Trust to a landing zone, Microsoft Defender for Cloud and secure score, Microsoft Sentinel, encryption and key/secret management, network security controls, and the CAF Secure methodology. WHEN: \"security design\", \"zero trust\", \"defender for cloud\", \"secure score\", \"sentinel\", \"siem\", \"encryption at rest\", \"key vault\", \"customer managed keys\", \"secrets management\", \"security baseline\", \"threat protection\", \"security posture\", \"MCSB\", \"microsoft cloud security benchmark\". DO NOT USE FOR: identity, RBAC, PIM, or Conditional Access design (use caf-identity-access-management); policy authoring/compliance reporting (use caf-governance); running an azqr/Key Vault expiry audit (use the azure-compliance skill); deploying the platform (use the alz-accelerator skill)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Security"
---

# Security

## Overview

The Security design area applies **Zero Trust** ("never trust, always verify") across the landing
zone and establishes the controls, monitoring, and encryption posture the platform enforces by
default. It is grounded in the **CAF Secure methodology** and the **Microsoft Cloud Security
Benchmark (MCSB)**, and it spans every other design area — identity, network, governance, and
management all carry security responsibilities. The goal is a **secure-by-default** platform so
workload teams inherit strong controls rather than building them per app.

## When to use this skill

- Defining the **security baseline** the platform enforces (Defender plans, MCSB via policy).
- Planning **threat detection and response** with Defender for Cloud and **Microsoft Sentinel**.
- Designing **encryption** (at rest/in transit, platform- vs customer-managed keys) and **secrets**
  management with Key Vault.
- Establishing **network security** controls (segmentation, firewall, DDoS, private access).
- Improving **secure score** / posture across subscriptions.

## Key concepts

- **Zero Trust** — verify explicitly, use least privilege, assume breach. Applies to identity,
  devices, network, apps, and data.
- **Microsoft Defender for Cloud (MDC)** — CSPM + CWPP: secure score, recommendations, and
  workload protection plans (servers, storage, SQL, containers, Key Vault, etc.).
- **Microsoft Cloud Security Benchmark (MCSB)** — the default control framework; assessed by MDC and
  enforceable via Azure Policy.
- **Microsoft Sentinel** — cloud-native SIEM/SOAR for detection, hunting, and response; typically
  fed by the platform Log Analytics workspace.
- **Encryption** — platform-managed keys by default; **customer-managed keys (CMK)** in Key Vault /
  Managed HSM where regulation requires key control.
- **Secrets management** — centralize secrets, keys, and certificates in **Key Vault**; prefer
  managed identities to avoid secrets entirely.

## Decision guidance

### Posture and threat protection

- Enable **Defender for Cloud** across subscriptions and turn on the **plans** relevant to your
  workloads; track and drive **secure score** improvements.
- Enforce the **MCSB** via policy initiative so new resources are compliant by default.
- Centralize logs and connect **Sentinel** to the platform Log Analytics workspace for org-wide
  detection (coordinate with `caf-management`).

### Encryption and secrets

- Use **encryption at rest by default**; adopt **CMK** only where you have a key-control mandate,
  and plan key lifecycle/rotation.
- Centralize secrets in **Key Vault** with RBAC, purge protection, and monitoring; eliminate
  embedded secrets by using managed identities.

### Network and data protection

- Apply **segmentation**, **Azure Firewall/NVA inspection**, **DDoS protection**, and **private
  endpoints** (see `caf-network-topology-connectivity`).
- Classify and protect data; restrict public network access to PaaS via Private Link.

## Recommended resources

- CAF Secure methodology — https://learn.microsoft.com/azure/cloud-adoption-framework/secure/
- Security design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/security
- Microsoft Zero Trust Assessment and Workshop — http://aka.ms/ztassess
- Microsoft Cloud Security Benchmark — https://learn.microsoft.com/security/benchmark/azure/
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-identity-access-management` — identity is the primary Zero Trust perimeter.
- Skill: `caf-management` — the shared Log Analytics workspace Sentinel and Defender rely on.
- Skill: `caf-governance` — policies that enforce the security baseline.
- Skill: `caf-network-topology-connectivity` — firewall, DDoS, and private-access controls.
- Agent: `governance-policy` (planned) — operationalizes security guardrails through policy.

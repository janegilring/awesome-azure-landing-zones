# Skills index

Skills packaged in this repository. Each skill is `skills/<name>/SKILL.md` with optional
`references/` deep-dives. See [`CONTRIBUTING.md`](../CONTRIBUTING.md) for the authoring template.

Status legend: ✅ available · 🟡 planned

## CAF/ALZ design-area skills (IaC-agnostic)

| Status | Design area | Skill | Description |
| :---: | :--- | :--- | :--- |
| 🟡 | 1. Azure billing and Microsoft Entra tenant | `caf-billing-entra-tenant` | Tenant creation, enrollment, billing, and multi-tenant decisions. |
| 🟡 | 2. Identity and access management | `caf-identity-access-management` | Entra ID, RBAC, PIM, and the identity security boundary. |
| 🟡 | 3. Resource organization | `caf-resource-organization` | Management group hierarchy and subscription organization. |
| ✅ | 4. Network topology and connectivity | [`caf-network-topology-connectivity`](../skills/caf-network-topology-connectivity/SKILL.md) | Hub-spoke vs Virtual WAN, DNS, private endpoints, hybrid connectivity. |
| 🟡 | 5. Security | `caf-security` | Defender for Cloud, Zero Trust, encryption, and secrets. |
| 🟡 | 6. Management | `caf-management` | Monitoring, AMBA, backup, and operational baselines. |
| 🟡 | 7. Governance | `caf-governance` | Azure Policy, cost, and compliance guardrails. |
| 🟡 | 8. Platform automation and DevOps | `caf-platform-automation-devops` | IaC, pipelines, and keeping ALZ up to date. |

## Technical skills

| Status | Skill | Description |
| :---: | :--- | :--- |
| 🟡 | `alz-accelerator` | Deploy ALZ with the Accelerator. Bicep and Terraform variants in `references/`. |

## Shared references

| Path | Description |
| :--- | :--- |
| [`skills/_shared/references/caf-link-catalog.md`](../skills/_shared/references/caf-link-catalog.md) | Curated CAF/ALZ links grouped by design area. |

# Skills index

Skills packaged in this repository. Each skill is `skills/<name>/SKILL.md` with optional
`references/` deep-dives. See [`CONTRIBUTING.md`](../CONTRIBUTING.md) for the authoring template.

Status legend: ✅ available · 🟡 planned

## CAF/ALZ design-area skills (IaC-agnostic)

| Status | Design area | Skill | Description |
| :---: | :--- | :--- | :--- |
| ✅ | 1. Azure billing and Microsoft Entra tenant | [`caf-billing-entra-tenant`](../skills/caf-billing-entra-tenant/SKILL.md) | Tenant creation, enrollment, billing, and multi-tenant decisions. |
| ✅ | 2. Identity and access management | [`caf-identity-access-management`](../skills/caf-identity-access-management/SKILL.md) | Entra ID, RBAC, PIM, and the identity security boundary. |
| ✅ | 3. Resource organization | [`caf-resource-organization`](../skills/caf-resource-organization/SKILL.md) | Management group hierarchy and subscription organization. |
| ✅ | 4. Network topology and connectivity | [`caf-network-topology-connectivity`](../skills/caf-network-topology-connectivity/SKILL.md) | Hub-spoke vs Virtual WAN, DNS, private endpoints, hybrid connectivity. |
| ✅ | 5. Security | [`caf-security`](../skills/caf-security/SKILL.md) | Defender for Cloud, Zero Trust, encryption, and secrets. |
| ✅ | 6. Management | [`caf-management`](../skills/caf-management/SKILL.md) | Monitoring, AMBA, backup, and operational baselines. |
| ✅ | 7. Governance | [`caf-governance`](../skills/caf-governance/SKILL.md) | Azure Policy, cost, and compliance guardrails. |
| ✅ | 8. Platform automation and DevOps | [`caf-platform-automation-devops`](../skills/caf-platform-automation-devops/SKILL.md) | IaC, pipelines, and keeping ALZ up to date. |

## Technical skills

| Status | Skill | Description |
| :---: | :--- | :--- |
| ✅ | [`alz-accelerator`](../skills/alz-accelerator/SKILL.md) | Deploy ALZ with the Accelerator. Bicep and Terraform variants in `references/`. |
| ✅ | [`landing-zone-vending`](../skills/landing-zone-vending/SKILL.md) | Subscription vending: request → deploy → govern → hand off, with the AVM sub-vending modules. |

## Shared references

| Path | Description |
| :--- | :--- |
| [`skills/_shared/references/caf-link-catalog.md`](../skills/_shared/references/caf-link-catalog.md) | Curated CAF/ALZ links grouped by design area. |

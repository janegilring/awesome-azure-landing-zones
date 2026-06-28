# Terraform accelerator (AVM for Platform Landing Zone)

Deep-dive companion to the `alz-accelerator` skill. The **Terraform ALZ Accelerator**
(`Azure/alz-terraform-accelerator`) deploys the platform using **AVM for Terraform**, primarily the
platform landing zone module, plus the Azure Landing Zones Library.

## What it deploys

- The **management group hierarchy** and **ALZ default policies** from the Library.
- The **platform subscriptions** scaffolding: Connectivity, Identity, Management.
- The **connectivity foundation** (hub-and-spoke or Virtual WAN) and management baseline, via AVM
  Terraform modules.

## Working with AVM in Terraform

- Find modules/providers/policies with the **`terraform` MCP server** (Registry tools); AVM modules
  are published to the **Terraform Registry**.
- Use the **AVM platform landing zone module** as the entry point; **pin module and provider
  versions** for reproducibility.

## State and identity

- Configure a **remote state backend** (e.g. an Azure Storage account) as part of bootstrap.
- Authenticate the pipeline with **OIDC / workload identity federation** — no stored secrets
  (`bootstrap.md`).
- Scope the deployment identity to the required **management-group** level.

## Checklist

1. Confirm **architecture inputs** (from the `caf-*` skills) and the management-group design.
2. Configure the **remote state backend** and **OIDC** deployment identity (`bootstrap.md`).
3. Pin **AVM module/provider versions**; consume the **ALZ Library** for archetypes/policies.
4. Run **`terraform plan`**, review, then **apply** behind approvals.
5. Tailor **policies and connectivity**; establish the update cadence and drift detection.

## Sources

- Terraform ALZ Accelerator — https://aka.ms/alz/tf/accelerator
- Terraform AVM for Platform Landing Zone — https://aka.ms/alz/tf/module
- Azure Landing Zones Library — https://aka.ms/alz/library
- Use infrastructure as code to update ALZ — https://aka.ms/alz/iac

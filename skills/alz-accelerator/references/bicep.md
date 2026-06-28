# Bicep accelerator (Azure Verified Modules)

Deep-dive companion to the `alz-accelerator` skill. The current **Bicep ALZ Accelerator** deploys
the platform using **Azure Verified Modules (AVM)** and the Azure Landing Zones Library.

## What it deploys

- The **management group hierarchy** and the **ALZ default policies** (via the Library archetypes).
- The **platform subscriptions** scaffolding: Connectivity, Identity, Management.
- The **connectivity foundation** (hub-and-spoke or Virtual WAN) and management/monitoring baseline,
  composed from AVM resource modules.

## Working with AVM in Bicep

- Discover module metadata with the **`bicep` MCP server**: `list_avm_metadata` (available modules
  and versions) and `get_az_resource_type_schema` (resource schemas).
- Reference AVM modules from the public registry (`br/public:avm/...`); **pin versions** for
  reproducibility.
- Use the `bicep` server's best-practice/diagnostics tools to keep templates clean.

## Deployment scope

- ALZ Bicep deploys at **tenant / management-group scope** for the hierarchy and policy, and at
  **subscription scope** for platform resources — ensure the deployment identity has the matching
  management-group permissions (see `bootstrap.md`).
- Validate with **what-if** (and deployment stacks where used) before apply.

## Checklist

1. Confirm the **architecture inputs** (from the `caf-*` skills) and the management-group design.
2. Pin **AVM module versions**; consume the **ALZ Library** for archetypes/policies.
3. Configure the **deployment identity** with management-group scope (`bootstrap.md`).
4. Run **what-if**, review, then apply.
5. Tailor **policies and connectivity** to the design; establish the update cadence.

## Sources

- Bicep ALZ Accelerator — https://aka.ms/alz/bicep/accelerator
- ALZ Bicep module repository — https://aka.ms/alz/bicep/repo
- Azure Landing Zones Library — https://aka.ms/alz/library
- Use infrastructure as code to update ALZ — https://aka.ms/alz/iac

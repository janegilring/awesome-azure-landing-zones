# Migrating ALZ-Bicep classic to the AVM Bicep accelerator

Deep-dive companion to the `alz-accelerator` skill. The original **`Azure/ALZ-Bicep` ("classic")**
framework is still supported, but new and evolving deployments should target the **AVM-based Bicep
accelerator**. Treat the move as a **planned migration**, not an in-place swap.

## Why migrate

| Aspect | Bicep classic (`Azure/ALZ-Bicep`) | Bicep accelerator (AVM) |
| :--- | :--- | :--- |
| Module basis | Bespoke ALZ modules | Azure Verified Modules (Microsoft-verified, versioned) |
| Library integration | Limited | Consumes the Azure Landing Zones Library |
| Long-term direction | Maintenance | Strategic / actively evolving |
| Reuse across IaC | Bicep-only | Shared archetypes/policies with the Library |

## Migration approach

1. **Inventory** the current deployment: modules in use, custom parameters, policy assignments and
   exemptions, and any bespoke resources.
2. **Map to AVM** equivalents — match each classic module to its AVM module and the Library
   archetype/policy set; note gaps.
3. **Stand up the AVM accelerator in parallel** (new repo/pipeline, `bootstrap.md`), targeting the
   same management-group hierarchy.
4. **Reconcile state** — management groups, subscriptions, and policy assignments are largely
   declarative; plan how existing assignments are adopted vs replaced. Use **what-if** extensively.
5. **Move incrementally and audit-first** — shift policy to **Audit** during transition to avoid
   disrupting running workloads, then re-enforce.
6. **Cut over and decommission** the classic pipeline once parity is verified.

## Cautions

- **Don't dual-manage** the same scope from two pipelines simultaneously — pick the source of truth
  per scope during the transition.
- **Pin AVM versions** and validate with **what-if** before every apply.
- Keep **governance non-disruptive**: audit → remediate → enforce.

## Sources

- Bicep ALZ Accelerator — https://aka.ms/alz/bicep/accelerator
- ALZ Bicep module repository (classic) — https://aka.ms/alz/bicep/repo
- Azure Landing Zones Library — https://aka.ms/alz/library
- Transition existing environments to the ALZ architecture — https://aka.ms/alz/brownfield

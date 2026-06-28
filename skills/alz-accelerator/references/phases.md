# ALZ Accelerator deployment phases

Deep-dive companion to the `alz-accelerator` skill. The accelerator follows a phased path so that
prerequisites (identity, permissions, providers) are in place before the platform is deployed.

## The phases

| Phase | Name | Purpose |
| :--- | :--- | :--- |
| 0 | Planning | Decide architecture inputs, IaC variant, regions, and naming; confirm subscriptions exist. |
| 1 | Prerequisites | Permissions, deployment identity, registered resource providers, tooling. |
| 2 | Getting started | Bootstrap the toolchain (repo, pipelines, service connection) — see `bootstrap.md`. |
| 3 | Deployment | Deploy and tailor the platform / starter module; validate and operate. |

> The architecture decisions in Phase 0 come from the IaC-agnostic `caf-*` design-area skills. The
> accelerator implements them; it does not replace the design work.

## Phase checklist

1. **Plan (0):** confirm tenant, billing, target management group structure, regions, and the IaC
   variant (Terraform, Bicep AVM, or Bicep classic).
2. **Prerequisites (1):** elevate/assign the required management-group permissions, create the
   deployment identity, register providers, and install CLI tooling.
3. **Getting started (2):** bootstrap repos, CI/CD, and a secretless deployment identity
   (OIDC/workload identity federation).
4. **Deployment (3):** run plan/what-if, review, then apply the platform module; tailor policies and
   connectivity to the design.
5. **Operate:** establish the update cadence and drift detection.

## Sources

- Azure Landing Zones Accelerator — https://aka.ms/alz/accelerator
- Phase 0: planning — https://aka.ms/alz/acc/phase0
- Phase 1: prerequisites — https://aka.ms/alz/acc/phase1
- Phase 2: getting started — https://aka.ms/alz/acc/phase2
- Phase 3: deployment — https://aka.ms/alz/acc/phase3

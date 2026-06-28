# Bootstrap and deployment identity

Deep-dive companion to the `alz-accelerator` skill. The **bootstrap** is the one-time setup of the
deployment toolchain that precedes platform deployment: source control, CI/CD, and a secure,
least-privilege identity for pipelines to authenticate to Azure.

## What bootstrap provisions

- A **Git repository** for the platform-as-code (GitHub or Azure DevOps).
- **CI/CD pipelines / workflows** that run plan/what-if and apply.
- A **deployment identity** (managed identity or app registration) with a federated credential.
- The **service connection / OIDC trust** between the pipeline and Azure.
- Initial **state/backend** configuration (Terraform state storage, where applicable).

## Deployment identity — do it secretless

| Option | Use it? | Notes |
| :--- | :--- | :--- |
| **Workload identity federation (OIDC)** | ✅ Preferred | Short-lived federated tokens; no stored secrets to rotate or leak. |
| **User-assigned managed identity** | ✅ Where supported | No credentials in the pipeline; assign least-privilege roles. |
| **Service principal + client secret** | ⚠️ Avoid | Long-lived secret to store and rotate; last resort only. |

- Scope the identity to the **management-group level it actually needs** (often `Owner` or a
  tailored role at the intermediate root for the initial deployment) — not tenant-wide by habit, and
  tighten after initial setup.
- Protect the identity and its role assignments; review them periodically.

## Bootstrap checklist

1. Choose the **VCS + CI/CD** (GitHub Actions or Azure DevOps).
2. Create the **deployment identity** and configure **OIDC/workload identity federation**.
3. Assign **least-privilege** roles at the required management-group scope.
4. Configure the **pipeline** (plan/what-if on PR, apply on approval to main).
5. Configure the **state backend** (Terraform) or deployment stacks (Bicep), as applicable.
6. Verify with a **no-op plan/what-if** before the first real deployment.

## Sources

- Azure Landing Zones Accelerator — https://aka.ms/alz/accelerator
- Phase 1: prerequisites — https://aka.ms/alz/acc/phase1
- Phase 2: getting started — https://aka.ms/alz/acc/phase2
- Use infrastructure as code to update ALZ — https://aka.ms/alz/iac

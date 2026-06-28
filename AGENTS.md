# AGENTS.md — Awesome Azure Landing Zones

This repository is a library of **AI Skills** and **Agents** for Microsoft Cloud Adoption
Framework (CAF) and Azure Landing Zones (ALZ). It gives AI coding agents reusable, grounded
domain knowledge for designing, deploying, governing, and operating Azure landing zones.

> All artifacts in this repository are authored in **English**.

## Repository layout

| Path | Purpose |
| :--- | :--- |
| `skills/` | One folder per skill. Each contains a `SKILL.md` and an optional `references/` folder for deep-dive content. |
| `agents/` | One `*.agent.md` per specialized agent persona. |
| `docs/` | Index tables: [`README.skills.md`](docs/README.skills.md) and [`README.agents.md`](docs/README.agents.md). End-to-end [`scenarios.md`](docs/scenarios.md). |
| `skills/_shared/` | Shared references used by multiple skills (e.g. the curated CAF link catalog). |
| `.vscode/mcp.json` | MCP server configuration that gives the agents live tools (Azure, Terraform, Bicep, draw.io, docs). |
| `CONTRIBUTING.md` | Authoring conventions for skills and agents. |
| `plugin.json` | Copilot CLI plugin manifest: makes the whole repo installable as one plugin. |
| `.github/plugin/marketplace.json` | Marketplace manifest: publishes the plugin for `copilot plugin marketplace add` + install by name. |
| `README.md` | Human-facing overview and install instructions (Copilot CLI plugin / marketplace). |
| `docs/resources.md` | Curated `aka.ms` link catalog and other useful resources. |

## What is a skill vs. an agent?

- **Skill** (`skills/<name>/SKILL.md`) — packaged, on-demand domain knowledge for **one topic**.
  Loaded into context only when its triggers match. Design-area skills are **IaC-agnostic**
  (conceptual CAF guidance); only the ALZ Accelerator skill splits Bicep vs Terraform.
- **Agent** (`agents/<name>.agent.md`) — a **persona** with a scope, a workflow, and a set of
  linked skills it relies on. Use agents to frame multi-step engagements (architecture,
  migration, governance, networking, vending, accelerator deployment).

## Skills in this repository

Design-area skills map 1:1 to the eight CAF/ALZ design areas, plus a technical ALZ Accelerator skill.

| Design area / topic | Skill |
| :--- | :--- |
| Azure billing and Microsoft Entra tenant | [`skills/caf-billing-entra-tenant/`](skills/caf-billing-entra-tenant/SKILL.md) |
| Identity and access management | [`skills/caf-identity-access-management/`](skills/caf-identity-access-management/SKILL.md) |
| Resource organization | [`skills/caf-resource-organization/`](skills/caf-resource-organization/SKILL.md) |
| Network topology and connectivity | [`skills/caf-network-topology-connectivity/`](skills/caf-network-topology-connectivity/SKILL.md) |
| Security | [`skills/caf-security/`](skills/caf-security/SKILL.md) |
| Management | [`skills/caf-management/`](skills/caf-management/SKILL.md) |
| Governance | [`skills/caf-governance/`](skills/caf-governance/SKILL.md) |
| Platform automation and DevOps | [`skills/caf-platform-automation-devops/`](skills/caf-platform-automation-devops/SKILL.md) |
| ALZ Accelerator (Bicep + Terraform) | [`skills/alz-accelerator/`](skills/alz-accelerator/SKILL.md) |
| Landing zone (subscription) vending | [`skills/landing-zone-vending/`](skills/landing-zone-vending/SKILL.md) |

> All eight design-area skills, the ALZ Accelerator skill, and all six agents now exist.
> See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the authoring template.

## Agents in this repository

| Agent | Scope |
| :--- | :--- |
| [`networking-connectivity`](agents/networking-connectivity.agent.md) | Hub-spoke, Virtual WAN, private DNS, private endpoints, hybrid connectivity. |
| [`azure-architect`](agents/azure-architect.agent.md) | CAF/WAF design-area decisions across the whole landing zone. |
| [`alz-accelerator-expert`](agents/alz-accelerator-expert.agent.md) | Bootstrap + starter modules for the ALZ Accelerator. |
| [`azure-migration`](agents/azure-migration.agent.md) | Azure Migrate and brownfield → ALZ adoption. |
| [`governance-policy`](agents/governance-policy.agent.md) | Azure Policy, AMBA, EPAC, compliance. |
| [`landing-zone-vending`](agents/landing-zone-vending.agent.md) | Subscription / landing zone vending. |

## MCP servers

The skills supply the *knowledge*; the MCP servers in [`.vscode/mcp.json`](.vscode/mcp.json)
supply the *tools* the agents act with. Skills guide, MCP executes. VS Code starts these servers
automatically (Agent mode → tools picker).

| Server | Transport | Purpose | Used by |
| :--- | :--- | :--- | :--- |
| `microsoft-learn` | http | Ground answers in official Microsoft/Azure docs. | **All agents.** |
| `github` | http | Fetch the latest from official ALZ repos (`Azure/ALZ-Bicep`, `Azure/alz-terraform-accelerator`, `Azure/Azure-Landing-Zones-Library`). | `alz-accelerator-expert`, `landing-zone-vending`, `azure-migration`. |
| `azure` | stdio (`npx @azure/mcp`) | Azure MCP Server — 200+ tools across 40+ services for live resource-level queries (resources, pricing, logs, diagnostics). | Agents that query/operate a live environment. |
| `azure-resource-manager` | http | Governance-level live queries: management-group hierarchy, tenant-wide Resource Graph, cross-subscription enumeration. | `azure-architect`, `governance-policy`, `azure-migration`. |
| `terraform` | stdio (Docker) | Terraform Registry — providers, modules (incl. **AVM for Terraform**), policies. | `alz-accelerator-expert`, `landing-zone-vending`. |
| `bicep` | stdio (`dnx`) | Bicep best practices, diagnostics, formatting, resource schemas, and **AVM metadata** (`list_avm_metadata`). | `alz-accelerator-expert`, `landing-zone-vending`. |
| `drawio` | stdio (`npx @drawio/mcp`) | Search Azure shape styles and render architecture / topology diagrams. | `networking-connectivity`, `azure-architect`. |

### Azure Verified Modules (AVM)

There is no separate "AVM MCP" server. AVM is reached through the IaC servers above:

- **Bicep** → the `bicep` server's `list_avm_metadata` tool (and `get_az_resource_type_schema`).
- **Terraform** → the `terraform` server's registry tools (AVM modules are published to the
  Terraform Registry).

### Infrastructure-as-Code awareness

The ALZ Accelerator ships in three IaC flavors; agents and the `alz-accelerator` skill must keep
these distinct and help migrate between them:

- **`terraform`** — `Azure/alz-terraform-accelerator` (uses AVM for Terraform).
- **`bicep`** — the current Bicep accelerator built on Azure Verified Modules.
- **`bicep-classic`** — the original `Azure/ALZ-Bicep` ("Classic") framework. When asked, help
  plan and migrate **`bicep-classic` → `bicep` (AVM)**.

### Related: Azure Skills Plugin

Microsoft's [Azure Skills Plugin](https://devblogs.microsoft.com/all-things-azure/announcing-the-azure-skills-plugin/)
(`aka.ms/azure-plugin`) bundles 19+ general Azure skills powered by the **Azure MCP Server** (which
includes Microsoft Foundry tooling) and the **context7** MCP server. This repository is
**complementary and CAF/ALZ-focused**: it adds landing-zone design-area expertise and ALZ-specific
agents on top of the same skills-plus-MCP pattern. The Azure MCP Server is shared between the two;
the **Microsoft Learn MCP** server, however, is **not** part of the Azure Skills Plugin — it is
declared in this repo's [`.vscode/mcp.json`](.vscode/mcp.json). Install the Azure Skills Plugin for
broad app-to-production Azure workflows (and as a prerequisite for scenarios that use its skills,
e.g. `azure-prepare` / `azure-deploy` / `microsoft-foundry`); use these skills/agents for
landing-zone architecture, governance, and accelerator work.

## Conventions for agents working in this repo

- **Ground every claim.** Prefer Microsoft Learn (`learn.microsoft.com`) and the official ALZ
  repositories. Reuse the curated links in
  [`skills/_shared/references/caf-link-catalog.md`](skills/_shared/references/caf-link-catalog.md)
  and [`docs/resources.md`](docs/resources.md).
- **Keep design-area skills IaC-agnostic.** Put Bicep/Terraform specifics in the ALZ Accelerator
  skill's `references/`.
- **Cross-link.** Each skill lists related skills and agents; each agent lists the skills it uses.
- **Declare MCP tools.** Each agent lists the MCP servers/tools it relies on (see the convention
  in [`CONTRIBUTING.md`](CONTRIBUTING.md)). Prefer live tools over guessing; never deploy without
  user confirmation.
- **Frontmatter.** Every `SKILL.md` starts with the YAML frontmatter described in
  [`CONTRIBUTING.md`](CONTRIBUTING.md). The `name` must match the folder name.
- **No customer secrets or tenant-specific data** in any artifact.
- **English only**, US spelling, sentence-case headings.

## Validating changes

- Confirm new/changed `SKILL.md` frontmatter parses and `name` matches the folder.
- Confirm every relative link resolves.
- Update the index tables in `docs/` and the tables above when adding a skill or agent.

# Awesome Azure Landing Zones

A library of **AI Skills** and **Agents** for the Microsoft **Cloud Adoption Framework (CAF)**
and **Azure Landing Zones (ALZ)**. It gives AI coding agents (GitHub Copilot, Copilot CLI,
Claude Code, and other Agent Skills-compatible tools) reusable, grounded domain knowledge for
designing, deploying, governing, and operating Azure landing zones.

> All artifacts in this repository are authored in **English** (US spelling, sentence-case headings).

## What's in here

- **Skills** (`skills/<name>/SKILL.md`) — packaged, on-demand domain knowledge for one topic.
  Design-area skills map 1:1 to the eight CAF/ALZ design areas and are **IaC-agnostic**
  (conceptual guidance). The ALZ Accelerator skill splits Bicep vs Terraform.
- **Agents** (`agents/<name>.agent.md`) — personas with a scope, a workflow, and the skills they
  rely on (architecture, migration, governance, networking, vending, accelerator deployment).
- **MCP servers** (`.vscode/mcp.json`) — live tools the agents act with (Microsoft Learn, GitHub,
  Azure, Azure Resource Manager, Terraform, Bicep, draw.io). Skills guide; MCP executes.

See [`AGENTS.md`](AGENTS.md) for the full overview, the skills/agents index in
[`docs/`](docs/), and the authoring conventions in [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Repository layout

| Path | Purpose |
| :--- | :--- |
| [`AGENTS.md`](AGENTS.md) | Repository overview for agents: skills/agents index, MCP servers, conventions. |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Authoring conventions for skills and agents. |
| `skills/` | One folder per skill (`SKILL.md` + optional `references/`). |
| `agents/` | One `*.agent.md` per specialized agent persona. |
| `docs/` | Index tables ([skills](docs/README.skills.md), [agents](docs/README.agents.md)) and the curated [resources / link catalog](docs/resources.md). |
| `.vscode/mcp.json` | MCP server configuration that gives the agents live tools. |

## Design areas

The design-area skills follow the eight official CAF/ALZ design areas:

1. Azure billing and Microsoft Entra tenant
2. Identity and access management
3. Resource organization
4. Network topology and connectivity
5. Security
6. Management
7. Governance
8. Platform automation and DevOps

Plus a technical **ALZ Accelerator** skill (Bicep + Terraform).

## Installing the skills and agents

The skills and agents in this repository follow the GitHub **Agent Skills** convention, so you can
install them with the [GitHub CLI](https://cli.github.com/), clone them manually, or copy
individual folders into a target project.

### Prerequisites

```powershell
# GitHub CLI (https://cli.github.com)
winget install --id GitHub.cli
gh auth login

# The skills extension for `gh skill ...` (https://cli.github.com/manual/gh_skill_install)
gh extension install github/gh-skill
```

### Discover skills

```powershell
# Search GitHub for Agent Skills (SKILL.md files)
# Note: GitHub code search only returns results once the repo has been indexed,
# which can take a while for brand-new public repositories.
gh search code '"SKILL.md" path:skills' --owner janegilring --limit 10

# Browse the community catalog of instructions, agents, and skills
Start-Process "https://awesome-copilot.github.com/"
```

### Install a single skill or agent with the GitHub CLI

```powershell
# Install one skill into the current project's .agents/skills folder
gh skill install janegilring/awesome-azure-landing-zones caf-network-topology-connectivity

# Reload VS Code so the new skill is picked up:
#   Ctrl+Shift+P → "Developer: Reload Window"
```

### Install manually (clone and copy)

Use this when you want everything, or when `gh skill` is not available:

```powershell
# Clone the repository
git clone https://github.com/janegilring/awesome-azure-landing-zones.git "$env:TEMP\awesome-azure-landing-zones"

# Copy the skills and agents into your project (same layout `gh skill` uses)
New-Item -ItemType Directory -Force -Path .agents\skills, .agents\agents | Out-Null
Copy-Item -Recurse "$env:TEMP\awesome-azure-landing-zones\skills\*"  .agents\skills\
Copy-Item -Recurse "$env:TEMP\awesome-azure-landing-zones\agents\*"  .agents\agents\

# Confirm what landed
Get-ChildItem .agents\skills -Directory
Get-ChildItem .agents\agents -File
```

### Use a skill

Once installed and the VS Code window is reloaded, reference the skill by name in your prompt, e.g.:

> Design connectivity for a multi-region landing zone. Use the `caf-network-topology-connectivity` skill.

## Resources

The curated catalog of CAF/ALZ `aka.ms` links and other useful resources has moved to
[`docs/resources.md`](docs/resources.md). A topic-grouped subset used by the skills lives in
[`skills/_shared/references/caf-link-catalog.md`](skills/_shared/references/caf-link-catalog.md).

## Related: Azure Skills Plugin

Microsoft's [Azure Skills Plugin](https://devblogs.microsoft.com/all-things-azure/announcing-the-azure-skills-plugin/)
(`aka.ms/azure-plugin`) bundles 19+ general Azure skills plus the Azure MCP and Foundry MCP servers.
This repository is **complementary and CAF/ALZ-focused** — it adds landing-zone design-area
expertise and ALZ-specific agents on top of the same skills-plus-MCP pattern.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the frontmatter contract, IaC-neutrality rules,
grounding sources, and the definition of done.

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
| `docs/` | Index tables ([skills](docs/README.skills.md), [agents](docs/README.agents.md)), end-to-end [scenarios](docs/scenarios.md), and the curated [resources / link catalog](docs/resources.md). |
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

This repository is a **GitHub Copilot CLI plugin** *and* a single-plugin **marketplace**: the
`agents/` and `skills/` folders plus the [`plugin.json`](plugin.json) manifest make the whole
library installable as one unit, and [`.github/plugin/marketplace.json`](.github/plugin/marketplace.json)
publishes it so it can be discovered and installed by name. Install everything in one command,
enable it declaratively for a team, or copy individual folders manually.

### Install everything with the Copilot CLI (recommended)

Requires the [GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/copilot-cli/about-cli).

```powershell
# Register this repo as a marketplace, then install the plugin (installs ALL skills and agents)
copilot plugin marketplace add janegilring/awesome-azure-landing-zones
copilot plugin install awesome-azure-landing-zones@awesome-azure-landing-zones

# Confirm it installed
copilot plugin list
```

Then start `copilot` and reference any skill or agent by name in your prompt.

> You can also install directly from the repo with
> `copilot plugin install janegilring/awesome-azure-landing-zones`. This still works today, but the
> Copilot CLI is **deprecating direct repo/URL installs** in favor of `plugin@marketplace`, so the
> marketplace flow above is the durable option.

### Enable it declaratively (teams)

Add the plugin to the `enabledPlugins` field of a user-level `~/.copilot/settings.json` or a
repository-level `.github/copilot/settings.json` so everyone on the project gets it automatically:

```json
{
  "enabledPlugins": ["awesome-azure-landing-zones@awesome-azure-landing-zones"]
}
```

### Update to the latest version

All skills and agents ship as a single plugin, so one update refreshes everything — changed
skills **and** any newly added ones. There is no separate install per skill.

```powershell
# Refresh the marketplace catalog, then update the plugin
copilot plugin marketplace update awesome-azure-landing-zones
copilot plugin update awesome-azure-landing-zones@awesome-azure-landing-zones

# Or update every installed plugin at once
copilot plugin update --all
```

> Re-running `copilot plugin install awesome-azure-landing-zones@awesome-azure-landing-zones` also
> pulls the latest, but `update` is the intended command. If you installed manually, re-run the
> clone-and-copy steps below to update.

### Install manually (clone and copy)

Use this when you are not using the Copilot CLI plugin system, or want only specific folders.
Skills and agents are picked up from your personal `~/.copilot` folders:

```powershell
git clone https://github.com/janegilring/awesome-azure-landing-zones.git "$env:TEMP\awesome-azure-landing-zones"

New-Item -ItemType Directory -Force -Path "$HOME\.copilot\skills", "$HOME\.copilot\agents" | Out-Null
Copy-Item -Recurse -Force "$env:TEMP\awesome-azure-landing-zones\skills\*" "$HOME\.copilot\skills\"
Copy-Item -Recurse -Force "$env:TEMP\awesome-azure-landing-zones\agents\*" "$HOME\.copilot\agents\"
```

> Skills are deduplicated by their `name` field and agents by file name, and project-level copies
> take precedence over a plugin if both are present.

### Use a skill or agent

Reference it by name in your prompt, e.g.:

> Design connectivity for a multi-region landing zone. Use the `caf-network-topology-connectivity` skill.

## Scenarios and example usage

For end-to-end walkthroughs that combine multiple skills and agents, see
[`docs/scenarios.md`](docs/scenarios.md). The initial example —
[self-service subscription vending with a request intake](docs/scenarios.md#scenario-1--self-service-subscription-vending-with-a-request-intake) —
shows how to define the vending backend first and then build a web (Container Apps) or AI (Foundry)
self-service intake on top of it.

## Resources

The curated catalog of CAF/ALZ `aka.ms` links and other useful resources has moved to
[`docs/resources.md`](docs/resources.md). A topic-grouped subset used by the skills lives in
[`skills/_shared/references/caf-link-catalog.md`](skills/_shared/references/caf-link-catalog.md).

## Related: Azure Skills Plugin

Microsoft's [Azure Skills Plugin](https://devblogs.microsoft.com/all-things-azure/announcing-the-azure-skills-plugin/)
(`aka.ms/azure-plugin`) bundles 19+ general Azure skills powered by the **Azure MCP Server** (which
includes Microsoft Foundry tooling) and the **context7** MCP server. This repository is
**complementary and CAF/ALZ-focused** — it adds landing-zone design-area expertise and ALZ-specific
agents on top of the same skills-plus-MCP pattern.

Install it (`copilot plugin marketplace add microsoft/azure-skills` then
`copilot plugin install azure@azure-skills`) when a scenario here uses one of its skills — for
example [`docs/scenarios.md`](docs/scenarios.md) references `azure-prepare`, `azure-deploy`, and
`microsoft-foundry`, which ship in **that** plugin, not this one. Note that the **Microsoft Learn
MCP** server is **not** part of the Azure Skills Plugin; in this repo it is provided by
[`.vscode/mcp.json`](.vscode/mcp.json) for VS Code Agent mode.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the frontmatter contract, IaC-neutrality rules,
grounding sources, and the definition of done.

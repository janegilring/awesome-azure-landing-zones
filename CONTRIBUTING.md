# Contributing — authoring skills and agents

This guide defines the conventions for authoring **skills** and **agents** in this repository.
Follow it so artifacts stay consistent, discoverable, and easy for AI agents to consume.

> **Language:** All artifacts are written in **English** (US spelling, sentence-case headings).

## 1. Skills

### 1.1 Folder layout

```text
skills/<skill-name>/
  SKILL.md            # required — frontmatter + body
  references/         # optional — deep-dive markdown loaded on demand
    <topic>.md
```

- `<skill-name>` is **kebab-case** and must equal the `name` in the frontmatter.
- Design-area skills use the prefix `caf-` (e.g. `caf-security`).
- Keep `SKILL.md` concise (a routing/overview document). Push depth into `references/`.

### 1.2 Required frontmatter

```yaml
---
name: caf-network-topology-connectivity
description: "<1–2 sentence purpose>. WHEN: \"trigger phrase\", \"another trigger\". DO NOT USE FOR: <out-of-scope>."
license: MIT
metadata:
  author: <org or team>
  version: "0.1.0"
  designArea: "Network topology and connectivity"   # design-area skills only
---
```

Rules:

- `name` matches the folder name exactly.
- `description` starts with the purpose, then a `WHEN:` list of natural-language triggers, and a
  `DO NOT USE FOR:` clause that routes to the correct alternative skill/agent. This drives
  retrieval, so be specific.
- `version` uses semver; new skills start at `0.1.0`.

### 1.3 Recommended body structure

1. `# <Title>` — human-readable name.
2. **Overview** — what the design area is and why it matters (2–4 sentences).
3. **When to use this skill** — bullet list mirroring the `WHEN:` triggers.
4. **Key concepts** — the vocabulary an agent needs.
5. **Decision guidance / checklist** — actionable, opinionated recommendations.
6. **Recommended resources** — curated Learn + repo links (reuse the link catalog).
7. **Related skills and agents** — cross-links.
8. **References** — point to files in `references/` for depth.

### 1.4 IaC neutrality

- Design-area skills (`caf-*`) are **IaC-agnostic** — describe concepts and decisions, not
  Bicep/Terraform syntax.
- Implementation specifics (modules, providers, `inputs.yaml`, `Deploy-Accelerator`,
  `terraform apply`, `az deployment`) belong in `skills/alz-accelerator/references/`.

## 2. Agents

### 2.1 File layout

```text
agents/<agent-name>.agent.md
```

`<agent-name>` is kebab-case. Agents are plain Markdown (per the AGENTS.md format) with optional
frontmatter for tooling that supports it.

### 2.2 Recommended frontmatter

```yaml
---
name: azure-networking
description: "<one-line persona summary>."
---
```

### 2.3 Recommended body structure

1. **Role / persona** — who the agent is and its expertise.
2. **Scope** — what it covers and explicit boundaries (what it does *not* do).
3. **When to engage** — trigger scenarios.
4. **Workflow** — the decision process / steps it follows.
5. **Skills it uses** — links to `skills/` this agent relies on.
6. **MCP servers / tools** — which servers from [`.vscode/mcp.json`](.vscode/mcp.json) the agent
   uses and for what (see §2.4).
7. **Key references** — Learn + repos.
8. **Guardrails** — safety/scope constraints (e.g. never apply changes without confirmation).

### 2.4 Declaring MCP servers and tools

Skills carry the knowledge; the MCP servers in [`.vscode/mcp.json`](.vscode/mcp.json) give the
agent live tools. Every agent must declare which servers it relies on.

- **List them in an "MCP servers / tools" body section** with the server name, the specific tools,
  and the purpose. Reference the table in [`AGENTS.md`](AGENTS.md) → "MCP servers".
- **`microsoft-learn` is implicit for every agent** (grounding). Still mention it if the agent
  leans on it heavily.
- **Prefer the explicit `{server}/{tool}` form over wildcards** in any frontmatter `tools:` line
  that tooling supports, e.g.
  `tools: search, 'azure-resource-manager/execute_query', 'drawio/search_shapes'`. Wildcards
  (`'azure/*'`) widen the surface and are fine for exploratory agents but less precise.
- **AVM has no dedicated server.** For Bicep AVM use the `bicep` server's `list_avm_metadata`; for
  Terraform AVM use the `terraform` server's registry tools.
- **Never deploy or mutate** a live environment from a tool call without explicit user confirmation.

## 3. Grounding and sources

- Prefer **Microsoft Learn** (`learn.microsoft.com`) and official ALZ repositories
  (`Azure/alz-terraform-accelerator`, `Azure/ALZ-Bicep`, `Azure/Azure-Landing-Zones-Library`).
- Reuse curated links from
  [`skills/_shared/references/caf-link-catalog.md`](skills/_shared/references/caf-link-catalog.md)
  and the [`docs/resources.md`](docs/resources.md) table.
- Do not invent URLs. If unsure, link to the design-area hub page.

## 4. Definition of done

- [ ] Frontmatter parses; `name` matches folder/file.
- [ ] `description` has `WHEN:` triggers and a `DO NOT USE FOR:` clause (skills).
- [ ] Body follows the recommended structure.
- [ ] All relative links resolve.
- [ ] Cross-links added (skill ↔ related skills/agents).
- [ ] Index tables updated: [`docs/README.skills.md`](docs/README.skills.md),
      [`docs/README.agents.md`](docs/README.agents.md), and the tables in
      [`AGENTS.md`](AGENTS.md).
- [ ] English only; no tenant-specific or secret data.

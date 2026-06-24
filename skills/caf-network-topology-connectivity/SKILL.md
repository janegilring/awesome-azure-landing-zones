---
name: caf-network-topology-connectivity
description: "CAF/ALZ guidance for the Network topology and connectivity design area: choosing hub-spoke vs Virtual WAN, planning IP addressing, DNS and private endpoints, egress/ingress inspection, and hybrid connectivity (ExpressRoute/VPN). WHEN: \"network topology\", \"hub and spoke\", \"virtual wan\", \"connectivity subscription\", \"private dns\", \"private endpoint\", \"private link dns\", \"expressroute\", \"vpn gateway\", \"azure firewall\", \"egress inspection\", \"ip address plan\", \"connect on-premises to azure\", \"landing zone networking\", \"vnet peering\". DO NOT USE FOR: deploying the platform with the Accelerator (use the alz-accelerator skill), application-internal networking unrelated to the platform, or Azure RBAC for network resources (use azure-rbac)."
license: MIT
metadata:
  author: Jan Egil Ring
  version: "0.1.0"
  designArea: "Network topology and connectivity"
---

# Network topology and connectivity

## Overview

Networking is central to almost everything inside an Azure landing zone: it connects workloads
to each other, to platform shared services, to external users, and to on-premises infrastructure.
This design area sits in the **environment group** of the eight ALZ design areas because the
decisions here shape the platform foundation that every landing zone depends on. Get the topology,
IP plan, DNS, and inspection model right early — these are expensive to change later.

In the conceptual ALZ architecture, network resources for the platform live in the
**Connectivity** management group (typically one Connectivity subscription), while workloads run
in **Corp** (private, possibly hybrid-connected) and **Online** (internet-facing) landing zones.

## When to use this skill

- Deciding between **hub-and-spoke** (customer-managed) and **Virtual WAN** (Microsoft-managed).
- Planning **IP addressing**, regions, and multi-region growth.
- Designing **DNS**, **Private Link**, and **private endpoint** name resolution.
- Planning **egress/ingress inspection** (Azure Firewall / NVA) and forced tunneling.
- Connecting **on-premises** via ExpressRoute or VPN, and choosing where gateways live.
- Reviewing an existing landing zone's connectivity design against CAF recommendations.

## Key concepts

- **Connectivity subscription** — platform-owned subscription hosting shared network resources
  (gateways, firewall, DNS, DDoS) in the Connectivity management group.
- **Hub-and-spoke** — a hub VNet provides shared services and hybrid connectivity; spoke VNets
  (workloads) peer to it. You manage the hub, routing, and peering yourself.
- **Virtual WAN** — Microsoft-managed hubs provide built-in any-to-any transitive connectivity and
  global transit across regions. Hubs only host Microsoft-managed resources (gateways, Azure
  Firewall via Firewall Manager, route tables, select NVAs).
- **Secured virtual hub** — a Virtual WAN hub with Azure Firewall managed by Firewall Manager.
- **Private endpoint / Private Link** — private IP access to Azure PaaS, removing public exposure;
  requires deliberate **private DNS** design to resolve `privatelink.*` zones.
- **Azure DNS Private Resolver** — enables on-premises and hub services (e.g. Azure Firewall DNS
  proxy) to resolve private DNS zones; the recommended building block for DNS at scale.
- **UDR (user-defined route)** — forces spoke traffic through the hub firewall for inspection.
- **Subscription democratization** — each workload (or workload-environment) gets its own
  subscription and VNet; hub-spoke/Virtual WAN is how those VNets share connectivity and inspection.

## Decision guidance

### Hub-and-spoke vs Virtual WAN

Use **Virtual WAN** when you want Microsoft to manage hub infrastructure and routing, especially for:

- Many regions and/or many branch/VPN/ExpressRoute sites (global transit, any-to-any).
- Lower operational overhead and built-in transitive routing.
- Large-scale Private Link (up to 4,000 private endpoints per hub).

Use **customer-managed hub-and-spoke** when you need:

- Full control over the hub (custom NVAs in the data path, bespoke routing).
- Capabilities or appliance placements not supported inside a Virtual WAN hub.
- A simpler single-region footprint where a self-managed hub is sufficient.

> Both are valid ALZ patterns. The Accelerator's platform landing zone starter supports
> `connectivity_type = virtual_wan | hub_and_spoke_vnet | none`. Pick per requirements, not habit.

### IP addressing

- Plan a **non-overlapping** address space across Azure regions and on-premises before deploying.
- Reserve space for growth (new regions, new spokes, gateway subnets, Bastion, Firewall).
- Avoid reusing ranges that exist on-premises to keep hybrid routing simple.

### DNS and private endpoints

- Centralize **private DNS zones** for `privatelink.*` in the platform and **auto-register** via
  policy (`Deploy-Private-DNS-Zones`) so workload teams don't manage DNS themselves.
- For Virtual WAN, implement the **DNS virtual hub extension** pattern (a DNS spoke with
  **Azure DNS Private Resolver** inbound endpoint) so the hub firewall and on-premises can resolve
  private zones. See [`references/dns-and-private-endpoints.md`](references/dns-and-private-endpoints.md).

### Inspection and egress

- Route inter-spoke and internet-bound traffic through **Azure Firewall** (or an NVA) in the hub
  using UDRs; enable **DNS proxy** on the firewall when private endpoints are in play.
- Decide **forced tunneling** to on-premises vs. local Azure egress per compliance needs.

### Hybrid connectivity

- Use **ExpressRoute** for private, predictable bandwidth; add a VPN as backup or for branch sites.
- Place gateways in the hub (hub-and-spoke) or in the Virtual WAN hub (Microsoft-managed).

## Recommended resources

- Network topology and connectivity design area — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/network-topology-and-connectivity
- Hub-and-spoke topology — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/hub-spoke-network-topology
- Virtual WAN topology — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/virtual-wan-network-topology
- Private Link and DNS in Virtual WAN — https://learn.microsoft.com/azure/architecture/networking/guide/private-link-virtual-wan-dns-guide
- Enable more Azure regions in ALZ — https://aka.ms/alz/multiregion
- More links: [`skills/_shared/references/caf-link-catalog.md`](../_shared/references/caf-link-catalog.md)

## Related skills and agents

- Skill: `caf-resource-organization` — where the Connectivity subscription/management group fit.
- Skill: `caf-security` — firewall, DDoS, and Zero Trust network controls.
- Skill: `caf-governance` — policies that enforce DNS, DDoS, and peering guardrails.
- Skill: `alz-accelerator` — deploying the connectivity resources with Bicep or Terraform.
- Agent: [`networking-connectivity`](../../agents/networking-connectivity.agent.md) — the persona that drives connectivity design using this skill.

## References

- [`references/topology-patterns.md`](references/topology-patterns.md) — deeper comparison of
  hub-spoke vs Virtual WAN, constraints, and when each applies.
- [`references/dns-and-private-endpoints.md`](references/dns-and-private-endpoints.md) — private
  DNS, Private Link, and the DNS virtual hub extension pattern.

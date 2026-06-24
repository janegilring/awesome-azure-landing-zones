# Topology patterns: hub-and-spoke vs Virtual WAN

Deep-dive companion to the `caf-network-topology-connectivity` skill. Use it to justify a topology
choice and to understand each pattern's constraints.

## The two ALZ-aligned patterns

| Aspect | Customer-managed hub-and-spoke | Azure Virtual WAN |
| :--- | :--- | :--- |
| Hub management | You manage the hub VNet, routing, peering | Microsoft-managed hub infrastructure and routing |
| Transitive connectivity | You configure (UDRs, peering, NVA) | Built-in any-to-any, including hub-to-hub |
| Global transit (multi-region) | Manual hub interconnect | Native across regions and hubs |
| NVAs in data path | Flexible placement in the hub | Only supported NVAs inside the hub; others via spoke/extension |
| Private endpoints | Standard VNet limits | Up to 4,000 private endpoints per hub |
| Operational overhead | Higher | Lower |
| Best for | Single region, bespoke routing/appliances | Many regions/branches, scale, low ops |

## Virtual WAN specifics

- **SKU:** choose **Standard** Virtual WAN for higher throughput, ExpressRoute, full-mesh
  any-to-any, and Azure Monitor insights. **Basic** supports only site-to-site VPN scenarios in a
  single hub.
- **Hub contents (Microsoft-managed only):** virtual network gateways (P2S VPN, S2S VPN,
  ExpressRoute), Azure Firewall via Firewall Manager, route tables, and select vendor NVAs.
- **Secured virtual hub:** enable Azure Firewall on the hub and route private + internet traffic
  through it. Enable **DNS proxy** when private endpoints are used.
- **Shared services that can't live in the hub** (custom DNS, custom NVAs, Bastion): deploy them
  using the **virtual hub extension pattern** — a single-responsibility spoke peered to the hub.

## Hub-and-spoke specifics

- Hub and spokes can live in different resource groups and subscriptions within the same or
  different Microsoft Entra tenants; a VNet cannot span subscriptions.
- Under **subscription democratization**, each workload's VNet is its own spoke; the hub provides
  shared hybrid connectivity, transit, and centralized inspection.
- Use **UDRs** on spokes to force inter-spoke and internet traffic through the hub firewall/NVA.

## Choosing — quick checklist

1. How many regions now and in 24 months? Multiple → favor Virtual WAN.
2. How many branch/VPN/ExpressRoute sites? Many → favor Virtual WAN.
3. Do you need custom NVAs directly in the hub data path? Yes → favor hub-and-spoke.
4. What is your private endpoint scale? Very high → Virtual WAN hub limits help.
5. What is your operations capacity? Limited → Virtual WAN reduces overhead.

## Sources

- Virtual WAN topology in ALZ — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/virtual-wan-network-topology
- Hub-spoke with Virtual WAN (recommendations) — https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke-virtual-wan-architecture
- Hub-and-spoke network topology — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/hub-spoke-network-topology
- Subscription democratization (design principle) — https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-principles

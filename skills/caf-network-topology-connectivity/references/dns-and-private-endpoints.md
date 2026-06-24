# DNS and private endpoints

Deep-dive companion to the `caf-network-topology-connectivity` skill. Covers private DNS, Private
Link, and resolving private endpoints across a landing zone — including the Virtual WAN case.

## Why DNS is the hard part of Private Link

A private endpoint gives an Azure PaaS resource a **private IP** in your VNet, but clients still
resolve the resource's **public FQDN** (e.g. `mystorage.blob.core.windows.net`). Correct private
resolution requires a `privatelink.*` private DNS zone with an `A` record mapping the FQDN to the
private IP, linked so every client (in Azure and on-premises) resolves to the private address.

## Centralized, policy-driven DNS (recommended)

- Host the `privatelink.*` **private DNS zones centrally** in the platform (Connectivity
  subscription).
- **Auto-register** records using Azure Policy (`Deploy-Private-DNS-Zones`) so workload teams never
  manage DNS manually. This keeps resolution consistent and governable.
- Link the zones so spokes and on-premises resolve them (directly or via a resolver).

## Hub-and-spoke DNS

- Place **DNS forwarders** or an **Azure DNS Private Resolver** in the hub.
- On-premises forwards `privatelink.*` (and Azure public zones as needed) to the resolver's
  **inbound endpoint**; Azure resources use the hub resolver as their DNS server.

## Virtual WAN DNS — the virtual hub extension pattern

Virtual WAN hubs can't host arbitrary services, so DNS is implemented as a **virtual hub
extension**: a dedicated DNS spoke VNet peered to the regional hub. Typical components:

1. A spoke VNet peered to the region's virtual hub (default routing forces traffic via the hub
   Azure Firewall).
2. An **Azure DNS Private Resolver** with an **inbound endpoint** in that spoke.
3. The `privatelink.*` private DNS zones linked to the spoke VNet.
4. Azure Firewall **DNS proxy** pointed at the resolver inbound endpoint, so the firewall (and
   anything using it for DNS) resolves private zones correctly.

This lets workload teams use private endpoints while DNS stays centralized and the hub firewall can
make FQDN-based decisions. For multi-region, repeat per region and link zones appropriately.

## Checklist

- [ ] `privatelink.*` zones hosted centrally and linked to resolving VNets.
- [ ] Policy auto-registers private endpoint DNS records.
- [ ] Resolver inbound endpoint reachable from on-premises (conditional forwarding configured).
- [ ] Firewall DNS proxy enabled where FQDN inspection is required.
- [ ] No overlapping or split-brain DNS between on-premises and Azure.

## Sources

- Private Link and DNS in Virtual WAN (guide) — https://learn.microsoft.com/azure/architecture/networking/guide/private-link-virtual-wan-dns-guide
- DNS virtual hub extension pattern — https://learn.microsoft.com/azure/architecture/networking/guide/private-link-virtual-wan-dns-virtual-hub-extension-pattern
- Azure Private Link in hub-and-spoke — https://learn.microsoft.com/azure/architecture/networking/guide/private-link-hub-spoke-network
- Azure DNS Private Resolver — https://learn.microsoft.com/azure/dns/private-resolver-endpoints-rulesets
- Private endpoint DNS integration scenarios — https://github.com/dmauser/PrivateLink/tree/master/DNS-Integration-Scenarios#41-azure-dns-private-resolver

# DNS service inventory questionnaire

> **Status:** `PROPOSED` · **2026-11-12**.
> Public questionnaire only. No infrastructure inspection, measurement collection,
> configuration change, or pilot has been performed through this document.

[Proposal overview](blue3.md) · [Architecture](isp-architecture.md) ·
[Pilot plan](isp-pilot.md)

## 1. Starting point and handling

The owner reports approximately 5,000 subscribers, about 85% business subscribers.
See [the planning context](blue3.md#planning-context) for its provenance and limits.
Clarify whether a subscriber record means a customer account, contract, or access
circuit. Do not equate it with one office, one company, one device, or one DNS
traffic profile. Multiple branches or links can belong to the same company.

Complete the operational answers in a private worksheet. This public document
contains field definitions, not the actual inventory. Use aliases such as
`DNS-A`, `POP-A`, and `CPE-FAMILY-A` in shared summaries. Keep addresses, customer
identifiers, private zones, topology details, and configuration extracts private.
Passwords, tokens, private keys, full device backups, and browsing logs are not
needed for this questionnaire.

For each answer, record its owner, observation date, scope, evidence source, and
status: **owner-reported**, **verified from an existing record**, or **unknown**.
An unknown value is useful information; do not turn an estimate into a measurement.
Start with existing diagrams, inventories, monitoring summaries, and operator
knowledge. Any new measurement or configuration access needs a separate, scoped
execution decision; no active probing or query logging is requested here.

## 2. Minimum inventory to choose the architecture

| Area | Information needed now | Decision it supports |
| --- | --- | --- |
| Subscriber and site model | Meaning of subscriber count; approximate business sizes, sites/links per customer, access methods, and relevant POPs | Policy unit, support scope, and representative cohort |
| Current recursive DNS | Own or third-party service; resolver count; software/version; physical/virtual placement; recurses directly or forwards; serving subscriber traffic versus authoritative zones | Reuse, separate pool, managed alternative, and rollback destination |
| Current DNS path | What clients/CPEs are told to use; where DNS is assigned; router proxy/cache or direct queries; what source/context the resolver actually receives | Where filtering can be inserted without losing subscription identity |
| Business DNS | Presence of internal DNS/Active Directory, local zones, conditional forwarding, VPN DNS, cloud security DNS, customer-managed firewalls, and centralized branch resolution | Preserve internal services and identify which public queries can reach Blue3 filtering |
| CGNAT and public IPv4 | Approximate share behind CGNAT versus dynamic/static public addressing; platform/version; DNS path relative to NAT; overlapping access realms and routed customer prefixes | Trusted identity boundary; eligibility for per-subscription policies |
| Session and address lifecycle | PPPoE/IPoE/DHCP/static provisioning; authoritative assignment source; stable circuit/site key; connect/disconnect/renewal events; expiry, reuse, delay, and out-of-order handling | Whether old address holders can ever select policy after reassignment |
| IPv6 | Adoption rate; access and delegation method; prefix size and stability; renewal/reassignment; DNS distribution through RA/DHCPv6; source identity seen by DNS | Dual-stack coverage, prefix ownership, and protection from a parallel ordinary DNS path |
| CPE and firewall fleet | Main manufacturers/models/firmware and approximate distribution; router/bridge mode; Blue3/customer ownership; supported remote management and DNS settings | Which families can be activated and restored reliably |
| Failure domains | Separation of power, hosts, storage, uplinks, routing, and management; existing ordinary DNS reserve capacity | Whether both filtering endpoints can fail together and how to recover |
| Operations | Named DNS/access/CPE owners; duty coverage; customer IT approval/contact; change windows; current restoration mechanism | Feasible rollout and support, without assuming a new SLA |

For identity, an assertion that a resolver is “before CGNAT” is not sufficient.
Document the complete path and the source it sees for each access class, including
intermediate NAT, shared proxies, VPNs, and centralized enterprise DNS. Use a
sanitized path such as client → internal DNS or CPE → access gateway → resolver;
mark where source translation or aggregation occurs. Start from existing evidence.

For business networks, identify who owns the internal DNS and which queries are
forwarded outside it. Preserve domain/internal resolution as a design requirement;
do not assume all endpoints should switch directly to an ISP resolver. Microsoft
documents internal DNS and forwarding for domain-connected servers.
[Source: Microsoft DNS client recommendations](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/best-practices-for-dns-client-settings).

Also record alternate forwarders, root-hint fallback, and VPN behavior: these can
change the path during a failure. The proposed design needs to distinguish an
approved ordinary fallback from an unnoticed loss of filtering.
[Source: Microsoft DNS forwarding and fallback](https://learn.microsoft.com/en-us/windows-server/networking/dns/forwarding).

## 3. Metrics needed to size the pilot

Prefer existing aggregate monitoring for at least seven consecutive days covering
business hours, a busy period, and a weekend. Extend the observation window if
that week misses known business peaks. If metrics do not exist, record the gap
for P1 instead of enabling collection now. Never supply query names to estimate QPS.

| Metric or input | Required detail | Why it matters |
| --- | --- | --- |
| Query volume | Average, p95, maximum and burst QPS per resolver and aggregate; sample interval, timezone, window, and gaps | Initial capacity and busy-period envelope |
| Measurement point | Ingress versus upstream queries, CPE/internal caching, duplicated series, resolver-specific peaks and their timestamps | Avoid double counting and underestimating simultaneous load |
| Cache behavior | Hit/miss rate and available cold-start/restart observations | Recursive work can differ at the same ingress QPS |
| Response latency | p50/p95/p99 from the same observation point; subscriber-path measurements if already available | Baseline for added filtering latency; server time alone is insufficient |
| Errors | Timeouts, SERVFAIL, resource drops, and other response classes separately | Establish normal failure rates without counting legitimate NXDOMAIN as downtime |
| Traffic mix | IPv4/IPv6 and UDP/TCP; DoH/DoT if already offered; connection counts/churn where applicable | Match supported transports and encryption overhead |
| Host and network load | CPU, memory, cache size, link utilization, and saturation at the same times as DNS peaks | Detect the limiting resource and spare capacity |
| Business cohort | Approximate devices/users per site, sites per customer, operating hours, central DNS, and known peak events | A shop and a large office are not interchangeable capacity units |
| Candidate-group load | Existing aggregate QPS for the intended group, or a justified measured access-segment upper bound | Lab load target before live enrollment |

Record counter definitions, not only dashboard screenshots or peak numbers. A
five-minute average is not an instantaneous maximum. Add concurrent resolver
series at matching timestamps; do not present the sum of unrelated maxima as a
measured aggregate peak. Mark a conservative bound explicitly when using one.

The population estimate establishes market scale, not hardware requirements.
Use measured ingress QPS, cache misses, transport mix, and failure-domain tests
to size the candidate pool. The [capacity gate](isp-architecture.md#8-capacity-and-option-selection)
requires the surviving domain to handle the pilot load with headroom. Reassess
before expanding beyond the measured cohort.

## 4. CPE and customer compatibility sheet

Use one private row per relevant CPE/firewall model and firmware, not one public
row per subscriber. Include these fields:

| Field group | Questions to answer |
| --- | --- |
| Provisioning | Is the device Blue3-managed, customer-managed, or in bridge mode? Who controls downstream DHCP, IPv6 advertisements, and internal DNS? |
| DNS behavior | Does it forward/cache DNS, advertise upstream endpoints directly, force a built-in upstream, or receive DNS through PPP/DHCP? Are IPv4 and IPv6 controlled separately? |
| Policy path | Do all business public lookups use this circuit, or are they tunneled to another site/provider? Is there multi-WAN failover? |
| Optional encrypted DNS | Which exact firmware supports the intended DoH/DoT path and identity model, if any? Unknown support is not a pilot promise. |
| Restoration | What settings must be restored at the provisioning source, router, and any internal forwarder? Can this be done remotely if filtering DNS is unavailable? |
| Timing and persistence | What are DHCP/RA lifetimes, cache effects, and known convergence times? Do settings survive renewals, reboot, and central reprovisioning? Mark untested behavior. |
| Ownership and criticality | Who authorizes changes? What working hours, critical services, outage tolerance, and customer/vendor restrictions apply? |

For each business pilot site, identify critical-service categories privately:
identity/login, ERP, fiscal invoicing, point of sale, banking, voice, VPN, cloud
applications, and any Blue3 service it relies on. These are examples to inventory,
not claims that every customer uses them. Record customer IT acceptance and the
restoration owner before selecting the site.

## 5. Decisions produced by the inventory

| Evidence available | Architecture or pilot consequence |
| --- | --- |
| Trusted current subscription/circuit identity reaches DNS | Evaluate per-subscription/site filtering, subject to the reassignment tests |
| Only shared post-NAT or centralized enterprise source is visible | Hold individual site policy claims; design a trusted path or explicitly agreed aggregate scope first |
| Internal business DNS or split namespaces are present | Preserve that authority and evaluate only the intended external-forwarding path; obtain the customer's IT approval |
| Both protocol families and CPE restoration are understood | That access/CPE combination can become a lab candidate |
| IPv6 behavior, independent recovery, or management ownership is unknown | Hold the affected combination; do not silently activate partial coverage |
| Candidate load and N-1 headroom are measured | Set lab capacity and a cohort cap; do not derive either solely from 5,000 subscribers |
| Staffed support and customer contacts are available | Select business-heavy cohorts by access/CPE/DNS profile and criticality |

The provisional 25–50 connection pilot ceiling is not an approved sample or a
statistically representative survey. Count participating circuits/sites, distinct
customer organizations, and supported technical profiles separately. Do not
force an exact 85/15 split: prioritize safe recovery and coverage of the business
DNS paths Blue3 intends to offer. A staff-home pilot alone cannot validate them.

The inventory is sufficient for a concrete architecture decision when we can
identify each intended policy scope, trace both IPv4/IPv6 query paths, preserve
internal DNS, choose viable failure domains, bound pilot load, and name a tested
restoration path to validate in the lab. Unknowns may remain outside the selected
scope, but must remain visible in the acceptance report.

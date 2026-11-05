# DNS filtering evaluation for Blue3

> **Status:** `PROPOSED` evaluation framework · **2026-11-05**.
> No pilot, provider selection, production architecture, or deployment is approved
> by this document.

[Documentation index](README.md) · [NextDNS guide](nextdns.md)

## Purpose and boundaries

Use NextDNS as a reference for managed filtering while determining what Blue3
actually needs. This repository does not yet implement a resolver or filtering
platform. The following criteria are a project proposal, not claims about
capabilities already available in NextDNS or deployed by Blue3.

| Audience | Questions to answer first |
| --- | --- |
| Personal devices | Installation, privacy preferences, and compatibility |
| Corporate users | Internal names, identity, policy groups, auditing, and continuity |
| ISP subscribers | Opt-in, scale, policy isolation, commercial permission, support, and availability |

Managed recursive DNS filtering does not replace authoritative zones or the
design of internal name resolution. An external resolver may not know corporate
names. Apple documents this compatibility issue for encrypted DNS clients.
[Source: Apple encrypted DNS overview](https://developer.apple.com/videos/play/wwdc2020/10047/).

## Components to evaluate

| Component | Evidence needed |
| --- | --- |
| Recursive resolution | Cache behavior, DNSSEC validation, IPv4/IPv6, capacity, and redundancy |
| Filtering | Block categories, reputation sources, narrow exceptions, and false positives |
| Transport | Client support for conventional DNS, DoH, and DoT; network restrictions |
| Internal DNS | Private zones, conditional forwarding or client exceptions, and VPN behavior |
| Policy management | User/group boundaries, administrative access, change history, and isolation |
| Operations | Query rate, latency percentiles, errors, alerting, and recovery procedures |
| Data handling | Log fields, retention, access, storage location, and deletion procedures |

## Managed service or self-operated infrastructure

| Criterion | Managed service | Self-operated infrastructure |
| --- | --- | --- |
| Operations | Provider manages part of the resolver and filtering service | Blue3 owns maintenance, capacity, and incident response |
| Control | Limited by exposed features and contract | Depends on implemented components and policies |
| Availability | Provider service and network reachability both matter | Requires tested redundancy and an operational team |
| Data | Assess provider processing and configured retention | Blue3 operates and protects its own collection/storage |
| Cost | Subscription and commercial conditions | Infrastructure, connectivity, staffing, and maintenance |

A hybrid design is possible, but should name the resolver used by each class of
query and during each failure mode. An unfiltered fallback may preserve name
resolution while silently removing protection; document that choice explicitly.

## Pilot sequence

1. Select one audience and a small, consenting test group. Assign an operator and
   a person who can stop the pilot.
2. Measure the existing path: average/peak queries per second, monthly volume,
   concurrent clients, geography, IPv4/IPv6, and essential internal names.
3. Map recursive and authoritative roles, VPNs, caching, and client DNS overrides.
   Keep operational addresses and account data outside this public repository.
4. Set success thresholds before testing: p50/p95/p99 latency, resolution errors,
   availability, acceptable false positives, and support response expectations.
5. Test policy isolation, allow/deny exceptions, essential apps, internal names,
   captive portals, and each selected device/network combination.
6. Exercise resolver and uplink failures. Decide whether each case should keep
   resolving without filtering or fail closed; measure the result and rollback.
7. Confirm the intended commercial use, support terms, limits, and any SLA with
   the provider. The published Pro plan is personal/family; Business pricing is
   per 50 employees. Neither establishes ISP resale permission.
   [Source: official pricing](https://nextdns.io/pricing).
8. Review the proposed data handling and applicable privacy obligations with the
   responsible owner. Record the purpose, access, retention, and deletion path.
9. Record results and decide whether to proceed, revise the design, or stop.
   No measurements or successful pilot results exist in this repository yet.

## Evidence required before production

- Architecture diagram showing normal, internal-name, and fallback DNS paths.
- Measured capacity, latency, errors, and recovery behavior.
- Approved logging and retention settings, with named access owners.
- Procedures for false positives, user support, incidents, and rollback.
- Confirmed commercial terms and operating costs.
- Explicit acceptance criteria and an accountable operations owner.

## Decisions still open

The first audience, required scale, internal DNS dependencies, retention, and
availability target remain open. Track these decisions in the
[project queue](../.continue/README.md); record accepted technical decisions in
the [ADR log](decisions.md). Selecting a public repository name does not select
a DNS provider or authorize changes to Blue3's network.

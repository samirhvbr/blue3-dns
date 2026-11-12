# DNS filtering for Blue3 subscribers

> **Status:** `PROPOSED` architecture and pilot · **2026-11-12**.
> Audience confirmed: Blue3 Internet subscribers. Technical choices, service
> policies, and pilot execution remain subject to review. No implementation.

[Documentation index](README.md) · [Open decisions](../.continue/README.md)

## Recommendation for review

Offer an optional security filtering profile per Internet subscription, initially
covering queries through the subscriber's Blue3 connection. Evaluate a separate
Blue3-operated filtering resolver pool with two independent failure domains.
Keep ordinary DNS available for nonparticipants and explicit restoration.

This approach makes identity, policy isolation, customer support, and rollback
testable before expanding the service. It requires operating capacity and a
trusted subscription-to-address mapping; it is not automatically preferable if
those conditions cannot be met. A contracted managed ISP service remains an
alternative. NextDNS is a reference, not a selected supplier.

| Read | Decisions and evidence covered |
| --- | --- |
| [Architecture](isp-architecture.md) | Components and query flow; CGNAT/IPv6 identity; caching and DNSSEC; transport; failures; capacity and supplier alternatives |
| [Pilot plan](isp-pilot.md) | Inventory and baseline; lab; internal volunteers; subscriber cohort; targets; rollback; final evidence report |
| [Operations and privacy](isp-operations-privacy.md) | Enrollment/withdrawal; proposed retention; support and exceptions; monitoring and ownership |

## Scope and limits

Start with known malicious/phishing domains. Evaluate advertising/tracking rules
later as a separate profile. Parental controls, off-network protection, billing,
and a new customer portal are outside the first pilot. Filtering is not an
antivirus, a complete firewall, or a guarantee that all advertisements disappear.

A policy initially belongs to the connection/household, not each person or
device. Shared public IPs behind CGNAT cannot select individual subscriber
policies safely. IPv6 prefix changes, stale identity records, and shared caches
are acceptance tests, not details to postpone until after enrollment.

Recommend preserving connectivity when protection cannot be applied safely,
with an explicit degraded state and restoration procedure. This availability
trade-off needs owner approval and must appear in participant information.

## Decisions before execution

| Decision | Proposed starting point | Still needed |
| --- | --- | --- |
| Offering | Optional security profile; no initial pilot billing | Service owner approval of scope and participant terms |
| Architecture | Separate Blue3-operated pool before CGNAT | Current resolver/access inventory, trustworthy identity lifecycle, and operator capacity |
| Stack and feeds | Evaluate a familiar established resolver first | Version-specific tests, feed quality/license, total operating cost |
| Managed alternative | Evaluate only with suitable ISP terms | Written commercial eligibility, data terms, policy isolation, and quote |
| Cohort | Lab → 5–10 internal volunteers → up to 25–50 subscribers | Available subscribers, supported access/CPE combinations, and staffed support |
| Failure policy | Equivalent filtered peers; explicit ordinary-DNS restoration | Approval of reduced protection and measured end-to-end recovery |
| Privacy | Raw query logging off; limited aggregate metrics | Purpose/basis, notice, retention, deletion, vendor review, accountable owner |
| Acceptance | Proposed targets in the pilot plan | Baseline, named approvers, and final thresholds before enrollment |

No current subscriber count, QPS baseline, resolver inventory, provider quote,
or device result is asserted by these documents. Keep actual addresses, account
identifiers, topology details, and customer evidence in private operational records.

## The next three actions

1. **Complete the private network and identity inventory.** Establish the current
   DNS path, access-session source, IPv4/IPv6 lifecycle, CPE restoration method,
   and available failure domains. These facts can validate or rule out the
   recommended topology before implementation effort is committed.
2. **Approve the service boundary and operating responsibilities.** Decide
   opt-in behavior, degraded protection, privacy/retention, support coverage, and
   budget. Do not convert proposed targets into customer promises.
3. **Authorize a bounded lab evaluation after those decisions.** Select one
   candidate stack and execute the isolation, DNS correctness, failure, and
   restoration matrix before any customer pilot. A public release of this
   documentation does not grant that authorization.

Accepted scope decisions live in [the ADR log](decisions.md). Unresolved work is
tracked in [the project queue](../.continue/README.md). Official source checks
are recorded in [the source register](sources.md); no lab or pilot has run.

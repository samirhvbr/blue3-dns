# Subscriber service operations and privacy proposal

> **Status:** `PROPOSED` · **2026-11-12**.
> Responsibilities and data-handling defaults for review, not an adopted service
> policy, legal determination, or authorization to collect subscriber data.

[Proposal overview](blue3.md) · [Architecture](isp-architecture.md) ·
[Pilot gates](isp-pilot.md)

## 1. Enrollment and subscriber experience

Recommend an optional service, initially without pilot billing. Use existing
account authentication or a verified support request to establish subscription
ownership and, for a business, the authorized account/site administrator and IT
change owner. One policy affects the participating connection/site; it does not
identify individual staff, visitors, or household members. Review notice and
shared-network implications with the privacy owner. Record which branch/link
participates instead of assuming approval covers every site of a company.

Before activation, explain the categories, expected coverage, false-positive
risk, data processing, support channel, and availability-first fallback. For a
business, also record critical applications, internal DNS/VPN dependencies, change
windows, restoration owner, and emergency IT contact. Provide
withdrawal with comparable ease to enrollment. Keep optional advertising rules
and future parental controls separate from the initial security profile.

Proposed service states:

| State | Meaning visible to support and, where available, the subscriber |
| --- | --- |
| Requested | Participation recorded; no claim that filtering is active |
| Pending verification | Approved configuration is being checked on the connection |
| Active | A connection-level check confirms the selected policy on the advertised endpoints |
| Degraded | Resolution may continue while filtering or identity verification is unavailable |
| Withdrawn | Ordinary DNS configuration has been restored and verified; cache effects may persist temporarily |

Do not mark activation complete merely because an API or CPE update succeeded.
Verify an owned allowed test name and a synthetic blocked name through the
subscriber path, including IPv6 where enabled. Show the last verification time;
do not imply continuous protection of every app. Initial delivery can use the
existing support workflow; a new portal is an evolution after pilot acceptance.

## 2. Proposed data handling

DNS names can reveal activity without exposing HTTPS page content. Disabling
query logs does not mean the resolver never processes names. A managed upstream
also observes the queries sent to it. Treat pseudonymous subscription references
as potentially identifiable, and keep both mappings and pilot evidence private.

| Data | Proposed pilot handling | Purpose and access |
| --- | --- | --- |
| Raw queried names, client IPs, packet captures | No routine query logging or capture | Not needed for ordinary pilot health measurement |
| Aggregate QPS, latency, response classes, block counts, policy age | 30-day rolling retention; no names, IPs, or subscription IDs in metric labels | NOC measures service health; small-cohort aggregates remain restricted |
| Active address/prefix bindings | Valid session/lease only; invalidate immediately on reassignment or withdrawal | Resolver policy selection; access integration and limited NOC access |
| Binding generations/revocation markers | Retain the minimum state needed to reject delayed events; define the source's maximum replay window before the pilot | Integrity of identity; not a historical browsing or address-reporting service |
| Enrollment, withdrawal, and notice version | Store in the existing protected customer system under an approved purpose and retention schedule | Account/support owners; no duplicate customer database in this project |
| Administrative changes and exception approvals | Actor, time, scope, revision, reason, and expiry; propose 30 days after pilot closure, subject to the approved schedule | Review policy changes and incidents; sensitive exception domains stay private |
| Targeted diagnostic data | Separate case authorization; at most 15 minutes of collection and deletion within 24 hours | Restricted troubleshooting only when synthetic tests and counters are insufficient |

The retention numbers are proposed operational limits, not legal requirements.
Before enabling diagnostics, specify the narrow fields/scope, who can approve,
where data is encrypted, and how deletion reaches replicas and exports. Exclude
raw diagnostic data from general backups and routine ticket attachments. Close
the case with a sanitized conclusion instead of a browsing-history archive.

Reconcile these choices with the privacy owner before the pilot: purpose and
lawful basis for each processing activity, controller/processor roles, customer
notice and rights requests, retention exceptions, vendors/subprocessors, and
any international transfer. Optional service enrollment does not by itself
establish the lawful basis for every data use. ANPD explains that permissible
retention depends on purpose and circumstances rather than one universal period.
[Source: ANPD FAQ, sections 2.6 and 5.5](https://www.gov.br/anpd/pt-br/acesso-a-informacao/perguntas-frequentes).

This proposal does not change the retention of existing access, authentication,
or legally required records. Resolve those duties separately with the responsible
owner. Do not reuse DNS data for advertising, model training, or resale as part
of this pilot. Publish only aggregate, reviewed results in this public repository.

## 3. Support and false positives

Provide one support intake for activation, unexpected blocking, loss of DNS,
and withdrawal. Record connection/cohort reference, approximate time, affected
service, and current policy revision privately. Request a domain only if needed;
avoid full URLs, browsing history, credentials, or whole-device captures.

Triage sequence for a future operator:

1. Establish whether this is a filtering decision, resolution failure, local CPE
   issue, application/VPN override, or an upstream service outage.
2. Reproduce with an approved test from the affected path; compare with the
   ordinary baseline and inspect the policy reason without general query logging.
3. If filtering is responsible, offer immediate withdrawal or a narrow temporary
   exception reviewed by the policy owner. Proposed default exception lifetime:
   24 hours, followed by removal or an explicitly renewed approval.
4. Check the exception on both endpoints and confirm that another subscription's
   policy is unchanged. Submit a feed correction through its normal process if
   warranted; do not silently disable an entire category for the cohort.
5. Record impact, elapsed time, policy revision, and outcome for the pilot report.

Support targets must fit staffed hours. Do not promise a response-time SLA or
sell a 24-hour managed protection service until staffing is confirmed. NOC must
have a reachable duty contact whenever pilot connections remain active, or the
cohort must be restored to ordinary DNS outside the agreed coverage window.

## 4. Monitoring and incident ownership

Measure ordinary resolution and filtering enforcement separately. A resolver
that always returns allowed answers can look available while providing no
filtering. Use paired allowed/blocked synthetic probes at the node and through
the CPE, and compare both protocol families and advertised endpoints.

Alert on unexpected timeouts/SERVFAIL, latency regression, policy mismatch,
stale feeds, invalid bindings, resource saturation, encrypted-transport or
certificate failures where applicable, and failed restoration. Do not label
legitimate NXDOMAIN or intentional blocking as a resolver outage.

| Responsibility | Proposed accountable role |
| --- | --- |
| Service scope, pilot budget, participant terms, phase acceptance | Blue3 service owner |
| Topology, identity fencing, capacity, change window, restoration | Network/access operations lead |
| Immediate pause or rollback for subscriber harm | On-duty NOC operator, without waiting for a committee |
| Feed selection, exceptions, security incident assessment | Designated security/policy owner |
| Notice, processing basis, vendor data terms, deletion evidence | Privacy owner with appropriate legal support |
| Activation checks, customer communication, false-positive intake | Support lead |

Assign people and backup contacts privately before execution. Policy changes
need an audit trail and review proportionate to their scope. Protect management
access with existing administrative authentication, least privilege, and MFA;
separate it from subscriber query traffic. Test recovery of policy/enrollment
state without backing up raw query logs.

For a security or privacy incident, stop the affected rollout, preserve only
necessary evidence under the incident process, restore the approved DNS path,
and involve the responsible owner. Disclosure decisions belong to that process;
do not post customer evidence in public issues. Repository vulnerability reports
follow [SECURITY.md](../SECURITY.md).

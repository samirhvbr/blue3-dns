# Subscriber DNS filtering pilot plan

> **Status:** `PROPOSED` · **2026-11-12**.
> Execution status: **NOT STARTED**. All phases, cohort sizes, time windows, and
> thresholds below require review. This plan authorizes no network changes,
> customer enrollment, data collection, paid service, or deployment.

[Proposal overview](blue3.md) · [Architecture](isp-architecture.md) ·
[Operations and privacy](isp-operations-privacy.md)

## 1. Questions the pilot must answer

Can Blue3 apply the requested security policy to the correct subscription under
CGNAT and IPv6, preserve normal DNS reliability, and restore ordinary service
promptly? Can the operations/support team sustain that behavior without routine
query logging or unacceptable false positives? These questions take precedence
over a high block count or a large enrollment number.

The recommended scope is one security profile and an ordinary comparison path.
Use safe, controlled policy differences to test isolation in the lab. Advertising
categories, parental controls, roaming/off-network service, billing automation,
and a custom subscriber UI are outside the first pilot.

## 2. Phases and gates

Durations are minimum observation proposals, not a committed schedule. A failed
gate extends or stops the phase; elapsed time alone never permits expansion.
The 25–50 connection range is a suggested ceiling for supportable learning, not
a capacity estimate or a requirement to recruit that many customers.

| Phase | Proposed scope and duration | Required evidence to advance |
| --- | --- | --- |
| P0 — Decision and inventory | No DNS changes; fill the private inventory and obtain approval for the next phase | Named owners, candidate path/stack, identity source, independent failure domains, data terms, budget cap, and agreed gates |
| P1 — Baseline | At least 7 days of authorized aggregate measurements from the current path and representative test connections | Busy-hour QPS/latency/error baseline, CPE families, IPv4/IPv6 coverage, essential services, existing DNS behavior and restoration method |
| P2 — Isolated lab | Duration determined by the test matrix; synthetic identities and controlled zones | All mandatory correctness, isolation, load, failure, and rollback tests pass; no real customer traffic or third-party load tests |
| P3 — Internal volunteers | 5–10 eligible staff connections, at least 7 days, explicitly participating | Full activation/withdrawal cycle, monitored peak hours, one controlled recovery exercise, working support coverage, and no unresolved stop condition |
| P4 — Subscriber volunteers | 25–50 eligible connections at most, at least 14 days; enroll in small batches | Gates met across the supported cohort, manageable support demand, no unexplained policy leakage, and validated withdrawal |
| P5 — Review and closure | Freeze expansion and prepare the evidence report | Owner chooses stop, revise/retest, or separately authorize a larger stage; restore participants unless continuation is explicitly approved |

Lab preparation may overlap baseline analysis after authorization; live customer
enrollment must wait for the lab gate. Before each phase, assign a named operator,
change window, observation coverage, participant notice, and stop authority.
No phase is currently authorized by this documentation delivery.

If dual-stack, pre-NAT identity, remote CPE restoration, or a particular firmware
family cannot be validated, exclude that combination explicitly. Do not silently
generalize a successful IPv4-only test to all subscribers. Cohort evidence must
cover each supported access method and CPE family; reduce scope when 50
connections cannot cover it responsibly.

## 3. Preparation and required artifacts

Complete these privately before P2/P3 as applicable:

- Inventory from the [architecture](isp-architecture.md#2-known-facts-and-missing-inputs),
  including address reassignment ordering and the actual current resolver path.
- Selected stack/version and feed licenses; for a managed candidate, written ISP
  commercial eligibility, limits, data handling, exit conditions, and cost cap.
- Two failure-domain diagrams, the policy delivery path, management access, and
  a restoration channel that survives resolver/control-plane failure.
- Approved customer notice, diagnostic policy, retention schedule, participant
  eligibility, support hours, named NOC duty contacts, and communications text.
- Frozen test corpus and expected outcomes: Blue3-controlled harmless allowed,
  blocked, signed, bogus-signed, alias, and negative test cases. Never browse
  live malware to demonstrate filtering.
- Captured pre-pilot CPE settings for each participant, including IPv6 DNS
  advertisements, dynamic provisioning source, and configuration lifetimes.
- A dated acceptance sheet with approvers, targets, exceptions, and evidence
  locations. Public links must never expose the private inventory or customer data.

## 4. Proposed acceptance measurements

Freeze the targets after P1 and before P2/P3 acceptance, with written reasons for
any change. They are pilot decision gates, not a commercial SLA. Keep results by
phase, access method, CPE family, IPv4/IPv6, and enabled transport; aggregate totals
must not conceal a broken supported combination.

| Measure | Proposed gate | Method |
| --- | --- | --- |
| Allowed-name availability | At least 99.9% successful synthetic resolutions during each live phase | Probe through representative CPEs at least once per minute; expected response within 2 seconds; report attempts, failures, probe gaps, and longest outage |
| Latency regression | Filtered p95 no more than baseline +10 ms; p99 no more than baseline +25 ms | Paired allowed-name probes from the same connection and comparable periods; report warm/cold caches separately |
| Policy correctness | 100% expected results in the controlled block/allow corpus | Probe both endpoints and all supported transports; blocking rate in real traffic is not detection coverage |
| Subscription and cache isolation | Zero confirmed cross-subscription policy applications or data disclosures | Reassignment, reconnect, prefix rotation, and positive/negative cache tests; a single failure blocks advancement |
| Essential services and false positives | Zero unresolved filter-caused essential-service failures; at most 1 confirmed false-positive incident per 100 subscriber-days in P4 | Pre-agreed banking, communications, work/study, and Blue3 service checks; de-duplicate the same household/service/cause incident and report affected connections as well |
| Policy change or withdrawal | Resolver policy converges within 5 minutes of an accepted change | Check both endpoints and affected CPE; record client cache/configuration delays separately and hold unsupported CPE families |
| Single-endpoint failure | Representative clients resume the same policy within 30 seconds | Controlled loss of each endpoint/uplink, one at a time; measure client recovery, not just server health |
| Full restoration | Ordinary DNS verified within 15 minutes from the rollback trigger | Exercise the complete procedure below, including detection, CPE update, IPv6, and client confirmation |
| N-1 capacity | Surviving failure domain meets latency/error gates at 2× observed aggregate pilot peak for 1 hour | Isolated controlled load; exercise warm/cold caches, miss bursts, and enabled encrypted transports |
| Nonparticipant protection | Zero attributable disruption or new filtering on comparison connections | Ordinary-path probes before, during, and after every scoped change |
| Privacy and exposure | No routine raw query logging, no unauthorized recursion, successful diagnostic deletion test | Inspect each component's actual configuration/log outputs; test unauthorized sources only against authorized lab endpoints |

Availability success includes an expected legitimate NXDOMAIN in a dedicated
test case, but never an unexpected NXDOMAIN for an allowed name. Intentional
blocking is measured separately. Count failures during planned pilot disruption
exercises in the live-phase report, with a separate annotation; do not hide them
as maintenance exclusions. A probe gap is unknown coverage, not success.

A subscriber-day is 24 hours of enrolled observation, prorated for activation
and withdrawal. Report raw false-positive counts and exposure alongside the rate;
a small, short pilot does not establish a population-wide false-positive rate or
long-term availability. Do not infer malware detection accuracy from a synthetic
corpus or from a large number of blocked queries.

For the first lab load test, use the documented P1 upper bound described in
[capacity planning](isp-architecture.md#8-capacity-and-option-selection). Replace
it with measured pilot traffic and retest before expanding the cohort. Run
destructive both-endpoint failure tests in the lab; any live recovery exercise
needs its own scoped change window and still counts in the live results.

## 5. Mandatory lab and compatibility matrix

| Test | Expected outcome | Blocks live enrollment if it fails? |
| --- | --- | --- |
| Two subscriptions behind the same public CGNAT IP | Distinct intended policies from trusted pre-NAT context | Yes |
| Overlapping private addresses in different realms | No collision or policy crossover | Yes for that access model |
| IPv4 reassignment, early lease end, delayed disconnect, reordered events | No query inherits the previous subscriber's policy; uncertain context is degraded ordinary service | Yes |
| IPv6 prefix rotation and temporary client addresses | Correct current subscription; retired prefix cannot select old policy | Yes for dual-stack |
| Shared positive/negative cache and subscriber exception | No policy bypass or foreign blocking on cache hits; updates invalidate or partition correctly | Yes |
| Signed allowed/blocked zones, bogus signatures, DO/CD/AD flags | Allowed validation works; no authenticated claim on synthetic blocks; documented client-validator behavior | Yes |
| A/AAAA, CNAME chains, HTTPS/SVCB, EDNS, large answers, UDP/TCP | No supported query-type or alias bypass; TCP fallback succeeds | Yes |
| CPE DHCP and IPv6 RA/DHCPv6, reboot, renewal, stale advertisements | Both advertised endpoints apply the intended policy; restoration remains effective | Yes for each enrolled family |
| VPN, app DNS override, external DoH, cached answers | Coverage limits demonstrated and explained; no promise to intercept them | Yes if the notice misstates coverage |
| Internal names, captive portals, essential apps | Required names/services retain approved behavior; no internal-name leak upstream | Yes where applicable |
| Single node/site/uplink loss and both-node loss | Measured peer recovery or approved full restoration | Yes |
| Control-plane outage and address reuse during the outage | Working policy persists; expired/ambiguous identities never borrow another policy | Yes |
| Invalid/empty feed, incomplete load, node restart, divergent revisions | No silent empty policy; last working policy or declared degraded recovery | Yes |
| Spoofed source/context, unauthorized source, bounded query flood | No unauthorized recursion or forged policy identity; legitimate test clients remain usable | Yes |
| Diagnostic capture lifecycle and administrative restore | Approved data expires as specified; policy state recovers without query-log backups | Yes |
| DoH/DoT identity, certificate expiry, reconnect, bootstrap | Equivalent policy and exposure controls; no insecure silent downgrade | Yes only if that transport is offered |

Record exact versions and the specific behavior observed. A test that is out of
scope needs an explicit reason and a corresponding service limitation. A blank
result is not a pass. Test abuse and failure only in the authorized lab or a
specifically approved pilot window; this document contains no execution commands.

## 6. Rollback proposal

Recommend availability-first rollback to the captured ordinary DNS settings.
Disclose that filtering is absent during this mode. Do not wait for the service
owner to respond before the assigned NOC operator stops an unsafe pilot.

Immediate stop conditions are any confirmed cross-subscription policy/data leak,
unauthorized recursion, unexpected impact on nonparticipants, or loss of an
essential service attributable to filtering. Also stop expansion on a failed
acceptance gate. Proposed automatic alert thresholds for NOC action are two
consecutive minutes of failed allowed-name probes from two independent vantage
points, or latency above the agreed regression gate for ten minutes. Confirm the
fault scope promptly; the 15-minute restoration clock starts at the trigger,
not at the end of an investigation.

Future operator procedure:

1. Freeze enrollment and policy promotion; record start time, affected cohort,
   active revisions, and reason in the private incident record.
2. For a policy-only fault with healthy nodes, restore the previously verified
   policy on both endpoints. If that cannot promptly restore the approved
   behavior, proceed to full withdrawal of the affected cohort.
3. Restore the saved ordinary DNS provisioning at its source and on the affected
   CPEs through the independent management path. Cover IPv4, IPv6, DHCP/RA
   lifetimes, and optional encrypted profiles. Do not rely on adding an ordinary
   server to a mixed resolver list or on a failed control plane.
4. Verify ordinary allowed-name resolution from representative affected clients
   and confirm that the failed policy is no longer selected. Check essential
   services and nonparticipants, and account for client caches and reboot/renewal
   behavior without promising instantaneous convergence.
5. Tell participants that filtering is disabled and how support will follow up.
   Record actual recovery time, remaining exceptions, and any manual intervention.
6. Keep enrollment paused. Resume only after root-cause review, a repeated failed
   test now passing, and the accountable owner's approval of the next phase.

If a CPE cannot be restored within the proposed target, exclude that family or
approve a revised, disclosed target **before** enrollment. A lab resolver's fast
restart is not proof of customer recovery. Retain the ordinary path and its
capacity throughout the pilot; do not retire it when initial tests pass.

## 7. Evidence and final decision

For each test, store: test ID, date, operator, candidate/version, policy revision,
synthetic cohort/CPE class, protocol, expected/observed outcome, measured duration,
artifact location, limitations, and reviewer. Keep private references access
controlled. A public report should contain sanitized findings and aggregate
measurements only.

| Current evidence | Status |
| --- | --- |
| Architecture and pilot documents | Proposed; available for owner review |
| Blue3 topology, traffic baseline, and cost sheet | Not collected by this project |
| Stack selection, configuration, and lab tests | Not started |
| CPE compatibility, rollback timings, and load measurements | Not tested |
| Internal or subscriber enrollment | Not started |
| Privacy/commercial acceptance and production decision | Pending responsible owners |

At P5, the decision report must state cohort coverage, sample size, observation
gaps, each gate's result, all incidents, support hours per participating
subscription, incremental cost, unresolved risks, and operator acceptance. The
outcomes are **stop**, **revise and repeat**, or **authorize a bounded next stage**.
Passing the pilot does not automatically authorize fleet-wide rollout, billing,
parental controls, or a production SLA.

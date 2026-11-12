# Open work and decisions

> **Status:** `ACTIVE` · Reviewed **2026-11-12**, version **0.1.2**.

This is the queue for work that has not been completed. Delivered documentation
is indexed in [docs/](../docs/README.md); delivery history is in the
[changelog](../CHANGELOG.md). The
[repodocs convention](https://github.com/samirhvbr/repodocs/blob/master/docs/conventions.md)
governs the distinction.

| Decision | Owner | Context |
| --- | --- | --- |
| Complete the private access/resolver/CPE and business-site inventory and baseline plan | Blue3 network operations and customer IT owners | [Questionnaire](../docs/isp-inventory.md); population estimate supplied, technical inventory and metrics still missing |
| Approve offering scope, enrollment, failure behavior, support ownership, and budget | Samir / Blue3 service owner | [Proposal overview](../docs/blue3.md); recommendations are not adopted service policies |
| Choose a candidate stack/feed or managed ISP contract after inventory | Network and service owners | [Alternatives](../docs/isp-architecture.md#8-capacity-and-option-selection); NextDNS remains a reference |
| Approve data purposes, notice, retention, vendor handling, and deletion | Privacy owner | [Data-handling proposal](../docs/isp-operations-privacy.md#2-proposed-data-handling) |
| Authorize a lab phase and name operators, gates, cohort, and rollback authority | Service owner and NOC | [Pilot plan](../docs/isp-pilot.md); no execution is authorized yet |

The subscriber audience is confirmed by [ADR-004](../docs/decisions.md#adr-004--focus-the-proposal-on-blue3-subscribers).
The owner-reported population and business share are recorded in the
[planning context](../docs/blue3.md#planning-context); they do not establish QPS,
device counts, actual topology, or an approved pilot cohort.
The architecture and pilot documents are delivered proposals; their execution
remains open. No infrastructure, provider account, CPE setting, query collection,
or customer enrollment has been created by this project. Personal iPhone
validation remains unperformed and outside the active subscriber pilot scope.

Do not turn these proposals into implementation without a new owner decision.

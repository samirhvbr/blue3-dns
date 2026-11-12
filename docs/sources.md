# Official source register

> **Status:** `ACTIVE` · Verification dates are recorded by group below.

[Documentation index](README.md)

The supplied README and archive were the editorial starting point. Product
claims were checked against the primary sources below. These checks verify what
the sources state; they are not independent service, device, or network tests.

## Subscriber architecture and pilot references

Checked on **2026-11-12**. The engineering recommendations and numeric pilot gates
are this project's proposals, not recommendations attributed to these sources.
Published component features do not prove compatibility with Blue3's topology.

| Source | What it supports |
| --- | --- |
| [RFC 5358](https://www.rfc-editor.org/rfc/rfc5358.html) | Restricting recursive DNS exposure and reducing reflection abuse |
| [RFC 7871](https://www.rfc-editor.org/rfc/rfc7871.html) | ECS behavior and privacy/operational limitations; it is not subscriber authentication |
| [RFC 8106](https://www.rfc-editor.org/rfc/rfc8106.html) | DNS information and lifetimes in IPv6 router advertisements |
| [RFC 8914](https://www.rfc-editor.org/rfc/rfc8914.html) | Extended DNS Errors, including client-requested filtering; EDE does not replace response codes |
| [BIND response policy reference](https://bind9.readthedocs.io/en/stable/reference.html#response-policy-zone-rpz-rewriting) | RPZ behavior, startup readiness, and DNSSEC rewriting limitations; a candidate mechanism, not a stack selection |
| [Unbound RPZ documentation](https://unbound.docs.nlnetlabs.nl/en/latest/topics/filtering/rpz.html) | Existing policy-zone support as another candidate mechanism |
| [NextDNS pricing](https://nextdns.io/pricing) | Rechecked intended plan use, Business billing unit, and ordinary resolution after the free quota; no ISP contract confirmed |
| [ANPD FAQ](https://www.gov.br/anpd/pt-br/acesso-a-informacao/perguntas-frequentes) | Sections 2.6 and 5.5 on processing bases and purpose-dependent retention; not a legal approval of this pilot |
| [Microsoft DNS client recommendations](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/best-practices-for-dns-client-settings) | Internal DNS requirements for domain-connected servers and external forwarding; supports the business inventory questions |
| [Microsoft DNS forwarding and fallback](https://learn.microsoft.com/en-us/windows-server/networking/dns/forwarding) | Conditional forwarding and root-hint fallback paths to inventory before promising filtering coverage |

## Owner-provided planning inputs

On **2026-11-12**, the owner reported approximately 5,000 subscribers, about 85%
business subscribers. These estimates support audience/cohort planning; they
were not independently measured and do not establish QPS, company size, device
count, or the number of distinct companies/sites. The
[proposal context](blue3.md#planning-context) distinguishes these inputs from
the technical inventory that remains unknown.

## Original NextDNS and iPhone references

Checked on **2026-11-05**. Only pricing was rechecked on 2026-11-12; do not infer
that all original product or device instructions were reverified with this edit.

| Source | What it supports |
| --- | --- |
| [NextDNS overview](https://nextdns.io/) | Filtering categories, custom lists, logs, analytics, DoH, and DoT |
| [NextDNS pricing](https://nextdns.io/pricing) | Query limits, intended use, employee/student billing units, and non-blocking resolution after the free quota |
| [NextDNS quota article](https://help.nextdns.io/t/p8hmvaw/what-happens-after-300k-queries) | Filtering/logging behavior after the free limit |
| [NextDNS privacy policy](https://nextdns.io/privacy) | Provider statements on collection, retention controls, jurisdiction, export, and deletion |
| [NextDNS Apple profile generator](https://apple.nextdns.io/) | Native profile setup and optional exclusions, signing, and root CA |
| [NextDNS team's app/native explanation](https://help.nextdns.io/t/83h1vla/nextdns-app-or-native-support) | The documented iOS app's use of native DoH and its controls |
| [Apple encrypted DNS overview](https://developer.apple.com/videos/play/wwdc2020/10047/) | System and app-specific DNS, VPN tunnel behavior, and internal-name exceptions |
| [Apple profile management](https://support.apple.com/guide/iphone/install-or-remove-configuration-profiles-iph6c493b19/ios) | Profile inspection, settings location, and removal |

## How to maintain this register

Recheck pricing, plan eligibility, and installation steps before relying on them
for a purchase or deployment. Update the affected guide and verification date
when a source changes. Link to the supporting page next to the claim.

The native-DNS explanations include older technical publications. Their continued
availability is not proof that every current app/iOS/VPN combination behaves the
same way; record the actual versions when testing a device.

Regional prices are deliberately linked rather than copied. The retrieved
pricing page displayed GBP, so it was not treated as a Brazilian price quote.
No provider account, iPhone, CPE, Blue3 network, or production resolver was
changed or tested while preparing this documentation. Apart from the owner's
population estimate, technical inventory, traffic measurements, capacity,
latency, recovery times, and vendor quotations remain uncollected. The
[pilot evidence table](isp-pilot.md#7-evidence-and-final-decision)
records that boundary.

# NextDNS guide

> **Status:** `ACTIVE` reference · Official sources checked on **2026-11-05**.
> Product behavior described here has not been tested on Blue3's network.

[Documentation index](README.md) · [iPhone setup](iphone.md) · [Blue3 evaluation](blue3.md)

## How DNS filtering works

When an app needs to connect to `ads.example.com`, it asks a DNS resolver for
the records needed to reach that name. If the query reaches NextDNS, the
service evaluates the associated configuration's policies before responding.

1. The device sends a DNS query.
2. NextDNS evaluates its security rules, blocklists, and exceptions.
3. A blocked name receives a blocking response according to the configuration.
   Blocking does not necessarily mean leaving the query unanswered.
4. An allowed query receives a normal DNS response; the device can then attempt
   its connection. DNS resolution alone does not guarantee the service works.

The configuration applies automatically; opening the app for each connection
is unnecessary. Latency depends on the network, resolver location, and caches.
Cached answers can delay the effect of a policy change.

## Capabilities

| Capability | What it does |
| --- | --- |
| Ad blocking | Blocks domains associated with advertising |
| Tracking protection | Filters queries to known tracking services |
| Security filtering | Blocks domains associated with malware, phishing, and other threats |
| Parental controls | Restricts categories and selected services |
| Custom lists | Allows or denies individual domains |
| Logs and analytics | Shows queries and filtering results when logging is enabled |
| Encrypted DNS | Supports DNS-over-HTTPS (DoH) and DNS-over-TLS (DoT) |

Availability and behavior depend on the selected configuration and client.
[Source: NextDNS product overview](https://nextdns.io/).

## Limits to understand

- DNS filtering is not a replacement for antivirus, a full firewall, updates,
  or application access controls.
- It cannot distinguish an ad from desired content when both use the same
  hostname. Do not expect it to remove every YouTube ad or cosmetic page element.
- An app's own resolver or a VPN's DNS configuration can bypass the selected
  system resolver. Already cached answers or direct IP connections also limit
  what a DNS policy can affect.
- DNS does not reveal HTTPS page bodies, messages, or passwords by itself.
  Allowing a domain does not establish that everything hosted there is safe.
- Blocklists can break legitimate sites. Test essential workflows and keep
  exceptions narrow rather than disabling all protection.
- DNS logs contain background app traffic too; they are not an exact browsing
  history or evidence that a person deliberately visited every domain.

Apple documents both system DNS settings and app-specific encrypted DNS, as
well as VPN precedence for queries inside a tunnel.
[Source: Apple encrypted DNS overview](https://developer.apple.com/videos/play/wwdc2020/10047/).

## Plans and quota

The following is a **2026-11-05 snapshot**, not a price quote:

| Plan | DNS queries | Devices | Published intended use / billing unit |
| --- | --- | --- | --- |
| Free | 300,000 per month | Unlimited | Free tier |
| Pro | Unlimited | Unlimited | Personal and close-family use |
| Business | Unlimited | Unlimited | Small and medium businesses; billed per 50 employees |
| Education | Unlimited | Unlimited | Schools and universities; billed per 250 students |

The pricing page says that exceeding the free quota leaves ordinary DNS
resolution available **without blocking**. The help article also describes
logging as disabled after the limit. A device still resolving names is therefore
not proof that filtering remains active.
[Pricing](https://nextdns.io/pricing) ·
[Quota behavior](https://help.nextdns.io/t/p8hmvaw/what-happens-after-300k-queries).

Check current prices and the currency shown by the official pricing page before
subscribing. Unlimited queries and devices do not establish contractual
permission to resell a service to ISP subscribers. Confirm that use case with
the provider before selecting a plan.

## Privacy

The resolver processes queried domain names. Those names can reveal services
and habits even though they are not the content of an HTTPS conversation.
Encrypted transport protects queries between the device and resolver; it does
not make the user anonymous or remove trust in the resolver operator.

NextDNS states that users control whether queries are logged, retention, and
storage jurisdiction. Its policy also describes access, export, and deletion
controls. These are provider statements, not an independent privacy audit.
[Source: NextDNS privacy policy](https://nextdns.io/privacy).

For a trial, choose logging deliberately and keep only the data needed to
diagnose filtering. Never publish account identifiers, device-specific profiles,
screenshots containing personal data, or raw DNS logs in this repository.

## Choosing a use case

For a personal iPhone, a native profile is a practical option when you prefer
system settings without an extra app. The app can make everyday toggling and
network exclusions easier. See the [iPhone guide](iphone.md).

Corporate and ISP use need separate decisions about internal DNS, policy
isolation, capacity, continuity, privacy, and commercial terms. The
[Blue3 evaluation](blue3.md) records the questions to answer before a pilot.

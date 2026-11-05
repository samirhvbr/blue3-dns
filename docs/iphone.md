# NextDNS on iPhone

> **Status:** `ACTIVE` reference · Official sources checked on **2026-11-05**.
> Setup, battery use, and compatibility have not been measured on a device here.

[Documentation index](README.md) · [NextDNS guide](nextdns.md)

## App or native profile

NextDNS describes its iOS app as using Apple's native DoH support. Policies are
managed through [the NextDNS dashboard](https://my.nextdns.io/) in either case.
[Source: NextDNS team's app/native explanation](https://help.nextdns.io/t/83h1vla/nextdns-app-or-native-support).

| Choice | NextDNS app | Native iOS profile |
| --- | --- | --- |
| Encrypted DNS | Yes | Yes |
| Wi-Fi and cellular | Subject to rules and competing configurations | Subject to rules and competing configurations |
| Enable or disable | App controls and system selection | iOS settings |
| Extra app required | Yes | No |
| Filtering policy | NextDNS dashboard | NextDNS dashboard |
| Network exclusions | App UI | Profile options |

The filtering outcome depends on the policy and the queries reaching it, not
on whether the settings arrived through an app or a profile. Prefer the profile
if you want fewer installed apps; prefer the app if convenient controls matter
more. No battery or performance advantage has been established by this project.

## Set up a native profile

1. Create or select a configuration at [my.nextdns.io](https://my.nextdns.io/).
   For an evaluation, use a separate configuration so test changes do not affect
   other devices.
2. Open the official [Apple profile generator](https://apple.nextdns.io/), enter
   that configuration's identifier, and review the generated profile's options.
3. Download the profile. In iOS **Settings**, select **Profile Downloaded**, then
   **Install**, and follow the device's prompts. Existing profiles are managed
   under **General → VPN & Device Management**; labels can vary with iOS version.
4. Confirm that the intended DNS configuration is selected, then run the checks
   below on Wi-Fi and cellular separately.

[Sources: NextDNS profile generator](https://apple.nextdns.io/) and
[Apple's profile management guide](https://support.apple.com/guide/iphone/install-or-remove-configuration-profiles-iph6c493b19/ios).

The generator offers optional network/domain exclusions and a root CA for its
HTTPS block page. **Installing that CA is not required for encrypted DNS**;
it changes certificate trust. Leave that option off for a basic DNS setup.
An excluded domain or Wi-Fi network intentionally uses another DNS path, so
include exclusions in your validation record.
[Source: generator options](https://apple.nextdns.io/).

## Validate the actual device

Use one setup method at a time while troubleshooting. Record the iOS version,
chosen method, configuration, network, and any VPN or Private Relay setting.

| Check | What to verify |
| --- | --- |
| Resolver | Open [test.nextdns.io](https://test.nextdns.io/) and inspect the reported status/protocol and the setup status in your dashboard |
| Policy | Temporarily deny `example.com` in the dedicated test configuration and confirm blocking after cached answers expire |
| Logs | If enabled, confirm the test query reaches the intended configuration; do not publish the raw output |
| Networks | Repeat on Wi-Fi and cellular rather than assuming one result covers both |
| Compatibility | Repeat with your usual VPN and Private Relay settings; test essential apps and any required internal names |
| Cleanup | Remove the temporary deny rule, restore any changed logging settings, and confirm normal access |

The test endpoint is a diagnostic aid, not a guarantee that every app uses the
same resolver. A successful browser test does not establish system-wide coverage.

## Conflicts and rollback

A VPN can supply its own DNS for traffic inside its tunnel. Individual apps can
select their own encrypted resolver, and enterprise networks may need exceptions
for private names. Validate these paths explicitly rather than stacking profiles.
[Source: Apple encrypted DNS overview](https://developer.apple.com/videos/play/wwdc2020/10047/).

To revert, disable the app configuration or remove the dedicated DNS profile in
iOS settings, then test connectivity again. Inspect a profile's contents before
removing it: a corporate profile may manage more than DNS. Managed devices may
require administrator action.
[Source: Apple profile management](https://support.apple.com/guide/iphone/install-or-remove-configuration-profiles-iph6c493b19/ios).

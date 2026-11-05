# Decisions

> **Status:** `ACTIVE` · Repository decisions recorded on **2026-11-05**.

## ADR-001 — Publish a documentation project before selecting DNS infrastructure

**Status:** `ACCEPTED` · 2026-11-05

**Context.** The owner requested a public `blue3-dns` repository and supplied
NextDNS reference material. The material covers both personal devices and Blue3
use cases, but supplies no measured infrastructure requirements or deployment.

**Decision.** Publish the guides at `samirhvbr/blue3-dns`, in English (US), with
source dates and explicit evaluation status. Use `master`, initial version
`0.1.0`, and the fleet's MIT license and version/commit conventions. No provider,
pilot, runtime stack, or network configuration is selected by this publication.

**Consequences.** Readers can use and improve a stable reference without
mistaking a proposal for an operating service. Device validation, corporate/ISP
scope, and commercial eligibility remain open decisions.

## ADR-002 — Keep generated profiles and DNS captures outside the public history

**Status:** `ACCEPTED` · 2026-11-05

**Context.** A generated Apple profile may identify a configuration or device;
DNS captures and query logs can identify users and internal services.

**Decision.** Retain the supplied `*.mobileconfig` exclusion and exclude raw
`*.pcap`/`*.pcapng` captures as sensitive artifacts. Publish only sanitized,
necessary evidence and use fictional examples. These are explicit publication
exceptions in addition to the fleet's secret exclusions.

**Consequences.** Profiles and captures stay in an appropriate private location.
Gitignore is a convenience, not a disclosure control: review each staged file.

## ADR-003 — Use hosted release automation for the documentation repository

**Status:** `ACCEPTED` · 2026-11-05

**Context.** This project has no application suite or deployment. It needs the
fleet's version tags and Releases without access to operational networks.

**Decision.** Run the release workflow on `ubuntu-latest`, with a pinned checkout
action and job-scoped content-write permission. Keep shell inputs in environment
variables. Follow the upstream maintenance conventions through links; publish no
self-hosted runner inventory or network access instructions in the agent guide.

**Consequences.** No office runner is registered. Publication is distinct from
runtime validation. Wait for the workflow before any manual release recovery to
avoid concurrent publishers.

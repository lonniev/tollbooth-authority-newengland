# Tollbooth Authority New England

<p align="center">
  <img src="https://raw.githubusercontent.com/lonniev/tollbooth-dpyc/main/docs/tollbooth-hero.png" alt="Milo drives the Lightning Turnpike — Don't Pester Your Customer" width="800">
</p>

**The New England exit off the North American on-ramp.**

> Identity: `npub157hmysd6sw7m8kkldycnnc74mssxp9nweddk2p5sw9l2nktr950qyxrenn`
> Parent: [`tollbooth-authority-northamerica`](https://github.com/lonniev/tollbooth-authority-northamerica) (`npub1zummm…xd2t`)
> Service: `https://tollbooth-authority-newengland.fastmcp.app/mcp`
> Neon region: `aws-us-east-2` (Ohio)

A sub-regional Authority certified by NorthAmerica. The first 3-deep Authority chain in the DPYC ecosystem (Prime → NorthAmerica → NewEngland); operators serving New England patrons may choose NE to keep certification latency and audit trails as local as the protocol allows.

> *The metaphors in this project are drawn with admiration from* The Phantom Tollbooth *by Norton Juster, illustrated by Jules Feiffer (1961). We just built the payment infrastructure.*

---

## Architecture

This repository is a thin consumer of the [`tollbooth-dpyc`](https://github.com/lonniev/tollbooth-dpyc) wheel's `tollbooth.authority` extension (wheel ≥ 0.22.1). The entire deployable surface lives in `src/tollbooth_authority/server.py` (~80 lines): a FastMCP instance, an OperatorRuntime, and two `register_*_tools(mcp, runtime)` calls. Onboarding state machine, Schnorr signer, replay tracker, Neon tenant provisioning, and the 10 Authority @tool definitions are wheel-resident and shared with every other Authority MCP in the ecosystem.

NE was scaffolded from `tollbooth-authority` at the start (May 16, 2026) but is no longer a fork — wheel v0.22.0 collapsed every shared module into `tollbooth.authority.*`. The two repos now coexist as independent thin consumers of the same mixin, distinct only in identity, region, and Neon tenant.

For protocol semantics, certificate format, fee cascade, and tool catalog, see the canonical [`tollbooth-authority`](https://github.com/lonniev/tollbooth-authority#mcp-tools) README.

## Onboarding chain note

NE's onboarding (`confirm_authority_claim` → `check_authority_approval`) escalates to **NorthAmerica**, not Prime — the wheel's `resolve_my_parent_npub` reads NE's own `upstream_authority_npub` from the dpyc-community registry and routes the approval DM there. The 3-deep chain is transparent to the protocol; the registry is the source of truth.

## Deploy

Two secrets required by FastMCP Cloud (Horizon):

| Env var | Value |
|---|---|
| `TOLLBOOTH_NOSTR_OPERATOR_NSEC` | NE's nsec |
| `NEON_DATABASE_URL` | A pooled Neon connection string in `aws-us-east-2` |

Everything else — BTCPay credentials for the cashier — arrives via Secure Courier post-deploy (see [`docs/how-to-add-authority.md`](https://github.com/lonniev/dpyc-community/blob/main/docs/how-to-add-authority.md) in the community repo).

## DPYC Ecosystem

NE is one Authority in the wider DPYC federation — a Social Contract of Operators and Authorities bound by transparent, auditable economic rules, with a Certification Chain cascading from the First Curator down through regional Authorities like this one.

- [`tollbooth-dpyc`](https://github.com/lonniev/tollbooth-dpyc) — shared Python SDK (all crypto, vault, auth, pricing, audit)
- [`dpyc-community`](https://github.com/lonniev/dpyc-community) — governance registry and onboarding docs
- [`dpyc-oracle`](https://github.com/lonniev/dpyc-oracle) — free community concierge (registry reads)
- [`tollbooth-authority`](https://github.com/lonniev/tollbooth-authority) — canonical Authority (certification backbone)
- [`tollbooth-sample`](https://github.com/lonniev/tollbooth-sample) — reference Operator implementation
- [`tollbooth-pricing-studio`](https://github.com/lonniev/tollbooth-pricing-studio) — native pricing editor (Swift/iOS)
- [`cypher-mcp`](https://github.com/lonniev/cypher-mcp) — monetized graph answers (named Cypher over Neo4j/AuraDB)
- [`schwab-mcp`](https://github.com/lonniev/schwab-mcp) — Schwab brokerage data
- [`thebrain-mcp`](https://github.com/lonniev/thebrain-mcp) — TheBrain knowledge graph
- [`excalibur-mcp`](https://github.com/lonniev/excalibur-mcp) — X/Twitter posting
- [`taxsort-mcp`](https://github.com/lonniev/taxsort-mcp) — tax sorting and classification
- [`optionality-mcp`](https://github.com/lonniev/optionality-mcp) — options analytics
- [`tollbooth-oauth2-collector`](https://github.com/lonniev/tollbooth-oauth2-collector) — OAuth2 callback collector
- [`tollbooth-shortlinks`](https://github.com/lonniev/tollbooth-shortlinks) — URL shortener

## License

Apache-2.0

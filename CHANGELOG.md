# Changelog

All notable changes to this project will be documented in this file.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

## [0.5.0] — 2026-06-29
- chore: track tollbooth-dpyc **0.57.0** — the SDK unifies the Secure Courier possession token under one name `dpop_token` (retiring `proof`/`proof_token`/`poison`). This regional Authority's proof handling lives entirely in the wheel's `authority/tools.py` (renamed there), so this is a pin bump + uv.lock regen with no wire-API change here. Picks up the wheel's free `patron_auth` probe and proof-vs-credential flow cross-references.

## [0.4.2] — 2026-06-22

### Changed — consume SDK 0.52.0 (vault_source/purchase_mode decoupling)

- **chore: track tollbooth-dpyc through 0.52.0.** Picks up the vault_source/purchase_mode decoupling: NewEngland Authority now explicitly uses `vault_source="env"` (self-provision Neon from env) and `purchase_mode="auto"` (derive direct/certified from registry chain; resolves to "certified" under NorthAmerica). No wire-API changes. The server.py comments clarify NewEngland's sub-Authority position.
- Previously tracked: **0.49.0 — REQUIRED for the operator bootstrap NIP-33 switchover.** Cold switchover with no fallback; after deploy, re-run `get_operator_config`/`register_operator` per operator. (Also carried: deferred-adoption tools, dynamic tenant ownership + `repair_operator_schema`, the 0.47.0 dunning.)
- docs: add a DPYC ecosystem peer-repo section to the README (includes the cypher-mcp newcomer).

## [0.4.1] — 2026-06-11
- chore: track tollbooth-dpyc through 0.44.15 — SDK audit hardening (correctness fixes for credit-tranche expiration in 0.44.9 and proof-reply handling in 0.44.10; blocking mypy + coverage gates). No wire-API changes.

## [0.4.0] — 2026-05-16

### Changed — collapse to thin wheel consumer (mirrors canonical 0.9.0)

Adopts the `tollbooth.authority` mixin from tollbooth-dpyc 0.22.0.
Identical refactor to canonical tollbooth-authority @ 80e7c35:

- `server.py` shrinks from ~1000 lines to ~80 (actor-specific config only)
- 8 modules deleted (actor, config, nostr_signing, onboarding, registry,
  replay, role_migration, tenant_provisioner) — code lives in
  `tollbooth.authority.*`
- `AuthorityActor` re-export from package `__init__.py` removed (no
  external consumers)

This is the moment NE genuinely becomes its own thing rather than a
fork of tollbooth-authority. Every piece of generic Authority code is
now wheel-resident; this repo holds only NE's identity (npub via env),
display name, instructions, and Neon region.

Pin bumped to `tollbooth-dpyc[nostr]==0.22.0`.


## [0.3.0] — 2026-05-16

### Changed — escalate onboarding to registered parent (NorthAmerica)

`confirm_authority_claim` / `check_authority_approval` now use the
wheel's `resolve_my_parent_npub` instead of the local `_resolve_prime_npub`.
NE's registered upstream in dpyc-community is NorthAmerica, so NE's
onboarding flow now escalates to NA — not Prime. This is the change
that makes the 3-deep Authority chain (Prime → NA → NE) work end-to-end.

Pin bumped to `tollbooth-dpyc[nostr]==0.20.0`. Local
`_resolve_prime_npub` deleted. `OnboardingChallenge.prime_npub` →
`parent_npub`.

## [0.2.0] — 2026-05-16

### Changed — adopt tollbooth-dpyc v0.19.0, drop local proof helper

Same mirror as canonical tollbooth-authority @ 2912147: the wheel now
owns proof verification via `tollbooth.identity_proof.require_proof`.

- Pinned `tollbooth-dpyc[nostr]==0.19.0`.
- Deleted the local `_verify_operator_proof` helper (5 callers updated
  to use `require_proof` from the wheel).
- Deleted the `check_balance` override (wheel's standard now does what
  the override did).
- Deleted the `mcp._tool_manager._tools.pop(...)` workaround.

## [0.1.0] — 2026-05-16

- Initial scaffold from the `tollbooth-authority` template
- Identity: `npub157hmysd6sw7m8kkldycnnc74mssxp9nweddk2p5sw9l2nktr950qyxrenn`
  (regenerated after the original npub's nsec was lost during onboarding)
- Role: sub-regional certifier for New England
- Upstream: Tollbooth-Authority-NorthAmerica
- Code identical to `tollbooth-authority`; only service-name, identity,
  and registry metadata differ

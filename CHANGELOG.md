# Changelog

All notable changes to this project will be documented in this file.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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

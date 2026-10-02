---
project: hsm-pki-trust-anchor
status: parked
phase: ""
phase_status: not started
next: >-
  Resume when a test HSM is available; first action: re-run the Luna
  conformance suite in multivendor-hsm-pki and compare with its
  docs/test-matrix.md.
blocked_on: "A test HSM (Luna or nShield) is needed for the next test"
needs_owner: false
needs_owner_why: ""
updated: 2026-10-02
---

# Progress

**Parked on 2026-10-02.** This repository holds the published inventory
signing key, the root of trust the companion repositories verify against,
and nothing that runs. It is parked with them: the next step on that side
needs a test HSM. There is no phase plan here and no test matrix; the
`next` line points at multivendor-hsm-pki's. Nothing changes while parked.

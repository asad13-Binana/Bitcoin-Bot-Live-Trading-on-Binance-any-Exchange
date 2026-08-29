# AWS TestNet evidence applicability to Bitcoin LIVE — 30 August 2026

## Scope and evidence status

The supplied runtime evidence came from the Bitcoin TestNet AWS experiment, not
from this LIVE repository on AWS or Oracle. The shared non-core controls were
reproduced against LIVE baseline commit
`ee6c00e5f590361c9c1aae72aaf68ecfcf9e911b`. No TestNet incident is represented
as a completed LIVE-host test.

## Safe parity corrections in this change

- The execution sidecar uses five bounded retries instead of an unlimited
  reconciliation restart loop.
- Telegram starts independently of sidecar health once Freqtrade is healthy,
  preserving owner visibility while entries stay fail-closed.
- Health checking uses the immutable `bitcoin-live` identity and its private
  config snapshot; repository-root `.env` fallback is removed.
- Unknown or stale `mode_*` Telegram callbacks fall back safely instead of
  raising a routing exception.
- Every static Telegram menu button has an owner-response regression test.
- Active Oracle guidance now uses the `bitcoin-live` paths, runner, wrapper,
  validator, monitoring port and rendered systemd unit names.

## Deliberately unresolved protected-core issue

The TestNet post-exit residual-balance finding concerns execution/risk core.
This LIVE parity change does not change ownership semantics, durable state,
reconciliation, order placement or the protected strategy. Any future core fix
must first be separately authorised, implemented and proven on authenticated
TestNet before being considered here.

## Readiness classification

- GitHub/source: ready for pull-request validation after generated integrity
  records and CI pass.
- Oracle: pending installation and real-host validation.
- LIVE package: simulation-only.
- LIVE money: prohibited by the recorded failed profitability/drawdown gate.

# ParamLog coverage manifest

Last updated: 2026-11-23 (values grow as recording continues).

## What is recorded

| Dataset | Since (UTC) | Cadence | Scope |
|---|---|---|---|
| Market registry snapshots | 2026-11-22 04:48 | Daily 00:05 | All perp dexes: native + xyz, flx, vntl, hyna, km, abcd, cash, para, mkts, io — 354 markets (80 tier-A, 274 tier-B) |
| Deployer meta snapshots | 2026-11-22 | Daily raw save; polled every 5 min for changes | Every market on every dex above |
| Meta-change ledger | 2026-11-22 | Derived from the 5-minute poll diffs | deployerFeeScale, growthMode, isDelisted |
| Settled funding | 2026-11-22 02:00 | Hourly | All 354 registered markets |
| Trades | 2026-11-22 | Continuous push | Tier-A markets (89 coins observed incl. focus/control) |

## Known gaps (stated, not hidden)

- **Funding multiplier changes are not yet recorded.** The Hyperliquid meta
  feed used for polling does not carry the per-asset funding multiplier; a
  recorder upgrade to poll the dex-level endpoint is planned. Until then,
  ParamLog does not report funding-multiplier history.
- **Trading halts are not yet recorded as ledger events.** The meta feed has
  no halt flag. Delisting state *is* recorded. If halt history matters to
  you, ask before relying on ParamLog for it.
- **Ledger rows before 2026-11-22 do not exist.** Markets delisted before
  recording began are not in the ledger.
- Snapshot-derived rows carry day-level bounds; poll-diff rows carry the
  5-minute poll bounds. Every row states its precision and sources.
- No changes had been observed in the ledger as of 2026-11-23 — recording
  began 2026-11-22 and no deployer changed a tracked parameter in the first
  day. The ledger fills as changes occur; the free tier always includes the
  most recent 7 days of changes.

## Freshness

Hosted responses apply a minimum 15-minute delay to all market data. The
data store is refreshed hourly from the recorder.

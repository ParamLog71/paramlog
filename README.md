# ParamLog

**The change ledger for Hyperliquid HIP-3 deployer parameters.**

HIP-3 deployers can change a market's deployer fee scale, growth mode, or
delisting state at any time. Those changes silently rewrite the assumptions of
every strategy running on the market. ParamLog polls every HIP-3 dex every
five minutes and records **when each parameter changed, from what to what**,
with the observation window for each change — so you can check, after the
fact, exactly what a market's parameters were at any recorded moment.

## Connect

Hosted MCP (Streamable HTTP):

- Endpoint: `https://paramlog.com/mcp`
- Auth: `Authorization: Bearer <your key>` on every request
- Get a free key: email **support@paramlog.com** (100 calls/month, data
  delayed at least 15 minutes, meta changes for the last 7 days)
- 30-day pass: **$29** via https://paramlog.com (2,000 calls, full recorded
  meta-change history, funding by date range)

Client configuration (any MCP client that supports remote servers with
custom headers):

```json
{
  "mcpServers": {
    "paramlog": {
      "url": "https://paramlog.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

Plain HTTP example:

```bash
curl -X POST https://paramlog.com/mcp \
  -H "Authorization: Bearer YOUR_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_meta_changes","arguments":{"dex":"para","limit":10}}}'
```

## Tools

| Tool | Description |
|---|---|
| `list_markets` | Recorded perp markets (native + HIP-3) with tier, 24h notional volume, deployer fee scale, growth mode. |
| `get_market_meta` | Current recorded meta for one market, with observation timestamps. |
| `get_funding_history` | Settled hourly funding rate and premium for one market. |
| `get_recent_trades` | Recent recorded trades (time, price, size, side) for tier-A markets. |
| `get_meta_changes` | The ledger: parameter changes with old/new values, observation bounds, and source. Filter by dex, coin, and time range; paginate with `limit`/`offset`. |

Ledger semantics: a change row reports the last observation of the old value
and the first observation of the new value — the change happened inside that
window. Absence of a field in the source feed is reported as unknown, never
guessed. See [docs/coverage.md](docs/coverage.md) for what is recorded, since
when, and the known gaps.

## Coverage (summary)

- Recording since **2026-11-22**; all Hyperliquid perp dexes (native + 10
  HIP-3 dexes), 354 markets in the registry (80 tier-A with trades).
- Meta polled every 5 minutes; settled funding recorded hourly.
- Tracked changes: deployer fee scale, growth mode, delisting state.

## Notes

ParamLog is a read-only data service. It does not provide trading advice and
is not affiliated with Hyperliquid. Support is by email only:
support@paramlog.com.

## License

The contents of this repository are MIT-licensed (see LICENSE). The hosted
service and its data are provided under the terms presented at checkout and
in the tool responses.

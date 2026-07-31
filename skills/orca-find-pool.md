---
name: Find and inspect an Orca liquidity pool
description: Search Orca Whirlpools for a token pair and read a pool's stats via the Orca Public REST API.
api: openapi/orca-openapi-original.json
operations: [search_whirlpools, get_whirlpool, get_protocol_info]
---

# Find and inspect an Orca liquidity pool

Use the open, read-only Orca Public REST API (`https://api.orca.so/v2/solana`) to
locate a pool and read its state. No authentication is required.

## Steps

1. **(Optional) Check protocol health** — `GET /protocol` (`get_protocol_info`)
   returns protocol-wide TVL, 24h volume, fees, and revenue.
2. **Search for the pool** — `GET /pools/search?q=SOL-USDC` (`search_whirlpools`).
   Pass the token pair or a symbol in `q`. Narrow with `minTvl`, `minVolume`,
   `verifiedOnly`, and sort with `sortBy` + `sortDirection`.
3. **Read the pool** — take an `address` from the search results and call
   `GET /pools/{address}` (`get_whirlpool`) for full pool state and stats.
4. **Page through results** — responses are wrapped as `{ data, meta }`; follow
   `meta.next` (cursor) with the `next` query param, and set `size` to control
   page size.

## Rules

- Handle `400` (bad params), `404` (unknown address), `429` (rate limited —
  back off), and `500` per `errors/orca-problem-types.yml`.
- The error body is `{ inner, status }` (AppError), not RFC 9457 problem+json.
- This API is read-only; to actually swap or provide liquidity, use the
  `@orca-so/whirlpools` SDK (see `packages/orca-packages.yml`).

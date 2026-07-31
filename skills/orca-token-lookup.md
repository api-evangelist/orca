---
name: Look up an Orca-listed token
description: Search and fetch SPL token metadata and the ORCA token supply via the Orca Public REST API.
api: openapi/orca-openapi-original.json
operations: [search_tokens, get_token, get_tokens, get_circulating_supply, get_total_supply]
---

# Look up an Orca-listed token

Use the open, read-only Orca Public REST API (`https://api.orca.so/v2/solana`) to
resolve token metadata. No authentication is required.

## Steps

1. **Search by symbol** — `GET /tokens/search?q=ORCA` (`search_tokens`) returns
   matching tokens (address, decimals, name, symbol, imageUrl, tags).
2. **Fetch one token** — `GET /tokens/{mint_address}` (`get_token`) returns a
   single token by its Solana mint address.
3. **List tokens** — `GET /tokens` (`get_tokens`) with cursor params
   (`next`/`previous`/`size`) and `sort_by`/`sort_direction` for browsing.
4. **ORCA supply** — `GET /protocol/token/circulating_supply`
   (`get_circulating_supply`) and `GET /protocol/token/total_supply`
   (`get_total_supply`) return the ORCA governance token supply figures.

## Rules

- A mint address is a Solana public key (base58), e.g. wrapped SOL is
  `So11111111111111111111111111111111111111112`.
- Handle `400`/`404`/`429`/`500` per `errors/orca-problem-types.yml`; the error
  envelope is `{ inner, status }`.
- Responses are `{ data, meta }`; page with `meta.next`.

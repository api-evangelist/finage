---
name: Get real-time prices from Finage
description: >-
  Fetch the latest quote, trade, or bulk snapshot for stocks, forex pairs, and
  crypto pairs from the Finage Market Data API.
api: openapi/finage-openapi.yml
operations: [stockLastQuote, stockLastTrade, forexLastQuote, cryptoLastQuoteApi, stockSnapshotApi, marketStatusApi]
generated: '2026-07-22'
method: generated
---

# Get real-time prices from Finage

## Setup

- Base URL: `https://api.finage.co.uk`. Every request is a GET with your key appended as `?apikey=YOUR_API_KEY` (sign up at https://moon.finage.co.uk/register; 3-day free trial, free plan is 1,000 requests/month).
- Always send `Accept-Encoding: gzip` — without it the API returns `406 {"error":"GZip compression required"}`.
- Errors come back as `{"error":"<message>"}`; a permission error means your plan does not cover that market (see `errors/finage-problem-types.yml`).

## Steps

1. Optionally check the market is open with `marketStatusApi` (`GET /marketstatus`).
2. For a single US stock, call `stockLastQuote` (`GET /last/stock/{symbol}`, e.g. `AAPL`) for bid/ask, or `stockLastTrade` (`GET /last/trade/stock/{symbol}`) for the last executed trade.
3. For forex, call `forexLastQuote` (`GET /last/forex/{symbol}`, e.g. `GBPUSD`). For crypto, call `cryptoLastQuoteApi` (`GET /last/crypto/{symbol}`, e.g. `BTCUSD`).
4. For many symbols at once, use `stockSnapshotApi` (`GET /snapshot/stock?symbols=AAPL,MSFT&quotes=true&trades=true`) instead of looping single-symbol calls — it saves quota.
5. Timestamps are Unix epoch milliseconds. Symbol formats are listed at https://finage.co.uk/docs/symbols.

## Rules

- The API is read-only; there is no write path or idempotency-key contract.
- Quotas are monthly per plan (`rate-limits/finage-rate-limits.yml`); no per-second limit is documented, but prefer snapshots over per-symbol loops.
- For continuous prices use the WebSocket stream instead of polling (`asyncapi/finage-websocket-asyncapi.yml`).

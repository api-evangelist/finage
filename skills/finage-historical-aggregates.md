---
name: Pull historical OHLCV aggregates from Finage
description: >-
  Retrieve historical open/high/low/close/volume bars and previous-close data
  for stocks, forex, crypto, indices, and ETFs over a date range.
api: openapi/finage-openapi.yml
operations: [stockMarketAggregatesApi, forexAggregates, cryptoAggregatesApi, cfdIndexMarketAggregatesApi, cfdEtfMarketAggregatesApi, stockMarketPreviousClose]
generated: '2026-07-22'
method: generated
---

# Pull historical OHLCV aggregates from Finage

## Setup

- Base URL `https://api.finage.co.uk`, GET only, `?apikey=YOUR_API_KEY` on every call, `Accept-Encoding: gzip` required (406 otherwise).

## Steps

1. Pick the asset-class endpoint:
   - US stocks: `stockMarketAggregatesApi` — `GET /agg/stock/{symbol}/{multiply}/{time}/{from}/{to}` (e.g. `/agg/stock/AAPL/1/day/2025-02-05/2025-02-07`).
   - Forex: `forexAggregates` — `GET /agg/forex/{symbol}/{multiply}/{time}/{from}/{to}` (e.g. `GBPUSD`).
   - Crypto: `cryptoAggregatesApi` — `GET /agg/crypto/{symbol}/{multiply}/{time}/{from}/{to}` (e.g. `BTCUSD`).
   - CFD indices / ETFs: `cfdIndexMarketAggregatesApi` / `cfdEtfMarketAggregatesApi`.
2. Date bounds use `YYYY-MM-DD`; the `{time}` resolution takes values like `minute`, `hour`, `day` multiplied by `{multiply}`.
3. Each result row is an OHLCV bar: `o`, `h`, `l`, `c`, `v`, with `t` as epoch milliseconds; `totalResults` counts rows.
4. For just yesterday's bar, use the cheaper previous-close endpoints (e.g. `stockMarketPreviousClose` — `GET /agg/stock/prev-close/{symbol}`).

## Rules

- Historical depth varies by plan and market (e.g. up to 19 years US stocks, 13 years forex, 8 years crypto on Professional plans — see `plans/finage-plans.yml`).
- Batch date ranges instead of per-day calls; Basic plans carry a 100,000 calls/month quota.

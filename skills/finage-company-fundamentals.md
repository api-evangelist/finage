---
name: Research company fundamentals with Finage
description: >-
  Pull financial statements, ratios, holders, calendars, and market movers for
  equity research from the Finage Fundamentals endpoints.
api: openapi/finage-openapi.yml
operations: [marketSearchApi, stockMarketDetailsApi, incomeStatements, balanceSheetStatements, cashFlowStatements, financialRatios, institutionalHoldersApi, earningsCalendar, dividendsCalendar, mostActiveUsStocksApi]
generated: '2026-07-22'
method: generated
---

# Research company fundamentals with Finage

## Setup

- Base URL `https://api.finage.co.uk`, GET only, `?apikey=YOUR_API_KEY`, `Accept-Encoding: gzip` required.
- Fundamentals endpoints live under `/fnd/` and require a plan that includes fundamentals (permission errors return `{"error":"You do not have permission to make a request to this endpoint."}`).

## Steps

1. Resolve the ticker with `marketSearchApi` if you only have a company name, then profile it with `stockMarketDetailsApi` (`GET /fnd/detail/stock/{symbol}`).
2. Pull the statements — `incomeStatements`, `balanceSheetStatements`, `cashFlowStatements` (`GET /fnd/income-statement/{symbol}?limit=10&period=quarter`, etc.). `period` is `quarter` or annual; `limit` caps rows returned.
3. Add valuation context with `financialRatios` (`GET /fnd/financial-ratios/{symbol}`) and ownership via `institutionalHoldersApi` (`GET /fnd/funds/institutional-holder/{symbol}`).
4. Watch upcoming events with `earningsCalendar` and `dividendsCalendar` (date-bounded with `from`/`to` in `YYYY-MM-DD`).
5. Screen the broader market with movers endpoints such as `mostActiveUsStocksApi` (`GET /fnd/market-information/us/most-actives`).

## Rules

- All data is read-only; cache statement responses — fundamentals change quarterly, and Basic plans meter 100,000 calls/month.
- Cross-check symbols against https://finage.co.uk/docs/symbols; non-US listings use exchange-suffixed tickers (e.g. `TBP.TO`).

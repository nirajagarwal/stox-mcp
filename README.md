# stox-mcp

**Give your AI the numbers that matter.** An MCP server over the
[stox.market](https://stox.market) metric layer — *context, not data*: every
finance feed hands a model raw bars and statements, and the model still can't
answer "is this number good?". This server ships the judgment layer instead.

**Live endpoint:** `https://mcp.stox.market/mcp` (streamable HTTP) ·
**Demo & docs:** [stox.market/mcp](https://stox.market/mcp)

```bash
claude mcp add --transport http stox https://mcp.stox.market/mcp
```

## Tools

| tool | what it answers |
|---|---|
| `fingerprint` | The character of one stock: 16 percentile scores vs the S&P 500 cohort across four panels (Market Beat, Trend Persistence, Business Growth, Pressure), the P4 composite, curated peers, a one-line read. ~550 tokens. |
| `compare` | The 16-score matrix for 2–5 tickers, side by side. |
| `peers` | TRUE competitive peers — curated from segment overlap, not GICS (Costco → WMT/TGT/KR, not "Discount Stores"). |
| `theme_cluster` | Who a stock *moves with*: co-movement clusters from price behavior (market-removed residuals → random-matrix cleaning → Ward linkage). |
| `screen` | Quartile screen over the four headline scores, optionally per sector, sorted by the P4 composite. |
| `market_regime` | Market-stress stance, per-indicator bands, the VIX 5-yr percentile (the one input validated as predicting forward risk), and the S2 stress-confirmation badge. |

## Design

- **Token economy** — compact JSON, score + band + a one-line read; a
  fingerprint is ~550 tokens where a statements dump is 20,000.
- **Percentiles, not raw numbers** — every score is a cross-sectional rank
  against the real cohort (100 = best), so "is it good?" is answered by
  construction.
- **Provenance** — snapshot dates on scores, links back to the full picture.
- **Informational only** — descriptive statistics; never investment advice,
  no buy/sell recommendations. Derived metrics only; this is not a
  market-data feed.

Universe: S&P 500 + NASDAQ-100 + curated ETFs. Coverage and metric
definitions: [stox.market](https://stox.market).

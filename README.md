# SMC Confluence Mapper

A TradingView indicator (Pine Script v6) that maps a chart the way a Smart Money Concepts trader would. It marks structure and liquidity, confirms with classic candlestick patterns, overlays Fibonacci on the live impulse leg, and scores every bar for confluence. When enough factors line up under your entry rules, it plans the trade: it decides between a **market** or **limit** order, sets the stop beyond the stop-hunt, targets the next pool of liquidity, and tracks the setup to its outcome.

## What it draws

| Layer | What you see |
|---|---|
| Market structure | Swing labels (HH, HL, LH, LL), BOS (solid line) and CHoCH (dashed line) |
| Order blocks | Thin OB boxes, only with displacement, height-capped, no overlaps; removed (or greyed) once invalidated |
| Fair value gaps | Imbalances above a minimum ATR size; removed at 50% (CE) or full fill |
| Liquidity | BSL / SSL pools at swing highs / lows, EQH / EQL when equal, `$` where a pool is swept |
| Candlestick patterns | 18 patterns as plain text, at points of interest and in the trend direction by default |
| Fibonacci | Retracement on the current leg (0, 0.382, 0.5 EQ, 0.618, 0.705, 0.786, 1), OTE box, extension |
| Premium / discount | Optional boxes for the upper and lower half of the current leg |
| Signals | `BUY MKT 5/7` or `SELL LMT 4/7`; hover for the full checklist, order type and levels |
| Trade plan | Position-tool style risk / reward shading with entry (dashed while a limit is pending), SL, TP1, TP2; finished trades shrink to a result label like `TP2 hit +2.4R` |
| Dashboard | Bias, structure, HTF, fib, P/D, nearest BSL/SSL, killzone, active zones, candle, live scores, open setup, last signal, results in R |

## Install on TradingView

1. Open a chart, then the **Pine Editor** tab at the bottom.
2. Create a new indicator, delete the template, paste the whole of `src/smc_confluence_mapper.pine`.
3. Click **Save**, then **Add to chart**.
4. For alerts: chart menu → **Add alert** → condition **SMC Confluence Mapper** → pick `Buy setup`, `Sell setup`, `Limit filled`, `Take profit hit`, `Stop loss hit`, `BOS`, `CHoCH`, `Liquidity sweep`, or **Any alert() function call** for messages that include order type, entry, SL and TPs.

## Confluence factors

Each factor scores 1 point. Seven are always active; HTF bias and killzone are optional.

| # | Buy | Sell |
|---|---|---|
| 1 | Bullish structure (last break was up) | Bearish structure |
| 2 | Bullish order block tapped | Bearish order block tapped |
| 3 | Bullish FVG tapped | Bearish FVG tapped |
| 4 | Price in the fib golden zone of a bull leg | Golden zone of a bear leg |
| 5 | Price in discount | Price in premium |
| 6 | Bullish candlestick pattern | Bearish candlestick pattern |
| 7 | Sell-side liquidity (swing low / EQL) swept | Buy-side liquidity (swing high / EQH) swept |
| 8 | HTF structure bullish (optional) | HTF structure bearish (optional) |
| 9 | Inside London / New York killzone (optional) | Same |

## Market or limit?

Every signal plans both executions. **MARKET** is used when the signal candle closed close to the optimal price inside the zone (within *Market entry tolerance × ATR*) and the R:R still holds. Otherwise a **LIMIT** order is placed back at the optimal price (proximal edge, 50% of the zone, or OTE 0.705), and it is cancelled if price runs to TP1 first or it expires.

A signal fires only on a closed bar, when the score reaches the minimum **and** the required rules pass. The full rule set is in [docs/entry-rules.md](docs/entry-rules.md); definitions of every concept and pattern are in [docs/concepts.md](docs/concepts.md).

## Key settings

| Setting | Default | Effect |
|---|---|---|
| Swing length | 5 | Bars each side to confirm a swing. Raise on lower timeframes for cleaner structure |
| Break confirmation | Close | `Wick` makes structure react faster but noisier |
| Minimum confluence score | 4 | Out of 7 (8 with HTF on) |
| Require structure / POI / candle | On / On / On | Hard filters on top of the score |
| Minimum R:R to TP1 | 1.5 | Setups with a nearer target are skipped |
| Stop-loss buffer | 0.25 × ATR | Added beyond the zone / sweep wick |
| Order type | Auto | Or force Market only / Limit only |
| Limit price inside zone | 50% of zone | Proximal edge fills more, OTE 0.705 gives better R:R |
| Close % at TP1 / SL to entry | 50% / On | Partial and break-even management |
| Line width / Label size | 1 / Tiny | Global visual weight |
| Extend zones right | 10 bars | How far boxes project past the current bar |

## Repainting

- Swings are confirmed `Swing length` bars after they form (that is how pivots work); labels are placed back on the actual swing bar.
- Signals and alerts only fire on confirmed (closed) bars.
- HTF bias uses the last **closed** higher-timeframe bar, so it does not repaint.
- Fibonacci, OTE and dashboard drawings are redrawn on the last bar to follow the live leg.
- Liquidity pools appear once their swing is confirmed.

## Project layout

```
smc-confluence-mapper/
├── src/smc_confluence_mapper.pine   the indicator
├── docs/entry-rules.md              the exact signal logic
├── docs/concepts.md                 SMC, fib and candlestick definitions used in code
├── CHANGELOG.md
├── LICENSE
└── README.md
```

## Disclaimer

This indicator is an analysis tool, not financial advice. Past signals do not guarantee future results. Test on demo and forward-test before trading real money.

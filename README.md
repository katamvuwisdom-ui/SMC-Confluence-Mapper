# SMC Confluence Mapper

A TradingView indicator (Pine Script v6) that maps a chart the way a Smart Money Concepts trader would, confirms with classic candlestick patterns, overlays Fibonacci on the live impulse leg, and scores every bar for confluence. When enough factors line up under your entry rules, it annotates a complete trade setup: entry zone, stop loss, TP1 and TP2 with R-multiples.

## What it draws

| Layer | What you see |
|---|---|
| Market structure | Swing labels (HH, HL, LH, LL), BOS (solid line) and CHoCH (dashed line) |
| Order blocks | Bull / bear OB boxes from the candle that made the pullback extreme before each break; removed (or greyed) once invalidated |
| Fair value gaps | Three-candle imbalances above a minimum ATR size; removed once fully filled |
| Liquidity | Sell-side / buy-side sweep markers (wick through a recent extreme, close back inside) |
| Candlestick patterns | 18 patterns, labelled everywhere or only at points of interest |
| Fibonacci | Retracement on the current leg (0, 0.382, 0.5 EQ, 0.618, 0.705, 0.786, 1), golden-zone box, extension target |
| Premium / discount | Optional boxes for the upper and lower half of the current leg |
| Signals | BUY / SELL label with score (e.g. `BUY 5/7`) and the pattern name; hover for the full checklist |
| Trade setup | Entry-zone box plus entry, SL, TP1, TP2 lines with prices and R-multiples |
| Dashboard | Bias, last structure event, HTF bias, fib position, P/D, active OBs/FVGs, liquidity, candle, live scores, last signal |

## Install on TradingView

1. Open a chart, then the **Pine Editor** tab at the bottom.
2. Create a new indicator, delete the template, paste the whole of `src/smc_confluence_mapper.pine`.
3. Click **Save**, then **Add to chart**.
4. For alerts: chart menu → **Add alert** → condition **SMC Confluence Mapper** → pick `SMC-CM: Buy setup`, `Sell setup`, `BOS`, `CHoCH`, `Liquidity sweep`, or **Any alert() function call** for messages that include entry, SL and TPs.

## Confluence factors

Each factor scores 1 point. Seven are always active; the eighth (HTF bias) is optional.

| # | Buy | Sell |
|---|---|---|
| 1 | Bullish structure (last break was up) | Bearish structure |
| 2 | Bullish order block tapped | Bearish order block tapped |
| 3 | Bullish FVG tapped | Bearish FVG tapped |
| 4 | Price in the fib golden zone of a bull leg | Golden zone of a bear leg |
| 5 | Price in discount | Price in premium |
| 6 | Bullish candlestick pattern | Bearish candlestick pattern |
| 7 | Sell-side liquidity swept recently | Buy-side liquidity swept recently |
| 8 | HTF structure bullish (optional) | HTF structure bearish (optional) |

A signal fires only on a closed bar, when the score reaches the minimum **and** the required rules pass. The full rule set is in [docs/entry-rules.md](docs/entry-rules.md); definitions of every concept and pattern are in [docs/concepts.md](docs/concepts.md).

## Key settings

| Setting | Default | Effect |
|---|---|---|
| Swing length | 5 | Bars each side to confirm a swing. Raise on lower timeframes for cleaner structure |
| Break confirmation | Close | `Wick` makes structure react faster but noisier |
| Minimum confluence score | 4 | Out of 7 (8 with HTF on) |
| Require structure / POI / candle | On / On / On | Hard filters on top of the score |
| Minimum R:R to TP1 | 1.5 | Setups with a nearer target are skipped |
| Stop-loss buffer | 0.25 × ATR | Added beyond the entry zone |
| Label candlestick patterns | At POI only | Keeps the chart readable |

## Repainting

- Swings are confirmed `Swing length` bars after they form (that is how pivots work); labels are placed back on the actual swing bar.
- Signals and alerts only fire on confirmed (closed) bars.
- HTF bias uses the last **closed** higher-timeframe bar, so it does not repaint.
- Fibonacci, golden-zone and dashboard drawings are redrawn on the last bar to follow the live leg.

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

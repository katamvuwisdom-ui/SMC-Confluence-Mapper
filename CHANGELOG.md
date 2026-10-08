# Changelog

## [2.0.0] - 2026-10-08

### Added
- Order execution engine: Auto / Market only / Limit only. Auto picks MARKET when price is still at the optimal level and R:R holds, else a LIMIT at the zone's proximal edge, 50% (mean threshold / CE) or OTE 0.705.
- Trade tracking: limit fill, missed, expired, TP1 (partial + optional break-even), TP2, SL, timeout; results in R on the dashboard.
- Liquidity pools (BSL / SSL) from confirmed swings, EQH / EQL merging, `$` sweep markers.
- Liquidity-based targets: TP1 = nearest pool or leg extreme ≥ min R away; TP2 = next pool or fib extension.
- Stop placement beyond the sweep wick when a sweep preceded the entry.
- Optional killzone factor and shading (London / New York).
- Alerts: limit filled, take profit hit, stop loss hit, limit cancelled.
- Style inputs: line width, label size, zone extension, dashboard text size.

### Changed
- Much lighter drawings: thin borders, more transparent fills, tiny labels, text-only candlestick labels, short zone boxes.
- Order blocks: last opposite candle, displacement filter, height cap, no overlapping boxes. Defaults: 3 per side.
- FVGs: removed at 50% fill (CE) by default, no overlapping boxes, 3 per side, min size 0.3 × ATR.
- Liquidity sweep factor now uses swept swing pools instead of a rolling 20-bar extreme.
- Candlestick labels only for trend-aligned patterns by default.
- Trade drawings replaced with a position-tool style plan; finished trades collapse to one result label.

## [1.0.0] - 2026-10-08

### Added
- Market structure: swing labels (HH/HL/LH/LL), BOS and CHoCH with close or wick confirmation.
- Order blocks from the pullback extreme before each structure break, with invalidation.
- Fair value gaps with ATR size filter and fill removal.
- Liquidity sweep detection and markers.
- 18 candlestick patterns with per-family toggles and "At POI only" labelling.
- Fibonacci retracement on the live leg, golden zone, extension target, optional premium/discount zones.
- Confluence scoring (7 factors, 8 with optional HTF bias) and BUY/SELL signals with checklist tooltips.
- Trade setup annotation: entry zone, entry, SL, TP1, TP2 with R-multiples.
- Dashboard and alerts (alertcondition + alert() with full levels).

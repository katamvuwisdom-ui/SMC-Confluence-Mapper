# Changelog

## [3.0.0] - 2026-10-08

### Added
- Entry model "Sweep → Shift → Retrace" (default): liquidity sweep, then structure break the other way, then entry on the pullback into a zone.
- Plain-language trade card: entry with date and time, stop and targets in price, pips and R, the reasons in simple words, the plan step by step, and the track record.
- Expected path drawn from current price to entry, TP1 and TP2.
- 50% line (mean threshold / consequent encroachment) inside order blocks and FVGs.
- Hanging Man and Inverted Hammer patterns; plain-language meaning for every pattern.
- Alert message format: plain text (copy and share) or JSON for a webhook bridge.

### Changed
- Candlestick patterns rebuilt from textbook definitions with prior-trend context and a minimum candle size.
- Order blocks use only the OB candle's own range (no longer stretched to the leg extreme). Max height 1.5 × ATR; 2 per side.
- Trade labels are filled, high-contrast badges showing price, pips, R and time. Vivid default colours.
- Minimum R:R raised to 2.0.

### Removed
- The confluence dashboard (replaced by the trade card).

## [2.1.0] - 2026-10-08

### Added
- Trade card: side panel with the latest setup — side, order type and status; entry, stop loss, TP1, TP2 with price, pips and R; risk:reward; confluence score; and plain-language reasons (confluence checklist, why market or limit, why that stop, why those targets).
- Pip calculation: auto pip size (forex 0.0001 / JPY 0.01, gold 0.1, other symbols 1 tick) with a manual override.
- Pip distances in the signal tooltip.

### Changed
- All chart text (labels, box text, dashboard, card) uses one Text colour setting, white by default.

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

# Changelog

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

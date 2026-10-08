# Entry Rules

This is the exact logic the indicator runs. If you change a rule, change it here and in `src/smc_confluence_mapper.pine` together.

## 1. Context: the current leg

- Every time price breaks a confirmed swing high (bullish BOS / CHoCH), a **bullish leg** starts at the lowest low between that swing high and the break, and runs to the highest high made since. The leg high keeps extending while the leg is active.
- A break of a confirmed swing low starts a **bearish leg** the same way in reverse.
- The leg is the dealing range for Fibonacci, premium/discount and targets.
- **CHoCH** = the break is against the previous structure direction. **BOS** = the break continues it.

## 2. Liquidity (how market makers see the chart)

- Every confirmed swing high leaves **buy-side liquidity (BSL)** above it: stop losses of shorts and breakout buy orders.
- Every confirmed swing low leaves **sell-side liquidity (SSL)** below it.
- Two swings within *Equal highs / lows tolerance* merge into one stronger pool: **EQH** / **EQL**.
- When price trades through a pool, the pool is removed. If the candle **closes back inside**, it is a **sweep** (a stop-hunt) and is marked `$`.
- A SSL sweep is a bullish factor; a BSL sweep is a bearish factor, for *Sweep stays valid for* bars.
- The model: price sweeps liquidity on one side, returns to a point of interest, then expands toward the liquidity on the other side.

## 3. Points of interest (POI)

| POI | Buy tap | Sell tap |
|---|---|---|
| Order block | Low trades into an active bull OB and the bar closes above its bottom | High trades into an active bear OB and closes below its top |
| Fair value gap | Low trades into an active bull FVG and closes above its bottom | High trades into an active bear FVG and closes below its top |
| OTE / golden zone | Bull leg: low reaches the 0.618 level and close holds above 0.786 | Bear leg: high reaches 0.618 and close holds below 0.786 |

Order block quality filters:

- The OB candle is the last opposite-colour candle at the pullback extreme before the break.
- With *Require displacement*, the move away must leave an FVG or a strong candle (body > 1.5 × 14-bar average).
- If the wick range is taller than *Max OB height × ATR*, it is refined to the body; if the body is still too tall, the OB is skipped.
- A new OB that overlaps an active OB on the same side is skipped (no stacked boxes). Same for FVGs.
- An OB is invalidated when a candle closes through its far side. An FVG is removed at 50% fill (CE) or full fill, per setting.

## 4. Entry model: Sweep → Shift → Retrace (default)

The core SMC sequence, all within *Setup window* bars:

1. **Sweep** — price takes liquidity: dips under a swing low (SSL) for buys, or spikes above a swing high (BSL) for sells, and closes back inside.
2. **Shift** — after the sweep, price breaks structure the other way (BOS / CHoCH).
3. **Retrace** — price pulls back into an order block, FVG or the OTE zone and a confirmation candle closes there.

Switch *Entry model* to *Confluence score only* to drop the sweep → shift requirement.

## 5. Scoring

Each true factor adds 1 point. 7 core factors, plus HTF bias and killzone when enabled (max 9).

## 6. Signal conditions (all must pass)

**BUY**

1. Bar is closed.
2. If *One trade at a time*: no limit order pending and no trade open.
3. With the default model: a sell-side sweep followed by a bullish structure break, both within the setup window.
3. If *Require structure*: last structure break was bullish.
4. If *Require POI*: bull OB, bull FVG or OTE zone tapped on this bar.
5. If *Require candle*: a bullish candlestick pattern closed on this bar.
6. Score ≥ *Minimum confluence score*.
7. R:R to TP1 (from the chosen entry price) ≥ *Minimum R:R*.
8. At least *Bars between signals* since the last BUY.
9. No SELL qualified on the same bar.

**SELL** is the mirror image.

## 7. Order type: MARKET or LIMIT

1. **Entry zone** = the tapped OB, else the tapped FVG, else the OTE zone, else the signal candle.
2. **Optimal limit price** inside that zone, per *Limit price inside zone*:
   - *Proximal edge*: the near side of the zone (top for buys). Fills most often.
   - *50% of zone*: the OB mean threshold / FVG consequent encroachment. Default.
   - *OTE 0.705*: the 0.705 retracement of the leg.
   It is clamped inside the zone and never above the close for buys (below for sells).
3. **Stop loss** = beyond the farthest of: zone far edge, signal candle extreme, and the recent sweep wick, plus *SL buffer × ATR*.
4. In **Auto** mode the indicator plans both executions and picks:
   - **MARKET** at the signal close when the close is within *Market entry tolerance × ATR* of the optimal price **and** the market R:R still meets the minimum.
   - **LIMIT** at the optimal price otherwise (price has already moved away, or chasing it ruins the R:R).
5. *Market only* / *Limit only* force one type.

## 8. Targets

- **TP1** = the nearest resting liquidity at least *TP1 must be at least (R)* away: the nearest BSL/EQH pool or the leg high for buys (SSL/EQL or leg low for sells). Falls back to 2R.
- **TP2** = the next pool beyond TP1, else the Fibonacci extension (default −0.272), else TP1 + 1R.

## 9. Trade management (tracked bar by bar)

| Event | Rule |
|---|---|
| Limit filled | Price trades back to the limit price |
| Limit missed | Price reaches TP1 before the limit fills — order cancelled |
| Limit expired | Not filled within *Limit order expires after* bars |
| TP1 | *Close % at TP1* banked; if *Move SL to entry* is on, stop goes to break-even |
| TP2 | Remainder closed |
| SL | Full −1R before TP1; after TP1 the remainder closes at the (moved) stop |
| Timed out | Still open after *Close trade after* bars; closed at market |

If SL and a target are touched in the same candle, the stop is assumed first (conservative). The dashboard's **Results** row adds up every filled trade on the loaded chart history in R. It uses candle highs and lows only, with no spread or slippage, so treat it as a quick sanity check, not a full backtest.

## 10. HTF bias and killzones (optional)

- HTF bias: the same swing-break logic on the chosen higher timeframe, using its last closed bar.
- Killzones: London (02:00–05:00) and New York (07:00–10:00), New York time by default. When enabled, a signal inside a killzone gains one point.

# Entry Rules

This is the exact logic the indicator runs. If you change a rule, change it here and in `src/smc_confluence_mapper.pine` together.

## 1. Context: the current leg

- Every time price breaks a confirmed swing high (bullish BOS / CHoCH), a **bullish leg** starts at the lowest low between that swing high and the break, and runs to the highest high made since. The leg high keeps extending while the leg is active.
- A break of a confirmed swing low starts a **bearish leg** the same way in reverse.
- The leg is the dealing range for Fibonacci, premium/discount and targets.
- **CHoCH** = the break is against the previous structure direction. **BOS** = the break continues it.

## 2. Points of interest (POI)

A bar is "at a POI" when it taps at least one of:

| POI | Buy tap | Sell tap |
|---|---|---|
| Order block | Low trades into an active bull OB and the bar closes above its bottom | High trades into an active bear OB and closes below its top |
| Fair value gap | Low trades into an active bull FVG and closes above its bottom | High trades into an active bear FVG and closes below its top |
| Golden zone | Bull leg: low reaches the 0.618 level and close holds above 0.786 | Bear leg: high reaches 0.618 and close holds below 0.786 |

An OB is invalidated when a candle closes through its far side. An FVG is removed once price fully fills it.

## 3. Scoring

Each true factor adds 1 point (see README for the table). Maximum is 7, or 8 with HTF bias enabled.

## 4. Signal conditions (all must pass)

**BUY**

1. Bar is closed.
2. If *Require structure*: last structure break was bullish.
3. If *Require POI*: bull OB, bull FVG or golden zone tapped on this bar.
4. If *Require candle*: a bullish candlestick pattern closed on this bar.
5. Score ≥ *Minimum confluence score*.
6. R:R to TP1 ≥ *Minimum R:R*.
7. At least *Bars between signals* since the last BUY.
8. No SELL qualified on the same bar.

**SELL** is the mirror image.

## 5. Trade setup levels

| Level | BUY | SELL |
|---|---|---|
| Entry | Signal candle close | Signal candle close |
| Entry zone | Tapped OB → else tapped FVG → else golden zone → else signal candle body-to-low | Mirror |
| Stop loss | Lower of the bar low and zone bottom, minus *SL buffer × ATR(14)* | Higher of bar high and zone top, plus buffer |
| TP1 | Leg high (the liquidity above), or 2R if price is already above it | Leg low, or 2R |
| TP2 | Fib extension beyond the leg (default −0.272), always beyond TP1 | Mirror |

The newest *Trade setups kept on chart* setups stay drawn; older ones are removed.

## 6. Liquidity sweep definition

A bullish sweep: the bar's low breaks the lowest low of the previous *Liquidity lookback* bars but the bar closes back above it. It counts as a factor for *Sweep stays valid for* bars afterwards. Bearish sweep is the mirror at the highs.

## 7. HTF bias (optional)

The same swing-break logic runs on the chosen higher timeframe using its last closed bar. Bullish HTF structure adds a point to BUY; bearish adds a point to SELL.

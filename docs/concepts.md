# Concepts & Definitions

How each concept is defined in the code. Thresholds refer to `body` (|close − open|), `range` (high − low), upper/lower wick, `avgBody` (14-bar SMA of body) and `ATR(14)`.

## Smart Money Concepts

| Concept | Definition in code |
|---|---|
| Swing high / low | `ta.pivothigh` / `ta.pivotlow` with *Swing length* bars on each side |
| HH / LH | New swing high above / below the previous swing high |
| HL / LL | New swing low above / below the previous swing low |
| BOS | Close (or wick) beyond the last unbroken swing in the direction of the current structure |
| CHoCH | First break against the current structure direction |
| Order block | On a bullish break, the candle with the lowest low between the broken swing high and the break bar (mirror for bearish). Range = wick or body per setting |
| Fair value gap | Bull: `low > high[2]`; bear: `high < low[2]`; gap ≥ *Min gap × ATR* |
| Premium / discount | Above / below the 50% (equilibrium) of the current leg |
| Liquidity sweep | Wick through the recent *Liquidity lookback* extreme with a close back inside |

## Fibonacci

Drawn on the current leg. For a bull leg, 0 sits at the leg high and 1 at the leg low, so retracement levels count down into the pullback. Levels: 0, 0.382, 0.5 (EQ), 0.618, 0.705, 0.786, 1. The golden zone is between *Golden zone start* and *end* (default 0.618–0.786). The extension (default −0.272) is the TP2 target.

## Candlestick patterns

| Pattern | Bias | Rule |
|---|---|---|
| Bull Engulfing | Bull | Bearish candle then bullish candle whose body covers it and is larger |
| Bear Engulfing | Bear | Mirror |
| Hammer | Bull | Lower wick ≥ 60% of range, body ≤ 35%, upper wick ≤ 15% |
| Shooting Star | Bear | Upper wick ≥ 60% of range, body ≤ 35%, lower wick ≤ 15% |
| Morning Star | Bull | Big bearish candle, small-body candle (≤ 0.5 × avgBody), bullish candle closing above the first candle's midpoint |
| Evening Star | Bear | Mirror |
| Bull Harami | Bull | Large bearish candle, then a smaller bullish body inside it |
| Bear Harami | Bear | Mirror |
| Tweezer Bottom | Bull | Bearish then bullish candle with lows within 0.05 × ATR |
| Tweezer Top | Bear | Mirror at the highs |
| 3 White Soldiers | Bull | Three bullish candles, rising opens and closes, bodies > 0.6 × avgBody |
| 3 Black Crows | Bear | Mirror |
| Bull Marubozu | Bull | Body ≥ 90% of range and > 1.2 × avgBody |
| Bear Marubozu | Bear | Mirror |
| Piercing Line | Bull | Large bearish candle, bullish candle opening at/below its close and closing above its midpoint (but below its open) |
| Dark Cloud Cover | Bear | Mirror |
| Dragonfly Doji | Bull | Body ≤ 10% of range, lower wick ≥ 2 × upper wick and ≥ 50% of range |
| Gravestone Doji | Bear | Mirror |

When several patterns fire on one bar, the strongest is reported in this priority: Star → Three soldiers/crows → Engulfing → Piercing/Dark cloud → Hammer/Shooting star → Tweezer → Marubozu → Harami → Doji.

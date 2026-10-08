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
| Order block | On a bullish break, the last down-close candle at the pullback low before the displacement that broke structure (mirror for bearish). Zone = that one candle's high to low (or body). Refined to the body if taller than *Max OB height × ATR*, skipped if still too tall |
| Displacement | An FVG between the OB candle and the break, or a break candle body > 1.5 × avgBody |
| Mean threshold | 50% of an order block |
| Fair value gap | Bull: `low > high[2]`; bear: `high < low[2]`; gap ≥ *Min gap × ATR* |
| Consequent encroachment (CE) | 50% of a fair value gap |
| Premium / discount | Above / below the 50% (equilibrium) of the current leg |
| OTE | Optimal trade entry: the 0.618–0.786 retracement of the leg, 0.705 as the sweet spot |
| BSL / SSL | Buy-side liquidity above a confirmed swing high / sell-side below a swing low |
| EQH / EQL | Two swing highs / lows within *tolerance × ATR*: a stronger liquidity pool |
| Liquidity sweep | Wick through a pool with a close back inside it |
| Killzone | London 02:00–05:00 and New York 07:00–10:00, New York time |

## Fibonacci

Drawn on the current leg. For a bull leg, 0 sits at the leg high and 1 at the leg low, so retracement levels count down into the pullback. Levels: 0, 0.382, 0.5 (EQ), 0.618, 0.705, 0.786, 1. The golden zone is between *Golden zone start* and *end* (default 0.618–0.786). The extension (default −0.272) is the TP2 target.

## Candlestick patterns

Textbook definitions (Nison, *Japanese Candlestick Charting Techniques*; Bulkowski, *Encyclopedia of Candlestick Charts*). Two rules apply to every pattern:

- **Prior trend.** A reversal pattern must follow the trend it reverses. "Down" before a pattern means the bar before it closed below its 10-bar average and below the close 5 bars earlier (mirror for "up").
- **Meaningful size.** Single-candle patterns need a range of at least 0.5 × ATR(14), so tiny candles are never labelled.

| Pattern | Bias | Prior trend | Rule in code |
|---|---|---|---|
| Hammer | Bull | Down | Lower shadow ≥ 2 × body, upper shadow ≤ 10% of range |
| Hanging Man | Bear | Up | Same shape as the hammer |
| Shooting Star | Bear | Up | Upper shadow ≥ 2 × body, lower shadow ≤ 10% of range |
| Inverted Hammer | Bull | Down | Same shape as the shooting star |
| Bullish Engulfing | Bull | Down | Down candle, then an up candle whose real body covers it and is larger |
| Bearish Engulfing | Bear | Up | Mirror |
| Bullish Harami | Bull | Down | Long down candle (body ≥ average), then an up body ≤ half its size inside its body |
| Bearish Harami | Bear | Up | Mirror |
| Piercing Line | Bull | Down | Long down candle, then an up candle opening at/below its close and closing above its midpoint but below its open |
| Dark Cloud Cover | Bear | Up | Mirror |
| Morning Star | Bull | Down | Long down candle, a star (body ≤ 30% of the first) at or below its body, then an up candle closing above the first candle's midpoint |
| Evening Star | Bear | Up | Mirror |
| 3 White Soldiers | Bull | Down | Three long up candles, each opening inside the prior body, higher closes, small upper shadows (≤ 25% of range) |
| 3 Black Crows | Bear | Up | Mirror |
| Bullish / Bearish Marubozu | Either | Any | Body ≥ 90% of range and ≥ 1.3 × average body |
| Tweezer Bottom | Bull | Down | Down then up candle, lows within 5% of range, at the 5-bar low |
| Tweezer Top | Bear | Up | Mirror at the 5-bar high |
| Dragonfly Doji | Bull | Down | Body ≤ 5% of range, upper shadow ≤ 10% |
| Gravestone Doji | Bear | Up | Body ≤ 5% of range, lower shadow ≤ 10% |

When several patterns fire on one bar, the strongest is reported: Star → Three soldiers/crows → Engulfing → Piercing/Dark cloud → Hammer/Shooting star → Inverted hammer/Hanging man → Tweezer → Marubozu → Harami → Doji.

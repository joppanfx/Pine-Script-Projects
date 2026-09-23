# 📕 JoppanFX | Candlestick Patterns
## 📸 Interface Visualization
![JoppanFX Candlestick Patterns](./Assets/JoppanFX%20%7C%20Candlestick%20Patterns.png)

## 📌 Overview
The **JoppanFX: Candlestick Patterns** is a configurable price-action analysis tool designed to identify commonly used **candlestick reversal and continuation patterns** directly on the chart.

The indicator combines **candle geometry**, **trend context**, **shadow-to-body ratios**, and **engulfment conditions** to systematically identify:

- **Hammer**
- **Inverted Hammer**
- **Shooting Star**
- **Hanging Man**
- **Bullish Engulfing**
- **Bearish Engulfing**

Unlike purely visual candlestick pattern recognition, this implementation uses configurable quantitative thresholds to standardize pattern detection across different instruments and market conditions.

---

## 🛠 Technical Logic
The indicator evaluates candlestick structures using four core components:

### 1. Candle Measurements
Each candle is decomposed into its fundamental components:

$$
\text{Body} = |Close - Open|
$$

$$
\text{Range} = High - Low
$$

$$
\text{Upper Shadow} = High - \max(Open, Close)
$$

$$
\text{Lower Shadow} = \min(Open, Close) - Low
$$

These measurements provide the geometric basis for all pattern detection.

### 2. Trend Context
Reversal-type patterns are evaluated within a configurable trend context.

A downtrend is defined as:

$$
Close_{current} < Close_{n\ bars\ ago}
$$

An uptrend is defined as:

$$
Close_{current} > Close_{n\ bars\ ago}
$$

where $n$ is the **Trend Lookback** parameter.

This prevents patterns such as Hammer and Shooting Star from being identified without the corresponding preceding directional context.

### 3. Shadow and Body Validation
The indicator uses configurable ratios to determine whether a candle has the required structure.

For a long shadow:

$$
Shadow \geq Body \times R
$$

where $R$ is the **Long Shadow / Body Ratio**.

The opposite shadow is constrained by:

$$
Opposite\ Shadow \leq Body \times R_o
$$

where $R_o$ is the **Maximum Opposite Shadow / Body Ratio**.

The minimum body size relative to the total candle range is:

$$
\frac{Body}{Range} \geq B
$$

where $B$ is the **Minimum Body / Range Ratio**.

### 4. Engulfing Detection
Engulfing patterns require a directional relationship between the current and previous candles.

#### Bullish Engulfing

The previous candle must be bearish:

$$
Close_{previous} < Open_{previous}
$$

The current candle must be bullish:

$$
Close_{current} > Open_{current}
$$

When using **Real Body** mode, the current real body must contain the previous real body:

$$
Open_{current} \leq Close_{previous}
$$

$$
Close_{current} \geq Open_{previous}
$$

When using **Entire Candle** mode, the current candle must contain the previous candle's complete high-low range:

$$
High_{current} \geq High_{previous}
$$

$$
Low_{current} \leq Low_{previous}
$$

#### Bearish Engulfing

The previous candle must be bullish:

$$
Close_{previous} > Open_{previous}
$$

The current candle must be bearish:

$$
Close_{current} < Open_{current}
$$

When using **Real Body** mode:

$$
Open_{current} \geq Close_{previous}
$$

$$
Close_{current} \leq Open_{previous}
$$

When using **Entire Candle** mode:

$$
High_{current} \geq High_{previous}
$$

$$
Low_{current} \leq Low_{previous}
$$

The final pattern conditions enforce the required bullish/bearish candle relationship regardless of which engulfing method is selected.

---

## 🔒 Production Safeguards

This version of the indicator includes multiple validation layers to prevent structurally invalid patterns from being displayed.

### Zero-Range Candle Protection
Candles with no price range are rejected before pattern calculations:

$$
High - Low > 0
$$

This prevents invalid ratio calculations.

### Zero-Body Candle Protection
Candles with a zero-sized real body are excluded from pattern detection. This prevents division-by-zero conditions when evaluating body-to-range and shadow-to-body ratios.

### Minimum Body / Range Validation
Extremely small-bodied candles can produce misleading shadow ratios. The configurable **Minimum Body / Range Ratio** prevents candles with insignificant bodies from qualifying for shadow-based patterns.

### Trend Context Validation
Hammer and Inverted Hammer patterns require a preceding downtrend, while Shooting Star and Hanging Man patterns require a preceding uptrend.

This ensures the same candle geometry is not automatically classified as a reversal pattern regardless of market context.

### Trend Lookback Protection
Trend evaluation is only performed once sufficient historical bars exist for the configured lookback period.

### Engulfing History Protection
Engulfing patterns require a previous candle and are therefore explicitly restricted until at least one historical bar is available.

### Configurable Pattern Selection
Each supported pattern can be independently enabled or disabled, allowing users to focus only on the structures relevant to their analysis.

### Configurable Engulfing Method
Engulfing detection can be evaluated using either:

- **Real Body** — compares the open and close boundaries.
- **Entire Candle** — compares the complete high-low range.

### Maximum Label Protection
The indicator uses TradingView's `max_labels_count=500` setting to maintain controlled chart rendering when numerous patterns are detected.

---

## 🚀 Key Features
- **Six Candlestick Patterns**
  Detects Hammer, Inverted Hammer, Shooting Star, Hanging Man, Bullish Engulfing, and Bearish Engulfing structures.

- **Quantitative Pattern Detection**
  Uses configurable body, shadow, range, and trend thresholds rather than relying solely on visual interpretation.

- **Trend-Aware Reversal Detection**
  Requires appropriate preceding directional context for Hammer, Inverted Hammer, Shooting Star, and Hanging Man patterns.

- **Configurable Engulfing Method**
  Supports both Real Body and Entire Candle engulfment definitions.

- **Independent Pattern Controls**
  Each pattern can be individually enabled or disabled.

- **Configurable Visual System**
  Pattern colors and label visibility can be customized directly from the indicator settings.

- **Dynamic Chart Contrast**
  Label text automatically switches between black and white based on chart background luminance.

- **Quant-Grade Watermark**
  Clean, institutional-style JoppanFX branding for professional chart sharing.

---

## ⚙️ Input Parameters

### Candlestick Patterns
| Parameter | Default | Description |
|-----------|---------|-------------|
| Hammer (Bullish) | True | Enables detection of Hammer patterns |
| Inverted Hammer (Bullish) | True | Enables detection of Inverted Hammer patterns |
| Shooting Star (Bearish) | True | Enables detection of Shooting Star patterns |
| Hanging Man (Bearish) | True | Enables detection of Hanging Man patterns |
| Bullish Engulfing | True | Enables detection of Bullish Engulfing patterns |
| Bearish Engulfing | True | Enables detection of Bearish Engulfing patterns |

### Pattern Settings
| Parameter | Default | Min | Max | Description |
|-----------|---------|-----|-----|-------------|
| Trend Lookback | 3 | 1 | 50 | Number of bars used to establish preceding trend direction |
| Long Shadow / Body Ratio | 2.0 | 1.0 | — | Minimum ratio required for the dominant shadow |
| Maximum Opposite Shadow / Body Ratio | 0.5 | 0.0 | — | Maximum permitted ratio for the opposite shadow |
| Minimum Body / Range Ratio | 0.05 | 0.0 | 1.0 | Minimum real-body size relative to the candle's total range |
| Bullish Engulfing Method | Real Body | — | — | Selects Real Body or Entire Candle engulfment logic |
| Bearish Engulfing Method | Real Body | — | — | Selects Real Body or Entire Candle engulfment logic |

### Visuals
| Parameter | Default | Description |
|-----------|---------|-------------|
| Hammer | Green | Label color for Hammer patterns |
| Inverted Hammer | Green | Label color for Inverted Hammer patterns |
| Shooting Star | Red | Label color for Shooting Star patterns |
| Hanging Man | Red | Label color for Hanging Man patterns |
| Bullish Engulfing | Green | Label color for Bullish Engulfing patterns |
| Bearish Engulfing | Red | Label color for Bearish Engulfing patterns |
| Show Labels | True | Toggles all candlestick pattern labels |

---

## 📈 How to Use

### Hammer
A Hammer is identified after a preceding downtrend when the candle contains a sufficiently long lower shadow, a constrained upper shadow, and an appropriately sized body.

It can be used to identify potential bullish reversal zones when confirmed by surrounding market structure.

### Inverted Hammer
An Inverted Hammer is identified after a preceding downtrend when the candle contains a sufficiently long upper shadow and a constrained lower shadow.

The pattern can highlight potential bullish reversal conditions, particularly when supported by subsequent price confirmation.

### Shooting Star
A Shooting Star is identified after a preceding uptrend when the candle contains a sufficiently long upper shadow and a constrained lower shadow.

It can highlight potential bearish reversal conditions following upward price movement.

### Hanging Man
A Hanging Man is identified after a preceding uptrend when the candle contains a sufficiently long lower shadow and a constrained upper shadow.

The structure can indicate potential bearish reversal conditions when supported by subsequent price action.

### Bullish Engulfing
A Bullish Engulfing pattern requires a bearish previous candle followed by a bullish current candle that engulfs either:

- The previous real body, when **Real Body** mode is selected.
- The previous candle's complete high-low range, when **Entire Candle** mode is selected.

### Bearish Engulfing
A Bearish Engulfing pattern requires a bullish previous candle followed by a bearish current candle that engulfs either:

- The previous real body, when **Real Body** mode is selected.
- The previous candle's complete high-low range, when **Entire Candle** mode is selected.

### Contextual Analysis
Candlestick patterns should be evaluated alongside market structure, support and resistance, volatility, liquidity, and subsequent price confirmation rather than being treated as standalone trade signals.

---

## 📝 Installation
1. Copy the source code from the `Indicators/` folder
2. Open **Pine Editor** in TradingView
3. Paste the code and click **"Add to Chart"**

---

## ⚖️ License
This project is part of the **JoppanFX Pine Script Projects** repository.
- ✅ Individual use permitted
- ⚠️ For commercial use or derivative work, contact the repository owner

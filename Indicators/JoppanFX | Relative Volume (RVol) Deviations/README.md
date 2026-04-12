# 📕 JoppanFX | Relative Volume (RVol) Deviations
## 📸 Interface Visualization
![JoppanFX RVol Deviations](./Assets/JoppanFX%20%7C%20Relative%20Volume%20(RVol)%20Deviations.png)

## 📌 Overview
The **JoppanFX: Relative Volume (RVol) Deviations** is a quantitative liquidity-tracking tool designed to identify **institutional participation** and **volume anomalies**.

By comparing real-time volume against a rolling **Simple Moving Average (SMA)**, this indicator isolates **high-volume deviations**—periods where activity significantly exceeds the statistical norm.

This protocol is essential for identifying:
- **Breakout confirmation**
- **Climax reversals**
- **Smart money positioning**

---

## 🛠 Technical Logic
The indicator evaluates volume intensity using three core components:

### 1. Volume Basis
Calculates the average volume over a specified lookback period ($n$):

$$
\text{Avg Volume} = \frac{1}{n} \sum_{i=0}^{n-1} \text{Volume}_i
$$

### 2. Relative Deviation (RVol)
Measures current volume relative to the historical average:

$$
\text{RVol Multiplier} = \frac{\text{Volume}_{\text{current}}}{\text{Avg Volume}}
$$

### 3. Threshold Logic
Generates a signal when volume exceeds a defined multiplier ($M$):

$$
\text{Signal} = \text{Volume}_{\text{current}} > (\text{Avg Volume} \times M)
$$

### 4. Candle Classification
Each high-volume bar is classified into one of three states:

| State | Condition | Interpretation |
|-------|-----------|----------------|
| **Bullish** | `close > open` and not a Doji | Buying aggression |
| **Bearish** | `close < open` and not a Doji | Selling aggression |
| **Neutral (Doji)** | `│close − open│ ≤ mintick` | Indecision / Absorption |

---

## 🔒 Production Safeguards

This version of the indicator includes a set of robustness guardrails that ensure stable, reliable behavior across all instruments and market conditions.

### Volume Availability Check
Before any calculation is performed, the indicator verifies that valid volume data exists for the current symbol. Instruments such as indices or certain CFDs that do not report volume are handled gracefully — no signals, colors, or labels are generated on symbols with missing volume data.

### Zero Average Volume Guard
The RVol multiplier (`Volume / Avg Volume`) is only computed when the average volume is strictly greater than zero. This prevents division-by-zero errors and eliminates corrupt label output on newly listed symbols or instruments with sparse data.

### Lookback Warmup Period
Signals are suppressed for the first `n` bars (equal to the Lookback Period). During this warmup window, the SMA does not yet have sufficient data to produce a statistically meaningful average. Firing signals during this period would produce misleading deviations and is blocked entirely.

### ATR-Based Label Offset
Label placement is calculated using a fraction of the **Average True Range (ATR)** rather than a fixed tick count. This ensures labels scale correctly to the instrument's actual price volatility — a setting that works on Bitcoin works equally well on a forex pair, an equity, or an index.

$$
\text{Label Offset} = \text{ATR}_{14} \times \text{Offset Multiplier}
$$

### Multiplier Input Validation
The Volume Multiplier input enforces a minimum value of `0.1`. Entering zero or a negative value would invert the threshold logic, causing every bar to register as a high-volume event. This constraint prevents silent misconfiguration.

### Neutral Doji Classification
High-volume candles where `│close − open│ ≤ 1 mintick` are classified as **Neutral** rather than Bearish. This corrects the semantic ambiguity of the prior `close <= open` condition and aligns signal output with standard candlestick interpretation. Doji bars receive a dedicated color and a distinct label style.

---

## 🚀 Key Features
- **Dynamic RVol Calculation**
  Normalizes volume across any timeframe, ensuring "high volume" is always context-aware.

- **Three-State Candle Classification**
  Distinguishes bullish aggression, bearish aggression, and neutral absorption — including correct Doji handling.

- **ATR-Scaled Magnitude Labels**
  Displays exact volume multipliers (e.g., *3.2x*) with instrument-adaptive vertical positioning.

- **Production-Grade Safeguards**
  Gracefully handles missing volume data, zero averages, warmup periods, and bad user input without errors or misleading signals.

- **Quant-Grade Watermark**
  Clean, institutional-style branding for professional chart sharing.

---

## ⚙️ Input Parameters

### Volume Settings
| Parameter | Default | Min | Description |
|-----------|---------|-----|-------------|
| Lookback Period | 60 | 1 | Number of bars used to calculate average volume. Signals are suppressed during the warmup window. |
| Volume Multiplier | 2.5 | 0.1 | Deviation threshold. A bar must exceed `Avg Volume × Multiplier` to register as high volume. |

### Visuals
| Parameter | Default | Description |
|-----------|---------|-------------|
| High Vol Bullish | Green | Bar and label color for bullish high-volume candles |
| High Vol Bearish | Red | Bar and label color for bearish high-volume candles |
| High Vol Doji (Neutral) | White | Bar and label color for neutral / absorption candles |
| Show Volume Labels | True | Toggles the RVol multiplier label on each flagged bar |
| Label ATR Offset (Multiplier) | 0.25 | Scales label distance from the candle using ATR. Increase for more spacing on volatile instruments. |

---

## 📈 How to Use

### Breakout Confirmation
A breakout above resistance accompanied by a **Bullish RVol Deviation (Green Bar)** signals strong institutional participation, increasing the statistical probability of continuation.

### Climax Reversals
Extreme spikes (e.g., **> 4.0×**) following an extended trend often indicate exhaustion. These are high-probability setups for **V-shaped reversals** and should be evaluated in the context of surrounding structure.

### Institutional Absorption
High-volume **Neutral (Doji)** bars at key levels — now correctly classified rather than forced into a bullish or bearish bucket — suggest absorption, where large participants are accumulating or distributing without committing to a directional move. These are among the highest-conviction signals the indicator produces.

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

# 📕 JoppanFX | Price Mean Deviation Protocol

## 📸 Visual Overview
![JoppanFX Price Mean Deviation Protocol](./Assets/JoppanFX%20%7C%20TWAP%20Deviation%20Protocol.png)

## 📌 Overview
The **JoppanFX: Price Mean Deviation Protocol** is a quantitative volatility-tracking tool designed to identify price extremes relative to a **rolling price mean baseline**.  
By integrating a smoothed **OHLC4-based price mean** with dynamic **Standard Deviation bands**, this indicator provides a statistically grounded framework for identifying **price extensions** and **mean-reversion opportunities**.

Unlike standard **Bollinger Bands**, this protocol utilizes the **OHLC4** (average of Open, High, Low, and Close) to deliver a smoother and more comprehensive representation of value distribution over a defined lookback period.

> **Note on Terminology:** This indicator uses an SMA of OHLC4 as a price mean proxy. A true Time-Weighted Average Price (TWAP) requires tick-level data not natively available in Pine Script v6. This approximation holds accurately on fixed-interval timeframes (1m, 5m, 1H, etc.).

---

## 🛠 Technical Logic
The protocol operates on three core mathematical pillars:

### 1. Price Mean Basis
Calculates the rolling simple moving average of the OHLC4 price point:

$$
\text{Price Mean} = \frac{1}{n} \sum_{i=0}^{n-1} \text{OHLC4}_i
$$

### 2. Volatility Expansion
Measures the dispersion of price from the basis using **Standard Deviation ($\sigma$)**.

### 3. Deviation Bands
Projects upper and lower bands based on a user-defined multiplier (**Z-score equivalent**):

$$
\text{Upper Band} = \text{Price Mean} + (\sigma \times \text{Multiplier})
$$
$$
\text{Lower Band} = \text{Price Mean} - (\sigma \times \text{Multiplier})
$$

---

## 🚀 Key Features
- **Rolling Price Mean Basis**  
  A custom-length baseline that tracks the *fair value* of the asset using OHLC4 smoothing.

- **Dynamic Volatility Bands**  
  Real-time envelopes that expand and contract based on market conditions.

- **Confirmed Bar Signal System**  
  Candle highlighting triggers only on **confirmed (closed) bars**, eliminating mid-bar false signals on live ticks.

- **Production-Grade Reliability**  
  Built with `na()` guards on early bars, input validation, and a `max_bars_back` declaration for stability across all data feeds.

---

## ⚙️ Input Parameters
| Parameter             | Default | Min   | Description |
|----------------------|---------|-------|-------------|
| Basis Lookback       | 48      | 1     | Number of periods used to calculate the price mean and deviation |
| Deviation Multiplier | 2.0     | 0.1   | Controls band width (~95% statistical coverage at 2.0) |
| Basis Line Color     | Gray    | —     | Color of the central price mean line |
| Upper Band Color     | Red     | —     | Color of the upper deviation band |
| Lower Band Color     | Green   | —     | Color of the lower deviation band |
| Show Band Shading    | True    | —     | Toggles shading between the basis and each band |

---

## 📈 How to Use

### Mean Reversion
When price closes **outside** either band on a confirmed bar:
- **Above Upper Band (Red)** — price is statistically extended to the upside
- **Below Lower Band (Green)** — price is statistically extended to the downside  

Traders often anticipate a move back toward the **price mean baseline** in these conditions.

### Trend Strength
Persistent movement along the upper or lower band indicates **strong directional momentum** rather than an imminent reversal. Use additional confluence before fading the move.

### Volatility Filter
Band contraction signals a **volatility squeeze**, often preceding an expansion or breakout.

---

## 📝 Installation
1. Copy the source code from the `Indicators/` folder  
2. Open **Pine Editor** in TradingView  
3. Paste the code and click **"Add to Chart"**

---

## 🔖 Changelog

### v2.0
- Renamed from *TWAP Deviation Protocol* to *Price Mean Deviation Protocol* for technical accuracy
- Added `minval=0.1` guard on Deviation Multiplier input
- Implemented `na()` guards on all condition booleans for early-bar reliability
- Shifted bar coloring to `barstate.isconfirmed` — no more mid-bar signal flicker
- Replaced `plot.style_linebr` with `plot.style_line` (redundant break logic removed)
- Added `max_bars_back=500` declaration for cross-feed stability
- Dynamic watermark coloring via `chart.bg_color` — readable on both dark and light themes
- Refactored group labels to plain string constants (`G_CORE`, `G_VIS`)
- Renamed internal variables to precise, descriptive identifiers (`basisLen`, `devMult`, `aboveUpperBand`, `belowLowerBand`)

### v1.0
- Initial public release

---

## ⚖️ License
This project is part of the **JoppanFX Pine Script Projects** repository.  
- ✅ Individual use permitted  
- ⚠️ For commercial use or derivative work, contact the repository owner

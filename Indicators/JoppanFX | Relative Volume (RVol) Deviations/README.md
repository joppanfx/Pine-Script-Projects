# 🩸 JoppanFX: TWAP Deviation Protocol

## 📌 Overview
The **JoppanFX: TWAP Deviation Protocol** is a quantitative volatility-tracking tool designed to identify price extremes relative to the **Time Weighted Average Price (TWAP)**.  

By integrating a rolling TWAP basis with dynamic **Standard Deviation bands**, this indicator provides a statistically grounded framework for identifying **overbought/oversold conditions** and **mean-reversion opportunities**.

Unlike standard **Bollinger Bands**, this protocol utilizes the **OHLC4** (average of Open, High, Low, and Close) to deliver a smoother and more comprehensive representation of value distribution over a defined lookback period.

---

## 🛠 Technical Logic
The protocol operates on three core mathematical pillars:

### 1. TWAP Basis
Calculates the moving average of the OHLC4 price point:

$$
TWAP = \frac{1}{n} \sum_{i=0}^{n-1} \text{OHLC4}_i
$$

### 2. Volatility Expansion
Measures the dispersion of price from the basis using **Standard Deviation ($\sigma$)**.

### 3. Deviation Envelopes
Projects upper and lower bands based on a user-defined multiplier (**Z-score equivalent**).

---

## 🚀 Key Features
- **Rolling TWAP Basis**  
  A custom-length baseline that tracks the *fair value* of the asset.

- **Dynamic Volatility Bands**  
  Real-time envelopes that expand and contract based on market conditions.

- **Visual Signal System**  
  Automatic candle highlighting when price breaches statistical boundaries.

- **Quant-Grade Watermark**  
  Clean, non-intrusive branding for professional chart presentation.

---

## ⚙️ Input Parameters

| Parameter           | Default | Description |
|--------------------|--------|------------|
| TWAP Lookback      | 48     | Number of periods used to calculate mean and deviation |
| StdDev Multiplier  | 2.0    | Controls band width (~95% coverage at 2.0) |
| Show Fill          | True   | Toggles shading between basis and bands |

---

## 📈 How to Use

### Mean Reversion
When price closes outside:
- **Upper Deviation (Red)**  
- **Lower Deviation (Green)**  

…it is statistically extended. Traders often anticipate a move back toward the **TWAP baseline**.

### Trend Strength
Persistent movement along upper or lower bands indicates **strong directional momentum**.

### Volatility Filter
Band contraction signals a **volatility squeeze**, often preceding a breakout.

---

## 📝 Installation
1. Copy the source code from the `indicators/` folder  
2. Open **Pine Editor** in TradingView  
3. Paste the code and click **"Add to Chart"**

---

## ⚖️ License
This project is part of the **JoppanFX Pine Script Projects** repository.  

- ✅ Individual use permitted  
- ⚠️ For commercial use or derivative work, contact the repository owner

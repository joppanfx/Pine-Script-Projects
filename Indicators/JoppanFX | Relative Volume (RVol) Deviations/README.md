# 📕 JoppanFX: Relative Volume (RVol) Deviations

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

---

## 🚀 Key Features

- **Dynamic RVol Calculation**  
  Normalizes volume across any timeframe, ensuring "high volume" is always context-aware.

- **Contextual Candle Coloring**  
  Highlights bullish vs bearish high-volume candles to reveal buying or selling aggression.

- **Real-Time Magnitude Labels**  
  Displays exact volume multipliers (e.g., *3.2x*) for instant liquidity assessment.

- **Quant-Grade Watermark**  
  Clean, institutional-style branding for professional chart sharing.

---

## ⚙️ Input Parameters

| Parameter           | Default | Description |
|--------------------|--------|------------|
| Lookback Period    | 60     | Number of bars used to calculate average volume |
| Volume Multiplier  | 2.5    | Threshold for deviation (e.g., 2.5× normal volume) |
| Show Labels        | True   | Toggles multiplier display on chart |
| Label Offset       | 10     | Controls vertical spacing of labels |

---

## 📈 How to Use

### Breakout Confirmation
A breakout above resistance with a **Bullish RVol Deviation (Green Bar)** signals strong participation, increasing the likelihood of continuation.

### Climax Reversals
Extreme spikes (e.g., **> 4.0×**) after extended trends often indicate exhaustion and potential **V-shaped reversals**.

### Institutional Absorption
High-volume candles with small bodies (e.g., **Doji**) at key levels suggest **absorption**, where large players accumulate or distribute positions.

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

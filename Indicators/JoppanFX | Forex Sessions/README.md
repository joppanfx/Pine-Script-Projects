# 📕 JoppanFX | Forex Sessions

## 📸 Interface Visualization
![JoppanFX Forex Sessions](./Assets/JoppanFX%20%7C%20Forex%20Sessions.png)

---

## 📌 Overview
The **JoppanFX: Forex Sessions** indicator is a time-structured market framework designed to track **global liquidity cycles** across the four major trading sessions:

- **Sydney**
- **Tokyo**
- **London**
- **New York**

By segmenting price action into discrete session windows, this tool enables traders to analyze **session-specific volatility, range expansion, and institutional activity**.

Each session is dynamically visualized through:
- **Start / End markers**
- **Session range boxes**
- **Real-time high/low tracking**

This provides a structured lens for identifying:
- **Session breakouts**
- **Liquidity sweeps**
- **Time-based market behavior**

---

## 🛠 Technical Logic

The indicator operates on a deterministic, UTC-based session model with three primary components:

---

### 1. Session Membership Logic

Each bar is classified based on whether its timestamp falls within predefined session windows:

$$
\text{Session Active} =
\begin{cases}
h \in [S, E), & \text{if } S < E \\
h \geq S \ \lor \ h < E, & \text{if session crosses midnight}
\end{cases}
$$

Where:
- $h$ = current hour (UTC)
- $S$ = session start hour
- $E$ = session end hour

---

### 2. Session Transition Detection

Session boundaries are detected using state transitions:

$$
\text{Start} = \text{Current State} \land \neg \text{Previous State}
$$

$$
\text{End} = \neg \text{Current State} \land \text{Previous State}
$$

This ensures precise identification of:
- Session openings
- Session closures

---

### 3. Dynamic Range Tracking

During an active session, the indicator continuously updates the session range:

$$
\text{Session High} = \max(\text{High}_t, \text{Session High})
$$

$$
\text{Session Low} = \min(\text{Low}_t, \text{Session Low})
$$

This allows real-time construction of:
- Session boxes
- High/Low levels
- Volatility envelopes

---

## 🧩 Session Classification

| Session | Time (UTC) | Color | Market Context |
|--------|-----------|------|----------------|
| **Sydney** | 21 → 06 | Pink | Low liquidity / accumulation phase |
| **Tokyo** | 00 → 09 | Violet | Asian range formation |
| **London** | 07 → 16 | Yellow | High volatility / expansion |
| **New York** | 12 → 21 | Blue | Continuation or reversal phase |

---

## 🔒 Production Safeguards

This indicator is engineered with robust safeguards to ensure consistent behavior across all chart environments.

---

### UTC Time Standardization
All session calculations are strictly based on **UTC time**, eliminating discrepancies caused by:
- Broker server time differences
- Chart timezone settings
- Daylight Saving Time (DST) shifts

---

### Midnight Session Handling
Sessions that span across midnight (e.g., Sydney) are handled using a dual-condition logic (`OR` condition), ensuring:
- Seamless continuity across day boundaries
- No session fragmentation

---

### State-Based Transition Accuracy
Session start and end events are derived from **state changes**, preventing:
- Duplicate triggers
- Missed transitions
- Label repainting issues

---

### Object Lifecycle Control
All drawing objects (boxes, lines, labels) are:
- Initialized only at session start
- Updated only while active
- Terminated cleanly at session end

This prevents:
- Memory overflow
- Object stacking
- Visual clutter

---

### Dynamic Contrast System
The indicator automatically adapts its watermark color based on chart background luminance:

$$
L = 0.299R + 0.587G + 0.114B
$$

- Light backgrounds → Dark text  
- Dark backgrounds → Light text  

Ensures consistent readability across all themes.

---

## 🚀 Key Features

- **Multi-Session Market Structure**
  Clearly segments global trading sessions for time-based analysis.

- **Dynamic Session Boxes**
  Real-time visualization of session range expansion and contraction.

- **Precision Start/End Markers**
  Accurate labeling of session boundaries using state transitions.

- **Live High/Low Tracking**
  Continuously updated session extremes with visual labels.

- **Cross-Market Compatibility**
  Works seamlessly across forex, indices, crypto, and equities.

- **Adaptive UI Contrast**
  Automatically adjusts visual elements based on chart theme.

- **Quant-Grade Watermark**
  Clean, professional branding for institutional chart presentation.

---

## ⚙️ Input Parameters

This indicator is designed for **zero-configuration deployment**.

All session definitions are internally standardized using UTC, ensuring:
- Consistency across users
- Elimination of configuration errors
- Immediate usability

---

## 📈 How to Use

### Session Breakouts
Breakouts from **London or New York session ranges** often signal strong directional moves driven by institutional order flow.

---

### Liquidity Sweeps
Price frequently takes out **previous session highs/lows** before reversing. These sweeps are high-probability setups when aligned with market structure.

---

### Session Range Trading
The **Tokyo session** often forms consolidation ranges that act as:
- Accumulation zones
- Pre-breakout structures for London

---

### Volatility Mapping
Each session exhibits distinct volatility characteristics:
- **Low (Sydney)**
- **Moderate (Tokyo)**
- **High (London/New York)**

Understanding this helps align strategies with market conditions.

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

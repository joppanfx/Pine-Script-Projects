# 📕 JoppanFX | Forex Sessions

## 📸 Interface Visualization

![JoppanFX Forex Sessions](./Assets/JoppanFX%20%7C%20Forex%20Sessions.png)

---

## 📌 Overview

The **JoppanFX | Forex Sessions** indicator is a time-structured market framework designed to track **global liquidity cycles** across the four major trading sessions:

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

The indicator uses a **timezone-aware session engine** based on IANA timezone identifiers, allowing each session to automatically follow its respective local market clock and **adapt to Daylight Saving Time (DST) without manual UTC adjustments**.

---

## 🛠 Technical Logic

The indicator operates on a deterministic, timezone-aware session model with three primary components:

---

### 1. Timezone-Aware Session Membership Logic

Each bar is evaluated against the configured session window using the session's designated **IANA timezone**.

Instead of converting all sessions to a fixed UTC offset, the indicator interprets the current bar timestamp directly in the local timezone associated with each financial center.

Conceptually:

$$
h = \text{Hour}(\text{timestamp}, \text{session timezone})
$$

The session membership logic is then:

$$
\text{Session Active} =
\begin{cases}
h \in [S,E), & \text{if } S < E \\
h \geq S \lor h < E, & \text{if session crosses midnight}
\end{cases}
$$

Where:

- $h$ = current local hour in the session's timezone
- $S$ = session start hour
- $E$ = session end hour

This means the user defines the session according to its **local market time**, while TradingView handles the timezone conversion internally.

> ⚠️ **Timeframe Note:** On sub-hourly timeframes, the first bar of a session may not align exactly with the defined start hour depending on the chart's bar boundaries. This is a known limitation of bar-based session detection.

---

### 2. IANA Timezone Mapping

Each major trading session is permanently associated with its corresponding IANA timezone:

| Session | IANA Timezone |
|--------|---------------|
| **Sydney** | `Australia/Sydney` |
| **Tokyo** | `Asia/Tokyo` |
| **London** | `Europe/London` |
| **New York** | `America/New_York` |

IANA timezone identifiers represent regional timezone rules rather than fixed UTC offsets.

This allows the indicator to correctly account for regional timezone changes, including DST transitions.

For example, London is represented by:

```text
Europe/London
```

rather than:

```text
UTC+0
```

During British Summer Time, TradingView interprets the same timestamp using London's UTC+1 offset. During standard time, it uses UTC+0.

No manual offset calculation is required.

---

### 3. Automatic Daylight Saving Time Handling

The indicator does **not** maintain separate summer and winter session configurations.

Instead, each session is evaluated using its corresponding IANA timezone.

For example:

```text
London Local Session
08:00 → 17:00
```

remains:

```text
08:00 → 17:00
```

throughout the year.

The underlying UTC representation automatically changes when the `Europe/London` timezone transitions between standard time and daylight time.

The same principle applies independently to:

- `Australia/Sydney`
- `Europe/London`
- `America/New_York`

Tokyo uses `Asia/Tokyo` and does not observe DST.

This eliminates the need for users to manually modify session times during DST transitions.

---

### 4. Session Transition Detection

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

The timezone-aware session state is calculated first, meaning all downstream session events automatically inherit the correct DST-aware timing.

---

### 5. Dynamic Range Tracking

During an active session, the indicator continuously updates the session range:

$$
\text{Session High} = \max(\text{High}_t,\text{Session High})
$$

$$
\text{Session Low} = \min(\text{Low}_t,\text{Session Low})
$$

This allows real-time construction of:

- Session boxes
- High/Low levels
- Volatility envelopes

---

## 🧩 Session Classification

The table below reflects the **default local session configuration**.

Session times are defined according to the local clock of each respective financial center rather than UTC.

| Session | Default Local Time | IANA Timezone | Default Color | Market Context |
|--------|--------------------|----------------|---------------|----------------|
| **Sydney** | 08:00 → 17:00 | `Australia/Sydney` | Pink | Low liquidity / accumulation phase |
| **Tokyo** | 09:00 → 18:00 | `Asia/Tokyo` | Violet | Asian range formation |
| **London** | 08:00 → 17:00 | `Europe/London` | Yellow | High volatility / expansion |
| **New York** | 08:00 → 17:00 | `America/New_York` | Blue | Continuation or reversal phase |

> **Note:** The displayed session hours are local hours for each respective financial center. Their corresponding UTC times may change when the region enters or exits Daylight Saving Time.

---

## 🔒 Production Safeguards

This indicator is engineered with robust safeguards to ensure consistent behavior across different chart environments.

---

### Timezone-Aware Session Standardization

Each session is evaluated using its dedicated IANA timezone rather than relying on the TradingView chart timezone.

This prevents session calculations from being affected by:

- Broker server time differences
- Chart timezone settings
- User location
- Manual UTC offset changes

The chart can be displayed in any supported timezone while the session engine continues to evaluate each market according to its own local timezone.

---

### Automatic DST Adaptation

Daylight Saving Time is handled automatically through the respective IANA timezone definitions.

Users do **not** need to manually switch between summer and winter UTC offsets.

For example, London remains configured as:

```text
08:00 → 17:00
```

in `Europe/London`.

The underlying UTC representation changes automatically when the timezone transitions between standard time and daylight time.

---

### Independent Session Timezones

Each session operates independently using its own timezone:

```text
Sydney      → Australia/Sydney
Tokyo       → Asia/Tokyo
London      → Europe/London
New York    → America/New_York
```

This is important because different regions can have different DST rules and transition dates.

The indicator does not assume that all sessions share a common DST schedule.

---

### Midnight Session Handling

Sessions that span across midnight are handled using a dual-condition logic (`OR` condition), ensuring:

- Seamless continuity across day boundaries
- No session fragmentation

This is particularly relevant to sessions such as Sydney when configured with a session window that crosses midnight in its local timezone.

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

This ensures consistent watermark readability across different TradingView chart themes.

---

## 🚀 Key Features

- **Timezone-Aware Session Engine**  
  Each session follows its own financial-center timezone rather than relying on fixed UTC offsets.

- **Automatic DST Adaptation**  
  Session timings automatically adjust to regional Daylight Saving Time changes without manual intervention.

- **Multi-Session Market Structure**  
  Clearly segments the four major global trading sessions for time-based analysis.

- **Dynamic Session Boxes**  
  Real-time visualization of session range expansion and contraction.

- **Precision Start/End Markers**  
  Accurate labeling of session boundaries using state transitions.

- **Live High/Low Tracking**  
  Continuously updated session extremes with horizontal level lines.

- **Local-Time Session Configuration**  
  Session start and end hours are configured according to each market's local clock.

- **Chart-Timezone Independence**  
  Session detection remains consistent regardless of the timezone selected on the TradingView chart.

- **Cross-Market Compatibility**  
  Works seamlessly across forex, indices, crypto, and equities.

- **Adaptive UI Contrast**  
  Automatically adjusts the watermark based on chart theme luminance.

- **Quant-Grade Watermark**  
  Clean, professional branding for institutional chart presentation.

---

## ⚙️ Input Parameters

Each session exposes three configurable inputs, grouped by session in the indicator settings panel.

Unlike the previous UTC-based version, the start and end hours are now interpreted according to the **local timezone of the respective session**.

| Parameter | Description | Default |
|-----------|-------------|---------|
| **Sydney Start Hour (Local)** | Local hour the Sydney session opens | `8` |
| **Sydney End Hour (Local)** | Local hour the Sydney session closes | `17` |
| **Sydney Color** | Box, line, and label color for Sydney | Pink |
| **Tokyo Start Hour (Local)** | Local hour the Tokyo session opens | `9` |
| **Tokyo End Hour (Local)** | Local hour the Tokyo session closes | `18` |
| **Tokyo Color** | Box, line, and label color for Tokyo | Violet |
| **London Start Hour (Local)** | Local hour the London session opens | `8` |
| **London End Hour (Local)** | Local hour the London session closes | `17` |
| **London Color** | Box, line, and label color for London | Yellow |
| **New York Start Hour (Local)** | Local hour the New York session opens | `8` |
| **New York End Hour (Local)** | Local hour the New York session closes | `17` |
| **New York Color** | Box, line, and label color for New York | Blue |

### Session Timezone Mapping

The timezone used for each session is internally defined as:

| Session | Timezone |
|---------|----------|
| **Sydney** | `Australia/Sydney` |
| **Tokyo** | `Asia/Tokyo` |
| **London** | `Europe/London` |
| **New York** | `America/New_York` |

> All session hours are interpreted as **local time within the corresponding session timezone**. DST adjustments are handled automatically by TradingView's timezone-aware timestamp system.

---

## 📈 How to Use

### Session Breakouts

Breakouts from **London or New York session ranges** often signal strong directional moves driven by increased market participation and liquidity.

---

### Liquidity Sweeps

Price frequently takes out **previous session highs/lows** before reversing.

The session high/low levels provided by this indicator can therefore be used as reference points when analyzing potential liquidity sweeps.

---

### Session Range Trading

The **Tokyo session** often forms consolidation ranges that can act as:

- Accumulation zones
- Pre-breakout structures for London
- Reference ranges for subsequent session analysis

---

### Volatility Mapping

Each session exhibits distinct volatility characteristics:

- **Sydney** — Lower liquidity
- **Tokyo** — Moderate liquidity and range formation
- **London** — Higher volatility and expansion
- **New York** — High volatility and continuation/reversal potential

Understanding these differences helps align strategies with changing market conditions.

---

### DST-Aware Session Analysis

Because session times are tied to their respective financial-center timezones, traders can use the indicator throughout the year without manually adjusting session inputs when DST transitions occur.

For example, a London session configured as:

```text
08:00 → 17:00
```

continues to represent **08:00–17:00 London local time** regardless of whether London is observing standard time or British Summer Time.

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

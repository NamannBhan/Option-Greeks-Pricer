# Black-Scholes Options Pricer & Greek Visualizer

An interactive derivative pricing dashboard built with Python and Streamlit. Input any market assumptions — spot price, strike, volatility, time to expiry, risk-free rate — and instantly visualize how the option price and all five Greeks respond.

**Live:** https://option-greeks-pricer-pftespyynbzwqmhm4jz2hp.streamlit.app
)

---

## What It Does

Adjusts six parameters via sidebar sliders and instantly outputs:

- **Call and Put prices** from the closed-form BSM solution
- **Live Greek values** — Delta, Gamma, Theta, Vega, Rho — for the current inputs
- **Six sensitivity charts** plotting each Greek across the full spot price range, showing how they behave as the option moves from deep OTM to deep ITM

---

## The Model

Implements the Black-Scholes-Merton closed-form solution for European options:

$$C = S \cdot N(d_1) - K e^{-rT} N(d_2) \qquad P = K e^{-rT} N(-d_2) - S \cdot N(-d_1)$$

$$d_1 = \frac{\ln(S/K) + (r + \sigma^2/2)T}{\sigma\sqrt{T}} \qquad d_2 = d_1 - \sigma\sqrt{T}$$

All Greeks are derived analytically from this formula using the standard partial derivatives.

---

## Greeks Implemented

| Greek | Symbol | Formula | What It Shows |
|---|---|---|---|
| Delta | Δ | N(d₁) for calls, −N(−d₁) for puts | Hedge ratio — option's sensitivity to a $1 move in the underlying |
| Gamma | Γ | N'(d₁) / (Sσ√T) | Rate of Delta change — peaks ATM, drives hedging cost |
| Theta | Θ | Derived / 365 | Daily time decay — how much value the option loses per day |
| Vega | ν | S√T · N'(d₁) · 0.01 | Sensitivity to 1% change in implied volatility |
| Rho | ρ | 0.01 · KT · e^(−rT) · N(d₂) | Sensitivity to 1% change in risk-free rate |

---

## Parameters

| Input | Description |
|---|---|
| Underlying Price (S) | Current spot price of the asset |
| Strike Price (K) | Exercise price of the option |
| Risk-Free Rate (r) | Annualized continuously compounded rate |
| Days to Expiry | Converted internally to T = days/365 |
| Volatility (σ) | Annualized implied volatility |
| Option Type | Call or Put |

---

## Stack

```
Python · NumPy · SciPy (norm.cdf / norm.pdf) · Matplotlib · Seaborn · Streamlit
```

---

## Run Locally

```bash
git clone https://github.com/NamannBhan/bsm-options-pricer
cd bsm-options-pricer
pip install -r requirements.txt
streamlit run app.py
```

**requirements.txt**
```
streamlit
numpy
scipy
matplotlib
seaborn
pandas
```

---

## Key Observations the Visualizations Reveal

**Delta** follows an S-curve from 0 → 1 (calls) as spot rises through the strike. The steepest region is ATM — this is where Gamma is highest and a delta-neutral hedge requires the most frequent rebalancing.

**Gamma** peaks sharply at-the-money and collapses in both directions. This is the convexity of the position — long options benefit from large moves (positive Gamma), short options are hurt by them.

**Theta** is negative for long options and accelerates toward expiry — options decay slowly when far out, then bleed rapidly in the final weeks. The curve is non-linear, not a straight line.

**Vega** is always positive for long options — any increase in implied volatility increases option value regardless of direction. Highest ATM and for longer-dated contracts.

**Rho** diverges between calls and puts — calls gain value as rates rise (higher cost of carry makes the right to buy more valuable), puts lose value.

---

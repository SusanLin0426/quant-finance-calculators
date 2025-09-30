# Quant Finance Calculators

A toolkit of Python programs for **financial engineering and quantitative finance**.  
This repository starts with a **Broken-Wing Butterfly (Trapezoid) Option Pricer**, implemented with both **closed-form Black–Scholes** and **Monte Carlo simulation**. More financial calculators (bonds, Greeks, portfolio metrics) will be added over time.

---

## What’s inside now

### Module 01 — Broken-Wing Butterfly (Trapezoid) Call

#### Payoff Definition
For maturity \(S_T\):


$$
\text{payoff}(S_T)=
\begin{cases}
0 & S_T \le K_1\\
S_T-K_1 & K_1 < S_T \le K_2\\
K_2-K_1 & K_2 < S_T \le K_3\\
\frac{K_2-K_1}{K_4-K_3}\(K_4-S_T) & K_3 < S_T \le K_4\\
0 & S_T > K_4~
\end{cases}
$$



#### Features
- **Closed-form Black–Scholes pricing** with dividend yield \(q\)  
- **Monte Carlo simulation** with configurable runs/batches  
- **95% confidence interval** for simulated prices  
- Parameters: `S0, r, q, sigma, T, K1, K2, K3, K4`

---



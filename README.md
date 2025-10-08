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


### Module 02 — Option Pricing: Black–Scholes, Monte Carlo, and CRR Binomial
- **Black–Scholes–Merton (with continuous dividend yield q)**
- **Monte Carlo simulation (risk-neutral, GBM)**
- **Cox–Ross–Rubinstein (CRR) binomial tree** for both **European** and **American** calls/puts
  
> Default parameters in the script: `S0=50, K=50, r=0.10, q=0.05, sigma=0.40, T=0.5`

## Features

- **Black–Scholes (European)** call/put with dividend yield \( q \)  

d1 = [ ln(S0 / K) + (r - q + 0.5 * sigma^2) * T ] / (sigma * sqrt(T))

d2 = d1 - sigma * sqrt(T)

Call Price (C) = S0 * exp(-q * T) * N(d1) - K * exp(-r * T) * N(d2)

Put Price (P) = K * exp(-r * T) * N(-d2) - S0 * exp(-q * T) * N(-d1)


where:
- `N(x)` is the cumulative distribution function (CDF) of the standard normal distribution
- `exp()` is the exponential function
- `ln()` is the natural logarithm
  

- **Monte Carlo (risk-neutral GBM)**

Generate random draws Z ~ N(0, 1)

Simulate stock price at maturity:
ST = S0 * exp( (r - q - 0.5 * sigma^2) * T + sigma * sqrt(T) * Z )

Compute discounted payoff:
For Call: C = exp(-r * T) * max(ST - K, 0)
For Put: P = exp(-r * T) * max(K - ST, 0)

Repeat simulation many times, take the mean payoff as the option price.

- **CRR Binomial Tree**
  - \( u=e^{\sigma\sqrt{\Delta t}},\ d=1/u,\ p=\frac{e^{(r-q)\Delta t}-d}{u-d} \)
  - Backward induction for **European** and **American** (early exercise) options
  - Adjustable steps `n` (e.g., 100 / 500)
  
dt = T / n
u = exp( sigma * sqrt(dt) )
d = 1 / u
p = ( exp((r - q) * dt) - d ) / (u - d)
df = exp(-r * dt)


Backward induction steps:

Compute terminal stock prices:
S[j, i] = S0 * (u^(i-j)) * (d^j)

Compute terminal option values:
For Call: v[j, i] = max(S[j, i] - K, 0)
For Put: v[j, i] = max(K - S[j, i], 0)

Work backward:
If European:
pv[j, i] = df * (p * pv[j, i+1] + (1 - p) * pv[j+1, i+1])
If American:
pv[j, i] = max(df * (p * pv[j, i+1] + (1 - p) * pv[j+1, i+1]), v[j, i])

Final price = `pv[0, 0]`



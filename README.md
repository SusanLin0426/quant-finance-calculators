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

This module implements three classical option pricing models:

- **Black–Scholes–Merton (with continuous dividend yield \(q\))**
- **Monte Carlo Simulation** under the **risk-neutral GBM**
- **Cox–Ross–Rubinstein (CRR) Binomial Tree** for both **European** and **American** options  

> Default parameters: `S0=50, K=50, r=0.10, q=0.05, sigma=0.40, T=0.5`

---

## Features

### **Black–Scholes (European) Pricing**

Closed-form pricing of a European call/put with dividend yield \(q\):


$$
d_1=\frac{\ln\!\left(\frac{S_0}{K}\right)+(r-q+0.5\sigma^2)T}{\sigma\sqrt{T}},\quad
d_2=d_1-\sigma\sqrt{T}
$$

**Call**

$$
C=S_0 e^{-qT}N(d_1)-K e^{-rT}N(d_2)
$$

**Put**

$$
P=K e^{-rT}N(-d_2)-S_0 e^{-qT}N(-d_1)
$$

Where \(N(\cdot)\) is the standard normal CDF.

---


Where:  
-  N(x): cumulative distribution function (CDF) of the standard normal distribution
- Where \(N(\cdot)\) is the CDF of the standard normal distribution

---

### **2️⃣ Monte Carlo Simulation (Risk-Neutral GBM)**

Simulate asset paths under the risk-neutral measure:

1. Generate random draws \( Z \sim N(0,1) \)
2. Simulate stock price at maturity:  
   \( S_T = S_0 e^{(r - q - 0.5\sigma^2)T + \sigma\sqrt{T}Z} \)
3. Compute discounted payoffs:  
   - Call: \( C = e^{-rT}\max(S_T - K, 0) \)  
   - Put:  \( P = e^{-rT}\max(K - S_T, 0) \)
4. Repeat many times and take the **mean payoff** as the option price.

---

### **3️⃣ CRR Binomial Tree**

Cox–Ross–Rubinstein discrete-time tree model for both **European** and **American** options.

#### Parameters
$$
u = e^{\sigma\sqrt{\Delta t}}, \quad
d = \frac{1}{u}, \quad
p = \frac{e^{(r-q)\Delta t} - d}{u - d}, \quad
\text{df} = e^{-r\Delta t}
$$

Where \(\Delta t = T / n\) and `n` is the number of steps (e.g., 100, 500).

#### Backward Induction
1. Compute terminal stock prices:  
   \( S[j,i] = S_0 \cdot u^{(i-j)} \cdot d^j \)

2. Compute terminal option payoffs:  
   - Call: \( v[j,i] = \max(S[j,i] - K, 0) \)  
   - Put:  \( v[j,i] = \max(K - S[j,i], 0) \)

3. Work backward through the tree:  
   - **European:**  
     \( pv[j,i] = \text{df} \cdot (p \cdot pv[j,i+1] + (1-p) \cdot pv[j+1,i+1]) \)  
   - **American:**  
     \( pv[j,i] = \max(\text{df} \cdot (p \cdot pv[j,i+1] + (1-p) \cdot pv[j+1,i+1]), v[j,i]) \)

4. The **final option price** is the root node:






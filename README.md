# Quant Finance Calculators

A growing toolkit of reusable **quantitative finance calculators** in Python.

## What’s inside now

### Module 01 — Broken-Wing Butterfly (Trapezoid) Call Pricer
Prices a piecewise-linear **broken-wing butterfly / trapezoid** payoff under **Black–Scholes** with dividend yield `q`, and validates it by **Monte Carlo** with a 95% confidence interval.

- Parameters: `S0, r, q, sigma, T, K1, K2, K3, K4`
- Outputs: closed-form price `C`, MC mean, stdev, 95% CI


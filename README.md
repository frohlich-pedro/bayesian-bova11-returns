# Bayesian Inference of BOVA11 Returns via MCMC

A Bayesian statistical approach to parameter estimation for the daily log-returns of **BOVA11** (an ETF tracking the Brazilian iBovespa index). 

Using Markov Chain Monte Carlo (MCMC) sampling via the No-U-Turn Sampler (NUTS), this project fits a **Student's t-distribution** to financial return data to capture heavy tails (*kurtosis*) and model market volatility with uncertainty bounds.

---

## Model Formulation

Standard Gaussian models often understate tail risk in financial time series. We model the daily log-returns $y_t = \log(P_t / P_{t-1})$ using a Student's t-distribution:

$$y_t \sim \text{Student-t}(\nu, \mu, \sigma)$$

### Priors
* **Location ($\mu$):** $\mu \sim \mathcal{N}(0, 0.05)$ — Daily expected return (drift).
* **Scale ($\sigma$):** $\sigma \sim \text{HalfNormal}(0.05)$ — Daily return dispersion (volatility).
* **Degrees of Freedom ($\nu$):** $\nu \sim \text{Exponential}(\lambda = 0.1)$ — Controls tail heaviness. $\nu \to \infty$ converges to a standard Gaussian, while $\nu < 10$ indicates heavy tails.

---

## Key Findings

Posterior parameter distributions estimated from historical BOVA11 daily returns:

* **$\mu$ (Daily Drift):** Concentrated around $0.0005$–$0.0006$. The posterior spans both negative and positive values, confirming no statistically significant positive daily drift.
* **$\sigma$ (Daily Volatility):** Well-defined around $0.0095$ ($\approx 0.95\%$ daily volatility, corresponding to an annualized volatility of $\approx 15\%$).
* **$\nu$ (Tail Weight):** Concentrated between $5$ and $10$. This low value of $\nu$ confirms strong empirical evidence of heavy tails (*fat tails*) in the Brazilian ETF market, demonstrating that Gaussian-based models underestimate extreme market shocks.

---

## Dependencies

* Python 3.x
* `pymc`
* `yfinance`
* `numpy`
* `matplotlib`

Install required packages:
```bash
pip install pymc yfinance numpy matplotlib

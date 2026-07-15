# Portfolio Optimization & Asset Allocation: Replicating ARK Invest's "Big Ideas"

## Project Overview
This project evaluates the macroeconomic asset allocation thesis presented by ARK Invest in their 2021 *Big Ideas* report, which asserted that a 1% to 5% structural allocation to Bitcoin significantly enhances an institutional portfolio's risk-adjusted efficiency[cite: 1]. To stress-test this empirical claim under real market regimes, I built a Modern Portfolio Theory (MPT) framework evaluating historical daily log returns across a multi-asset universe from 2018 to 2023[cite: 1].

## Asset Universe & Proxy Selection
The simulated portfolio evaluates 7 distinct, highly liquid institutional asset classes[cite: 1]:
* **U.S. Equities (`SPY`)** | **Investment-Grade Bonds (`AGG`)**[cite: 1]
* **Real Estate (`VNQ`)** | **Commodities (`DJP`)**[cite: 1]
* **Currencies (`UUP`)** | **Gold (`GLD`)**[cite: 1]
* **Alternative Cryptocurrencies (`BTC-USD`)**[cite: 1]

## Optimization Methodology
The framework implements two optimization paths to map the Efficient Frontier[cite: 1]:
1. **Monte Carlo Simulation:** Iterating across 50,000 randomly drawn Dirichlet-distributed allocation paths ($w_i \ge 0, \sum w_i = 1$) to map out portfolio risk-return geometries[cite: 1].
2. **Numerical Optimization:** Utilizing Sequential Least Squares Programming (`scipy.optimize.minimize`) to compute closed-form analytical allocations for the true tangency portfolio and the global minimum variance portfolio[cite: 1].

## Quantitative Results & Data Output

### 1. Asset Performance Summary (2018–2023)
* **SPY:** Annualized Return: **7.72%** | Annualized Volatility: **16.99%** | Sharpe Ratio: **0.2191**[cite: 1]
* **BTC:** Annualized Return: **3.41%** | Annualized Volatility: **20.00%** | Sharpe Ratio: **-0.0295**[cite: 1]
* **GLD:** Annualized Return: **2.59%** | Annualized Volatility: **5.90%** | Sharpe Ratio: **-0.2383**[cite: 1]

### 2. Optimal Long-Only Portfolio Allocations
* **Max Sharpe (Tangency) Portfolio:** Equities (`SPY`: **68.51%**), Fixed Income (`AGG`: **23.66%**), Commodities (`DJP`: **7.83%**), Bitcoin (`BTC`: **0.00%**)[cite: 1].
  * *Metrics:* Expected Return: **7.38%** | Portfolio Volatility: **14.25%** | Sharpe Ratio: **0.2371**[cite: 1]
* **Global Minimum Variance Portfolio:** Gold (`GLD`: **48.26%**), Real Estate (`VNQ`: **35.84%**), Bonds (`AGG`: **10.05%**), Currencies (`UUP`: **3.48%**), Equities (`SPY`: **2.37%**)[cite: 1].
  * *Metrics:* Expected Return: **2.24%** | Portfolio Volatility: **2.91%** | Sharpe Ratio: **-0.6045**[cite: 1]

---

### **Bitcoin's Role in the Optimal Portfolio**

In my analysis, Bitcoin received a **0% weight** in the long-only tangency portfolio, a result that remained perfectly consistent across every sensitivity test, risk-free rate assumption, and sub-sample period[cite: 1]. I found that two primary drivers explain this outcome:

* **Subpar Risk-Adjusted Returns:** Bitcoin's annualized Sharpe ratio over the 2018–2023 period came in at **-0.030**, which was the second worst in my asset universe, trailing only Gold (`GLD`) at -0.238[cite: 1]. The 2022 crypto bear market—during which Bitcoin plummeted by roughly 65%—was severe enough to completely erase all of its risk-adjusted gains from the 2020–2021 bull run[cite: 1].
* **High Equity Correlation:** Bitcoin's correlation with the S&P 500 (`SPY`) over this timeframe was **0.78**, which is significantly higher than market commentators commonly assume[cite: 1]. This pronounced co-movement with equities implies that Bitcoin provides almost no diversification benefit when a portfolio needs it most during risk-off episodes, drastically reducing its utility to the optimizer[cite: 1].

Even when I tested an unconstrained tangency model allowing short positions, the optimizer only allocated a minor 5.60% to Bitcoin[cite: 1]. That specific portfolio ultimately produced a deeply negative Sharpe ratio of -0.95 due to aggressive short bets against corporate bonds (`AGG`) and equities (`SPY`)[cite: 1]. Ultimately, my long-only optimizer correctly rejected Bitcoin entirely, prioritizing massive weights in `SPY` (68.5%) and `AGG` (23.7%) instead[cite: 1].

### **Big Ideas Context**

ARK Invest's 2021 *Big Ideas* report famously argued that establishing a 1% to 5% structural allocation to Bitcoin enhances a diversified portfolio's risk-return profile due to low asset correlation and asymmetric upside[cite: 1]. 

However, my empirical results **directly challenge this narrative** under real-world 2018–2023 macro conditions[cite: 1]:

1. **The asset did not offer low correlation:** Bitcoin's correlation with `SPY` was 0.78, rather than the near-zero baseline implied by ARK[cite: 1]. It effectively behaved like a high-beta risk asset rather than a true diversifier[cite: 1].
2. **The risk-adjusted value premium was negative:** A negative Sharpe ratio proves that Bitcoin destroyed risk-adjusted portfolio value over the full cycle[cite: 1]. No standard optimizer would rationally allocate capital to a negative-Sharpe asset when significantly more efficient alternatives are available[cite: 1].
3. **The results are independent of the selected period:** The 0% allocation to Bitcoin persisted even when I isolated the Pre-2021 bull-market sub-sample, indicating that traditional equities and fixed income dominated the optimizer even during Bitcoin's stronger historical windows[cite: 1].

In my view, ARK’s optimistic conclusion was largely a product of survivorship and timing bias, as their report was published in early 2021 near the local peak of the crypto bull market[cite: 1]. Had their research accounted for the full 2018–2023 dataset, it is highly improbable that they would have arrived at the same institutional asset allocation recommendation[cite: 1]. The essential takeaway from my model is that for Bitcoin to legitimately improve frontier efficiency, it requires a simultaneous regime of high Sharpe performance and decoupled equity correlation—conditions that flatly failed to materialize in my sample[cite: 1].

### **Practical Concerns**

Beyond the pure mathematical optimization, I identified several structural frictions that make a real-world institutional allocation to Bitcoin highly impractical:

1. **Rebalancing Friction:** Bitcoin’s massive annualized volatility triggers rapid drift in portfolio weights[cite: 1]. Maintaining a target allocation requires frequent rebalancing, which is theoretically elegant but practically expensive[cite: 1].
2. **High Transaction Drag:** Crypto spot markets still carry steep costs, ranging from 0.1% to 0.5% per trade, with bid-ask spreads widening significantly during market panic[cite: 1]. On a net-of-fee basis, Bitcoin’s risk-adjusted returns become even less attractive[cite: 1].
3. **Tax Complications:** Under IRS guidelines, Bitcoin is classified as property, meaning that every single rebalancing trade acts as a taxable capital gains realization event[cite: 1]. This lacks the structural advantage of traditional ETFs (which use tax-free, in-kind creation and redemption mechanisms) and introduces a massive cash drag on the portfolio[cite: 1].
4. **Investability Limits:** Throughout the bulk of my 2018–2023 sample, institutional access to Bitcoin was deeply constrained[cite: 1]. Managers had to rely on futures-based vehicles like `BITO` (which lose 5% to 10% annually to roll costs) or navigate highly complex direct custody frameworks[cite: 1]. While the January 2024 spot ETF approvals (like BlackRock's `IBIT`) have mitigated this friction, it represents a structural shift that occurred completely outside our historical optimization window[cite: 1].
5. **Extreme Estimation Error:** Mean-variance optimizers are highly sensitive to forward-looking return inputs[cite: 1]. Because Bitcoin has a brief operating history and undergoes frequent structural shifts, estimating its expected return purely from historical data is incredibly unreliable[cite: 1]. Moving forward, an allocation framework like a Black-Litterman model—where an investor can inject explicit macroeconomic views rather than relying strictly on backward-looking data—would be much more robust[cite: 1].

## Tech Stack
* **Language:** Python
* **Optimization & Math:** `scipy.optimize` (Sequential Least Squares Programming), `numpy` (Dirichlet sampling)[cite: 1]
* **Data Retrieval:** `yfinance`[cite: 1]
* **Data Processing:** `pandas`[cite: 1]
* **Visualization:** `matplotlib`[cite: 1]

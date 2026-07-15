# Portfolio Optimization & Asset Allocation: Replicating ARK Invest's "Big Ideas"

## Project Overview
This project evaluates the macroeconomic asset allocation thesis presented by ARK Invest in their 2021 *Big Ideas* report, which asserted that a 1% to 5% structural allocation to Bitcoin significantly enhances an institutional portfolio's risk-adjusted efficiency. To stress-test this empirical claim under real market regimes, I built a Modern Portfolio Theory (MPT) framework evaluating historical daily log returns across a multi-asset universe from 2018 to 2023.

## Asset Universe & Proxy Selection
The simulated portfolio evaluates 7 distinct, highly liquid institutional asset classes:
* **U.S. Equities (`SPY`)** | **Investment-Grade Bonds (`AGG`)**
* **Real Estate (`VNQ`)** | **Commodities (`DJP`)**
* **Currencies (`UUP`)** | **Gold (`GLD`)**
* **Alternative Cryptocurrencies (`BTC-USD`)**

## Optimization Methodology
The framework implements two optimization paths to map the Efficient Frontier:
1. **Monte Carlo Simulation:** Iterating across 50,000 randomly drawn Dirichlet-distributed allocation paths ($w_i \ge 0, \sum w_i = 1$) to map out portfolio risk-return geometries.
2. **Numerical Optimization:** Utilizing Sequential Least Squares Programming (`scipy.optimize.minimize`) to compute closed-form analytical allocations for the true tangency portfolio and the global minimum variance portfolio.

## Quantitative Results & Data Output

### 1. Asset Performance Summary (2018–2023)
* **SPY:** Annualized Return: **7.72%** | Annualized Volatility: **16.99%** | Sharpe Ratio: **0.2191**
* **BTC:** Annualized Return: **3.41%** | Annualized Volatility: **20.00%** | Sharpe Ratio: **-0.0295**
* **GLD:** Annualized Return: **2.59%** | Annualized Volatility: **5.90%** | Sharpe Ratio: **-0.2383**

### 2. Optimal Long-Only Portfolio Allocations
* **Max Sharpe (Tangency) Portfolio:** Equities (`SPY`: **68.51%**), Fixed Income (`AGG`: **23.66%**), Commodities (`DJP`: **7.83%**), Bitcoin (`BTC`: **0.00%**).
  * *Metrics:* Expected Return: **7.38%** | Portfolio Volatility: **14.25%** | Sharpe Ratio: **0.2371**
* **Global Minimum Variance Portfolio:** Gold (`GLD`: **48.26%**), Real Estate (`VNQ`: **35.84%**), Bonds (`AGG`: **10.05%**), Currencies (`UUP`: **3.48%**), Equities (`SPY`: **2.37%**).
  * *Metrics:* Expected Return: **2.24%** | Portfolio Volatility: **2.91%** | Sharpe Ratio: **-0.6045**

---

### **Bitcoin's Role in the Optimal Portfolio**

In my analysis, Bitcoin received a **0% weight** in the long-only tangency portfolio, a result that remained perfectly consistent across every sensitivity test, risk-free rate assumption, and sub-sample period. I found that two primary drivers explain this outcome:

* **Subpar Risk-Adjusted Returns:** Bitcoin's annualized Sharpe ratio over the 2018–2023 period came in at **-0.030**, which was the second worst in my asset universe, trailing only Gold (`GLD`) at -0.238. The 2022 crypto bear market—during which Bitcoin plummeted by roughly 65%—was severe enough to completely erase all of its risk-adjusted gains from the 2020–2021 bull run.
* **High Equity Correlation:** Bitcoin's correlation with the S&P 500 (`SPY`) over this timeframe was **0.78**, which is significantly higher than market commentators commonly assume. This pronounced co-movement with equities implies that Bitcoin provides almost no diversification benefit when a portfolio needs it most during risk-off episodes, drastically reducing its utility to the optimizer.

Even when I tested an unconstrained tangency model allowing short positions, the optimizer only allocated a minor 5.60% to Bitcoin. That specific portfolio ultimately produced a deeply negative Sharpe ratio of -0.95 due to aggressive short bets against corporate bonds (`AGG`) and equities (`SPY`). Ultimately, my long-only optimizer correctly rejected Bitcoin entirely, prioritizing massive weights in `SPY` (68.5%) and `AGG` (23.7%) instead.

### **Big Ideas Context**

ARK Invest's 2021 *Big Ideas* report famously argued that establishing a 1% to 5% structural allocation to Bitcoin enhances a diversified portfolio's risk-return profile due to low asset correlation and asymmetric upside. 

However, my empirical results **directly challenge this narrative** under real-world 2018–2023 macro conditions:

1. **The asset did not offer low correlation:** Bitcoin's correlation with `SPY` was 0.78, rather than the near-zero baseline implied by ARK. It effectively behaved like a high-beta risk asset rather than a true diversifier.
2. **The risk-adjusted value premium was negative:** A negative Sharpe ratio proves that Bitcoin destroyed risk-adjusted portfolio value over the full cycle. No standard optimizer would rationally allocate capital to a negative-Sharpe asset when significantly more efficient alternatives are available.
3. **The results are independent of the selected period:** The 0% allocation to Bitcoin persisted even when I isolated the Pre-2021 bull-market sub-sample, indicating that traditional equities and fixed income dominated the optimizer even during Bitcoin's stronger historical windows.

In my view, ARK’s optimistic conclusion was largely a product of survivorship and timing bias, as their report was published in early 2021 near the local peak of the crypto bull market. Had their research accounted for the full 2018–2023 dataset, it is highly improbable that they would have arrived at the same institutional asset allocation recommendation. The essential takeaway from my model is that for Bitcoin to legitimately improve frontier efficiency, it requires a simultaneous regime of high Sharpe performance and decoupled equity correlation—conditions that flatly failed to materialize in my sample.

### **Practical Concerns**

Beyond the pure mathematical optimization, I identified several structural frictions that make a real-world institutional allocation to Bitcoin highly impractical:

1. **Rebalancing Friction:** Bitcoin’s massive annualized volatility triggers rapid drift in portfolio weights. Maintaining a target allocation requires frequent rebalancing, which is theoretically elegant but practically expensive.
2. **High Transaction Drag:** Crypto spot markets still carry steep costs, ranging from 0.1% to 0.5% per trade, with bid-ask spreads widening significantly during market panic. On a net-of-fee basis, Bitcoin’s risk-adjusted returns become even less attractive.
3. **Tax Complications:** Under IRS guidelines, Bitcoin is classified as property, meaning that every single rebalancing trade acts as a taxable capital gains realization event. This lacks the structural advantage of traditional ETFs (which use tax-free, in-kind creation and redemption mechanisms) and introduces a massive cash drag on the portfolio.
4. **Investability Limits:** Throughout the bulk of my 2018–2023 sample, institutional access to Bitcoin was deeply constrained. Managers had to rely on futures-based vehicles like `BITO` (which lose 5% to 10% annually to roll costs) or navigate highly complex direct custody frameworks. While the January 2024 spot ETF approvals (like BlackRock's `IBIT`) have mitigated this friction, it represents a structural shift that occurred completely outside our historical optimization window.
5. **Extreme Estimation Error:** Mean-variance optimizers are highly sensitive to forward-looking return inputs. Because Bitcoin has a brief operating history and undergoes frequent structural shifts, estimating its expected return purely from historical data is incredibly unreliable. Moving forward, an allocation framework like a Black-Litterman model—where an investor can inject explicit macroeconomic views rather than relying strictly on backward-looking data—would be much more robust.

## Tech Stack
* **Language:** Python
* **Optimization & Math:** `scipy.optimize` (Sequential Least Squares Programming), `numpy` (Dirichlet sampling)
* **Data Retrieval:** `yfinance`
* **Data Processing:** `pandas`
* **Visualization:** `matplotlib`

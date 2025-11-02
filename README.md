# **Black Gold Arbitrage: A Statistical Arbitrage Backtest**
This repository contains a complete Python backtest for a statistical arbitrage (stat-arb) pairs trading strategy. The project identifies and exploits the temporary price-relationship breakdowns between two highly correlated energy assets:

USO: The United States Oil Fund (tracks the price of crude oil).
XLE: The Energy Select Sector SPDR Fund (a basket of energy stocks like Exxon, Chevron, etc.).

## **The Strategy: "The Crude vs. The Suits"**

Why this pair? The price of raw crude oil (USO) and the stock prices of the companies that drill, refine, and sell it (XLE) are fundamentally linked. When oil is expensive, energy companies should be more profitable, and their stocks should rise.
But they don't always move in perfect sync.
This strategy is built on the hypothesis that while USO and XLE are cointegrated (they share a long-term "leash"), they can temporarily diverge due to short-term market noise (e.g., earnings reports, inventory news, geopolitical events).
This project aims to profit from these small, temporary splits by betting that their relationship will always "revert to the mean."

## **Methodology**
The backtest is built using Python (Pandas, Statsmodels, yfinance) and follows these steps:

Cointegration Test: The Engle-Granger test is used to confirm a long-term statistical relationship between the two ETFs.
Spread & Hedge Ratio: A rolling OLS regression is used to calculate the dynamic hedge ratio, creating a stationary "spread" series.
Signal Generation: A rolling Z-Score is calculated on the spread. A signal is generated when the spread moves more than 2 standard deviations from its mean, signaling a statistically significant divergence.

## **Trading Rules:**

Short Signal (Z-Score > 1.0): The spread is "too expensive." We short the pair (Short XLE, Long USO).
Long Signal (Z-Score < -1.0): The spread is "too cheap." We long the pair (Long XLE, Short USO).

Exit Signal (Z-Score reverts to 0.0): The price relationship has returned to its average. We close the position to lock in the profit.

## **Performance & Key Metrics**
This strategy was backtested on an out-of-sample period to validate its performance.

Annualized Sharpe Ratio: 1.19
Maximum Drawdown (MDD): 8.18%
Profit Factor: 1.36

## **Result:**<img width="1384" height="1178" alt="BlackGoldPair Result" src="https://github.com/user-attachments/assets/d4733e1c-a0c8-472c-87c4-42d0a46c6020" />

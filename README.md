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

## **Limitations and Areas for Improvement**
While this backtest provides a solid proof-of-concept for the USO/XLE pair, it operates on several key simplifications. Before this could be considered a robust trading strategy, the following areas would need to be addressed:

**- Transaction Costs and Slippage:** The current P&L calculation is idealized and does not account for trading commissions, exchange fees, or the bid-ask spread. Mean-reversion strategies often involve frequent trades, and these costs would absolutely eat into the cumulative returns shown. Furthermore, the backtest assumes execution at the 'Close' price, whereas real-world orders would be filled at the 'Bid' or 'Ask', introducing slippage.

**- Lookahead Bias:** A subtle but critical flaw is present in the pair selection process. The USO/XLE pair was identified as cointegrated (p-value: 0.0010) using a test run over the entire data period, including the "out-of-sample" test period. This is a form of lookahead bias. A more rigorous approach would be a true walk-forward analysis:



1.   Formation Period: Run the cointegration screening only on an initial period (e.g., 2022-2023) to find promising pairs.
2.   Trading Period: Test the strategy only on the subsequent, completely unseen period (e.g., 2024-2025).


**- Parameter Risk:** The strategy's performance is sensitive to its parameters: the hedge ratio lookback , the Z-score lookback , and the entry/exit thresholds. These were set manually. A more advanced test would involve optimizing these parameters  on a validation set or, even better, making them dynamic such as adaptive thresholds based on rolling volatility.

**Strategy Robustness:** The current model is simple. It lacks a crucial risk management component: a stop-loss to prevent situations where the cointegrating relationship fundamentally breaks down. The Z-score could diverge well beyond -3.0 or +3.0, leading to significant losses. A real-world strategy must have a mechanism (e.g., a maximum loss per trade, or a secondary Z-score cutoff) to exit a trade when the mean-reversion assumption fails.

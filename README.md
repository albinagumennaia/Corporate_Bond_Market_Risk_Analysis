# Corporate Bond Market Risk Analysis

**Python | Value at Risk | Expected Shortfall | Monte Carlo Simulation | VaR Backtesting**

## Project Overview

This project examines market risk for a **hypothetical USD 20 million corporate bond portfolio** using publicly available U.S. Treasury and corporate bond yield data from **2005–2026**.

The analysis estimates one-day portfolio profit and loss (P&L), compares multiple Value at Risk (VaR) and Expected Shortfall (ES) methodologies, examines risk under historical financial crisis conditions, and evaluates model calibration through out-of-sample backtesting.

The primary objective is to understand how **distributional assumptions, market conditions, and historical calibration periods affect portfolio risk estimates**, rather than to identify a universally superior model.

## Project Objectives

- Construct interest-rate and credit-spread risk factors using historical market data.
- Estimate hypothetical corporate bond portfolio P&L using duration-based sensitivities.
- Compare historical simulation, normal parametric, Student's t parametric, and Monte Carlo VaR and Expected Shortfall.
- Examine stressed risk estimates during the 2008–2009 financial crisis.
- Evaluate VaR model calibration through out-of-sample backtesting.
- Identify modeling limitations and opportunities for further research.

## Data Sources

Historical market data was obtained from **Federal Reserve Economic Data (FRED)**.

| Series | Description |
|---|---|
| [DGS10](https://fred.stlouisfed.org/series/DGS10) | 10-Year Treasury Constant Maturity Rate |
| [DBAA](https://fred.stlouisfed.org/series/DBAA) | Moody's Seasoned Baa Corporate Bond Yield |

The analysis constructs daily Treasury yield changes and a credit-spread proxy calculated as the difference between corporate bond yields and Treasury yields.

**Note:** These series have different maturity characteristics, making the constructed credit spread an imperfect proxy for corporate credit risk.

## Portfolio Assumptions

The analysis uses the following illustrative portfolio parameters:

| Parameter | Assumption |
|---|---|
| Portfolio market value | USD 20,000,000 |
| Interest-rate duration | 4 |
| Credit spread duration | 4 |
| Risk horizon | One trading day |

Portfolio P&L is estimated using a first-order duration approximation:

$$
\Delta P \approx -P(D_r\Delta y+D_s\Delta s)
$$

Where:

- $P$ = Portfolio market value
- $D_r$ = Interest-rate duration
- $D_s$ = Credit spread duration
- $\Delta y$ = Treasury yield change in decimal form
- $\Delta s$ = Credit spread change in decimal form

This methodology estimates the portfolio's sensitivity to movements in benchmark interest rates and credit spreads.

## Risk Estimation Methodologies

### 1. Historical Simulation

Estimates VaR directly from historical portfolio P&L observations and calculates Expected Shortfall as the average loss within the worst 5% or 1% of outcomes.

### 2. Normal Parametric VaR and ES

Uses the historical mean and standard deviation of portfolio P&L, assuming a normal distribution.

### 3. Student's t Parametric VaR and ES

Fits a Student's t distribution to historical portfolio P&L to examine the impact of heavier tails on risk estimates.

### 4. Monte Carlo Simulation

Generates 10,000 hypothetical one-day market scenarios using a multivariate normal distribution calibrated to historical Treasury yield and credit spread changes.

The simulation incorporates the estimated means, volatilities, and covariance of the two risk factors.

### 5. Historical Crisis-Period Risk Analysis

Recalculates historical, normal parametric, and Student's t VaR and Expected Shortfall using observations from **September 2008–August 2009**.

This analysis examines the impact of crisis-period calibration on portfolio risk estimates. It is not intended to represent a comprehensive scenario-based stress-testing framework.

## Model Comparison

### Full Historical Period: 2005–2026

All figures represent approximate one-day risk estimates in USD.

| Method | 95% VaR | 99% VaR | 95% ES | 99% ES |
|---|---:|---:|---:|---:|
| Historical Simulation | 72,000 | 112,000 | 98,784 | 151,273 |
| Normal Parametric | 70,309 | 99,412 | 88,153 | 113,883 |
| Student's t Parametric | 66,439 | 108,713 | 93,571 | 140,706 |
| Normal Monte Carlo | 69,900 | 96,912 | 87,166 | 110,416 |

### Financial Crisis Period: September 2008–August 2009

| Method | 95% VaR | 99% VaR | 95% ES | 99% ES |
|---|---:|---:|---:|---:|
| Historical Stressed | 124,400 | 180,080 | 179,077 | 269,333 |
| Normal Parametric Stressed | 127,014 | 180,620 | 159,883 | 207,275 |
| Student's t Parametric Stressed | 121,613 | 192,510 | 166,401 | 240,420 |

### Key Findings

- Historical simulation generally produces higher extreme-tail risk estimates than the normal models.
- Normal parametric and normal Monte Carlo estimates are relatively close, consistent with their shared normality assumptions.
- Student's t produces a 99% Expected Shortfall estimate closer to the historical benchmark than the normal parametric model.
- Risk estimates calibrated to the 2008–2009 crisis period are substantially higher than their corresponding full-history estimates.
- The results demonstrate the importance of distributional assumptions and calibration periods in market risk measurement.

## VaR Backtesting and Model Validation

An out-of-sample VaR backtest was conducted to evaluate model calibration.

The dataset was divided into:

- **Training period:** 2005–2022 (4,500 observations)
- **Testing period:** 2023–2026 (945 observations)

VaR thresholds were estimated using only the training observations and compared with subsequent portfolio P&L outcomes.

A VaR exception occurs when a daily portfolio loss exceeds the estimated VaR threshold.

### Backtesting Results

| Model | Confidence | VaR (USD) | Expected Exceptions | Observed Exceptions | Exception Rate |
|---|---|---:|---:|---:|---:|
| Historical | 95% | 72,000 | 47.3 | 39 | 4.13% |
| Normal Parametric | 95% | 70,705 | 47.3 | 48 | 5.08% |
| Historical | 99% | 112,000 | 9.5 | 4 | 0.42% |
| Normal Parametric | 99% | 100,016 | 9.5 | 10 | 1.06% |

### Backtesting Interpretation

The normal parametric model produced exception frequencies closer to the expected 5% and 1% levels during the testing period.

Historical simulation appeared somewhat conservative, particularly at the 99% confidence level.

A comparison of training and testing distributions showed relatively similar overall P&L volatility, while the testing period exhibited somewhat less severe losses at the 99% empirical VaR threshold.

These findings do not establish that one model is universally superior. Exception frequencies alone do not fully assess loss severity, model stability, or performance under changing market conditions.

## Model Limitations

The project has several important limitations:

- **Hypothetical portfolio:** The analysis does not use actual corporate bond holdings or realized trading P&L.
- **Market data proxies:** Treasury and corporate bond yield series have different maturity characteristics and do not provide a maturity-matched credit spread.
- **Illustrative duration assumptions:** Interest-rate and credit spread durations are assumed rather than estimated from individual bond holdings.
- **Simplified P&L methodology:** The first-order duration approximation does not incorporate convexity, full bond repricing, coupon effects, or liquidity-related losses.
- **Distributional assumptions:** Normal models may underestimate skewness and heavy-tailed market movements.
- **Stress-period sample:** The 2008–2009 crisis analysis contains approximately 250 observations, limiting the reliability of extreme-tail estimates.
- **Backtesting methodology:** Fixed VaR thresholds are used rather than dynamically recalibrated models.

## Future Research

Potential extensions include:

- Analyzing actual bond holdings with security-level pricing and sensitivities.
- Using maturity-matched Treasury and corporate bond market data.
- Incorporating convexity and full bond repricing.
- Exploring Student's t Monte Carlo and historical bootstrap simulation.
- Applying GARCH models to capture time-varying volatility.
- Implementing rolling-window VaR calibration and more comprehensive backtesting methods.

## Technologies Used

- Python
- pandas
- NumPy
- SciPy
- Matplotlib
- Seaborn
- Google Colab
- FRED market data

## Running the Project

1. Open `Market_Risk_Analysis.ipynb` in Google Colab or Jupyter Notebook.
2. Run the notebook cells sequentially.
3. The notebook retrieves historical data from FRED, requiring an internet connection.
4. Review the resulting portfolio P&L analysis, risk estimates, visualizations, and backtesting results.

Results may vary slightly if historical data is updated or Monte Carlo simulation parameters are modified.

## Disclaimer

This is an independent educational project using publicly available market data and a hypothetical corporate bond portfolio.

The results are intended to demonstrate quantitative market risk modeling techniques and do not represent actual trading positions, investment recommendations, or regulatory risk measures.

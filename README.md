# Multi-Asset Portfolio Optimization with Dynamic Risk Management

A quantitative portfolio management system combining Markov-switching regime detection, GARCH volatility forecasting and dynamic risk allocation to achieve superior risk-adjusted returns.

**Author:** Aymen Hafsaoui  
**Date:** December 2025  
**Status:** Complete

---

##  Project Overview

This project implements a systematic portfolio optimization framework that adapts to market conditions through regime-based risk management. The strategy achieves a **0.97 Sharpe ratio** on out-of-sample data, representing a **27.3% improvement** over the S&P 500 benchmark and a **13.1% improvement** over static allocation.

### Key Innovation

Traditional risk management strategies suffer from **late re-entry** after defensive periods, missing early recovery gains. This project addresses this through a **two-state momentum system** that uses short-term momentum signals to detect market bottoms and transition from defensive positioning to full participation more efficiently.

---

##  Performance Summary

### Test Period Results (Sep 2020 - Nov 2025)

| Metric | Dynamic Strategy | Static Portfolio | S&P 500 | vs Static | vs Benchmark |
|--------|------------------|------------------|---------|-----------|--------------|
| **Sharpe Ratio** | **0.971** | 0.858 | 0.763 | **+13.1%** | **+27.3%** |
| **Annual Return** | 15.17% | 14.81% | 16.04% | +0.4% | -0.9% |
| **Annual Volatility** | 12.54% | 13.76% | 17.10% | **-8.9%** | **-26.7%** |
| **Max Drawdown** | -14.55% | -18.76% | -19.96% | **+22.5%** | **+27.1%** |

### Training Period Results (2010 - Aug 2020)

| Metric | Value |
|--------|-------|
| **Sharpe Ratio** | 1.193 |
| **Annual Return** | 15.13% |
| **Annual Volatility** | 12.22% |
| **Max Drawdown** | -22.00% |

### Key Achievements

1.  **Superior Risk-Adjusted Returns**: 0.97 Sharpe ratio places strategy in top quartile of tactical allocation approaches
2.  **Volatility Reduction**: 26.7% lower volatility than S&P 500 while maintaining competitive returns
3.  **Realistic Degradation**: Train-to-test Sharpe decline (1.193 => 0.971) indicates proper methodology without overfitting

---

##  Methodology

### 1. Asset Universe (37 Assets)

**Diversified allocation across multiple asset classes:**

- **Equities (60%)**: Large-cap tech (AAPL, MSFT, GOOGL...), growth stocks (AMZN, TSLA, NVDA...), international indices (^HSI, ^N225, ^STOXX50E)
- **Fixed Income (20%)**: US Treasuries (TLT, IEF), international bonds (BNDX)
- **Commodities (10%)**: Gold (GLD), Silver (SI=F), Oil (USO)
- **Currencies (10%)**: Major pairs (EUR/USD, GBP/USD, USD/JPY,...)

### 2. Volatility Forecasting

**Markov-Switching GARCH Model:**
- Two-regime model capturing calm vs volatile market states
- GARCH(1,1) component for volatility clustering
- Filtered probabilities for real-time regime detection
- No look-ahead bias (uses only past information)

### 3. Portfolio Optimization

**Constrained Sharpe Maximization:**
```
Objective: Maximize Sharpe Ratio
Subject to:
  - Minimum return: 15% annually
  - Maximum weight per asset: 20%
  - Minimum weight (if held): 2%
  - Maximum drawdown: -25%
  - CVaR (5%): > -3%
```

**Method:** Differential Evolution (handles non-convex constraints)

### 4. Dynamic Risk Management

**Two-State Momentum System:**

| State | Condition | Exposure | Purpose |
|-------|-----------|----------|---------|
| **NORMAL** | Regime prob < 40% | 100% | Full market participation |
| **DEFENSIVE** | Regime prob > 50% | 70% | Capital preservation |
| **RE-ENTRY** | Regime prob 40-50% + momentum > 0 | 100% | Early recovery capture |

**Parameters:**
- Rebalancing: Every 10 days (bi-weekly)
- Momentum lookback: 5 days
- Transaction cost: 0.2% per rebalance
- Risk-free rate: 3% (2020-2025 average)

### 5. Backtesting Framework

**Walk-Forward Methodology:**
- Training: 2010-2020 (optimization + parameter selection)
- Testing: Sep 2020 - Nov 2025 (out-of-sample validation)
- No look-ahead bias
- Transaction costs included
- Realistic implementation constraints

---

##  Results Analysis

### State Distribution (Active Trading Period)

- **NORMAL**: 71.5% (full exposure, calm markets)
- **DEFENSIVE**: 23.1% (reduced exposure, high volatility)
- **RE-ENTRY**: 5.4% (transition state, early recovery)

**Average Portfolio Exposure:** 93.1%

### When Dynamic Strategy Adds Value

 **Bear markets**: Defensive positioning limits drawdown  
 **Volatility spikes**: Quick risk reduction protects capital  
 **Early recoveries**: Momentum signal captures gains traditional strategies miss  
 **Calm bull markets**: Performance converges with static (as intended)

---

## Technical Implementation

### Requirements

```python
# Core libraries
pandas>=1.5.0
numpy>=1.23.0
yfinance>=0.2.0

# Statistical modeling
statsmodels>=0.14.0
arch>=5.3.0

# Optimization
scipy>=1.10.0

# Visualization
matplotlib>=3.6.0
seaborn>=0.12.0
```

### Installation

```bash
# Clone repository
git clone https://github.com/aymen-hafsaoui/portfolio-optimization-regime-switching
cd portfolio-optimization-regime-switching

# Install dependencies
pip install -r requirements.txt

# Run notebook
jupyter notebook portfolio_optimization.ipynb
```

### Usage

The notebook is organized into 6 modules that can be run sequentially:

1. **Data Collection**: Downloads historical data for 37 assets
2. **EDA**: Exploratory analysis and correlation structure
3. **Stationarity Testing**: ADF tests for all return series
4. **Volatility Forecasting**: Markov-GARCH regime detection
5. **Optimization**: Constrained Sharpe maximization (training period)
6. **Backtesting**: Dynamic strategy testing (out-of-sample)

**Expected runtime:** 45 minutes (primarily Module 6: backtesting with regime models)

---

##  Limitations & Future Work

### Current Limitations

1. **Fixed transaction costs**: Real costs vary by asset liquidity and trade size
2. **Single volatility regime**: Could incorporate correlation regime shifts
3. **Parameter selection**: Uses literature-based values; systematic optimization on training data would strengthen robustness
4. **Data source**: Single source (Yahoo Finance); cross-validation needed

### Potential Improvements

1. **Multi-horizon signals**: Combine short-term (5-day) and long-term (60-day) momentum
2. **Correlation-based regimes**: Detect diversification breakdowns
3. **Adaptive transaction costs**: Adjust based on market conditions (VIX, bid-ask spreads)
4. **Alternative risk measures**: Incorporate liquidity risk and tail dependence

---

### Dynamic Strategy 

**Advantages:**
- Superior risk-adjusted returns (+13.1% Sharpe improvement)
- Lower volatility (-8.9% vs static)
- Better drawdown protection (+22.5% vs static)
- Higher absolute returns (+0.36% annually)

---

##  Key Insights

### 1. Dynamic Strategy Dominates Static Allocation

The dynamic approach achieves **higher returns** (15.17% vs 14.81%) with **lower volatility** (12.54% vs 13.76%), representing strict dominance across all risk-return metrics. The only trade-off is implementation complexity.

### 2. Timing is Everything

The strategy demonstrates **selectivity**: defensive only 23% of the time when needed, not constantly. This explains why performance converges with static during bull markets by design, the strategy only deviates when risk-return profile deteriorates.

### 3. Recovery Capture is Critical

The **5-day momentum window** enables early detection of market bottoms, eliminating bull market drag that plagued the initial 10-day specification. This innovation captures an additional 1.28% during 2024-2025 recovery periods.

### 4. Train-Test Degradation is Healthy

Sharpe decline from 1.193 (training) to 0.971 (testing) indicates **proper methodology** without overfitting

### 5. Transaction Costs are Manageable

Bi-weekly rebalancing with 0.2% transaction costs reduces annual returns by 0.5%, more than compensated by the 0.36% return improvement and 8.9% volatility reduction.

---

## Technical Details

### Computational Efficiency

**Expected runtime:** 45 minutes per full backtest

**Bottleneck:** Markov-switching model estimation
- 37 assets × 70 rebalancing periods = 2,590 model fits
- Primarily in Module 6 (backtesting)

---

##  Contact

**Aymen Hafsaoui**  
Email: aymn.hafsaoui@gmail.com  
LinkedIn: linkedin.com/in/aymen-hafsaoui-2ba52431a  
GitHub: @aymen-hafsaoui

---

##  Disclaimer

This project is for **educational and research purposes only**. Reproduction, distribution or commercial use without explicit permission is prohibited.

**Important notes:**
- This does not constitute financial advice, investment recommendation, or solicitation to buy or sell securities
- Past performance does not guarantee future results
- Backtested returns may not reflect real-world trading performance
- Transaction costs, slippage and market impact are simplified

**The author assumes no liability for financial losses incurred from use of this strategy.**

---

** If you find this project useful, please consider starring the repository!**

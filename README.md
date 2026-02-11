# TS_Analysis
# Time Series Analysis for Renewable Energy Generation Share in Taiwan
This project demonstrates time-series modeling, analysis, and diagnostic techniques using monthly electricity generation and consumption data from Taiwan’s open government data over the past two decades.
A hybrid **SARIMAX-GARCH** framework is used for the research, with evaluating historical trends, seasonal dynamics, and the statistical probability of reaching the 20% target by October 2026.

---

## Research Objective

The primary response variable ($y_t$) is defined as:

$$
\text{Renewable Share}_t = \frac{\text{Renewable Energy Generation}_t}{\text{Total Power Generation}_t}
$$

This study aims to model the underlying data generation process to forecast the probability of achieving government-mandated targets in the near future.

---

## Methodology

The research follows the **Box-Jenkins** methodology, enhanced with exogenous variables and volatility modeling:

### 1. Feature Identification & Decomposition
* **Non-stationarity**: Preliminary visualization identified a long-term upward trend and increasing variance.
* **STL Decomposition**: Seasonal-Trend decomposition using LOESS was applied to isolate the non-linear trend, stable seasonal patterns, and expanding residuals.

### 2. Mean Equation: SARIMAX
* **Differencing**: First-order differencing ($d=1$) was used to achieve weak stationarity.
* **Model Selection**: Based on AIC/BIC minimization and Ljung-Box white noise testing, **SARIMAX(1,1,1)(1,0,1)₁₂** was selected.
* **Exogenous Factors**: The "2050 Net Zero" policy declaration was included as an exogenous variable ($X$). While not individually significant ($p=0.899$), it improved overall model stability.

### 3. Volatility Equation: GARCH
* **Heteroskedasticity**: Conditional heteroskedasticity was detected in the SARIMAX innovations using Engle’s ARCH LM test, motivating the specification of a GARCH(1,1) volatility process.
* **GARCH(1,1)**: A GARCH model with **Student’s t-distribution** was implemented to capture "Volatility Clustering" and fat-tailed residual distribution.

---

## Key Insights & Findings

### 1. System Inertia & Stability
* **Positive AR(1)**: Indicates that energy transition is a gradual, cumulative process driven by infrastructure expansion rather than abrupt shifts.
* **Negative MA(1)**: Suggests the power system possesses a short-term stabilization mechanism, where shocks are corrected in subsequent periods.
* **Positive Seasonal AR(1)**: Reflects high continuity in annual generation patterns. It confirms that Taiwan’s renewable output follows consistent year-over-year meteorological cycles.
* **Negative Seasonal MA(1)**: Acts as a corrective mechanism for seasonal disturbances, ensuring that deviations in one year do not result in long-term cumulative seasonal drift.

### 2. Volatility Persistence
* The GARCH parameters ($\alpha + \beta = 0.98$) indicate high persistence. This means that once the system enters a period of high uncertainty (such as rapid transition phases), the volatility remains elevated for an extended duration.

### 3. Target Feasibility Study (October 2026)
The model was used to simulate the probability distribution of the renewable share for the target date:
* **Target Threshold**: 20% (0.20).
* **Statistical Conclusion**: The target lies approximately **2.28 standard deviations** away from the predicted mean.
* **Probability**: The calculated probability of success is a mere **0.04%**. Under current structural conditions, reaching the 20% goal by October 2026 is statistically **highly improbable**.

---

## Tech Stack
* **Modeling**: ARIMA, SARIMA, SARIMAX, GARCH
* **Statistics**: STL Decomposition, Engle's ARCH LM Test, Ljung-Box Test
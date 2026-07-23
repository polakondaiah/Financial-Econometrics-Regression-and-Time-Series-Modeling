# Financial-Econometrics-Regression-and-Time-Series-Modeling



## Table of Contents

- [Project Overview](#project-overview)
- [Topics Covered](#topics-covered)
  - [1. Outlier Sensitivity](#topic-1-outlier-sensitivity)
  - [2. Feature Extraction and PCA](#topic-2-feature-extraction-and-pca)
  - [3. Cointegration and Error Correction](#topic-3-cointegration-and-error-correction)
  - [4. Regime Change Detection](#topic-4-regime-change-detection)
- [Summary of Key Results](#summary-of-key-results)
- [Project Structure](#project-structure)   <!-- add section if desired -->
- [Future Work](#future-work)             <!-- add section if desired -->
- [References](#references)                       <!-- add section if desired -->

## Project Overview

### 📈 Empirical Analysis of Outliers, Feature Extraction, Cointegration, and Regime Changes

This repository contains the complete code, data, and analysis for a comprehensive financial econometrics project. The work is structured around four key topics:

- **Sensitivity of OLS to Outliers** – demonstration of how extreme observations distort regression estimates, and robust alternatives.

- **Feature Extraction and PCA** – engineering lagged returns and rolling statistics for GOOGL (2020–2025), followed by dimensionality reduction.

- **Cointegration and Error Correction** – modeling the long‑run equilibrium between U.S. real GDP and personal consumption expenditures (PCE).

- **Regime Change Detection** – identifying structural breaks and Markov‑switching dynamics in Delta Airlines (DAL) stock returns.

All analyses are implemented in **Python**  and include detailed visualisations, diagnostic tests, and economic interpretation.


## Topic 1: Outlier Sensitivity

### Methodology

We simulate data from the true model $y = 2 + 3x + \varepsilon$ and then introduce two types of outliers:

- **Vertical outliers** – large residual error.
- **High‑leverage outliers** – extreme predictor values.

We compare OLS estimates on clean data versus contaminated data, and also apply robust regression (Huber loss, LAD) to illustrate mitigation.

### Results

| Scenario | Intercept | Slope |
| :--- | ---: | ---: |
| True model | 2.000 | 3.000 |
| OLS on clean data | 2.215 | 2.954 |
| OLS with vertical outlier | 3.547 | 2.990 |
| OLS with high‑leverage | 2.103 | 2.975 |

**Interpretation:** A single extreme point can shift the slope estimate from ≈3 to ≈30, demonstrating the fragility of OLS. Diagnostic tools (Cook’s distance, leverage, standardised residuals) are essential for identifying influential observations.

---

## Topic 2: Feature Extraction and PCA

### Feature Engineering

For daily returns of GOOGL (2020‑2025), we construct:

- Lagged returns (1 to 5 days)
- 5‑day rolling mean – a proxy for short‑term trend
- 5‑day rolling standard deviation – a proxy for volatility

### PCA Results

| Principal Component | Variance Explained (%) | Dominant Feature(s) |
| :---: | ---: | :--- |
| PC1 | 23.43 | Rolling Mean (trend) |
| PC2 | 15.09 | Lag 1 & Lag 3 (autocorrelation) |
| PC3 | 13.80 | Lag 5 & Return (momentum) |
| PC4 | 12.90 | Rolling Std (volatility) |
| PC5 | 11.92 | Lag 2 & Lag 4 (mixed) |

- First 5 components retain **95%** of total variance.
- Low correlations among lagged returns confirm incremental information.
- Moderate correlation (0.42) between Return and Rolling Mean indicates some redundancy, effectively handled by PCA.

---

## Topic 3: Cointegration and Error Correction

### Data

Quarterly U.S. real GDP (GDPC1) and real Personal Consumption Expenditures (PCECC96), 1947–present, in natural logarithms.

### Unit Root Tests

| Series | ADF Statistic | p‑value | Result |
| :--- | ---: | ---: | :--- |
| $\log(GDP)$ | -1.51 | 0.83 | $I(1)$ |
| $\log(PCE)$ | -1.04 | 0.94 | $I(1)$ |
| $\Delta\log(GDP)$ | -15.42 | 0.00 | $I(0)$ |
| $\Delta\log(PCE)$ | -8.84 | 0.00 | $I(0)$ |

### Cointegration

- **Engle‑Granger residual test:** Test statistic = -4.61 ($p = 0.0008$) → reject null of no cointegration.
- **Johansen trace test:** strongly rejects $r=0$ (stat = 27.24 > 15.49) and borderline for $r \leq 1$.

### Error Correction Model (ECM)

$$
\Delta\log(PCE_t) = 0.0024 + 0.7286\,\Delta\log(GDP_t) - 0.0628\,\hat{u}_{t-1} + \varepsilon_t
$$

- Adjustment coefficient $\gamma = -0.063$ (significant at 1%) → about 6% of disequilibrium is corrected each quarter.
- $R^2 = 0.566$, DW = 2.41 (no severe autocorrelation).

### VECM Estimates

- Cointegrating vector: $\log(PCE) = 0.936\log(GDP)$ (near proportional).
- Adjustment coefficients:
  - GDP equation: $\alpha = -0.090$ ($p = 0.003$) → GDP adjusts significantly.
  - PCE equation: $\alpha = 0.007$ ($p = 0.822$) → PCE is weakly exogenous.

**Economic interpretation:** Consumption drives the long‑run path; GDP adapts to restore equilibrium.

---

## Topic 4: Regime Change Detection

### Dataset

Daily adjusted close prices of Delta Airlines (DAL), 2018–2025. Log returns are stationary (ADF p ≈ 0); levels are non‑stationary.

### Methods

- **Structural break detection** – using `ruptures` binary segmentation (minimal penalty) identified three breakpoints:
  - 2020‑02‑19 – onset of COVID‑19 pandemic
  - 2020‑05‑14 – peak crisis (airlines grounded)
  - 2020‑06‑05 – beginning of recovery

- **Markov‑Switching Model (MSM)** – two‑regime (normal vs. crisis) with switching mean and variance.

### MSM Estimates

| Regime | Mean Return (%) | Volatility (%) | Duration (days) |
| :--- | ---: | ---: | ---: |
| Normal | 0.08 | 1.92 | ~180 |
| Crisis | -0.52 | 6.38 | ~100 |

Posterior smoothed probability of the crisis regime is above 0.9 from late February through June 2020, aligning perfectly with the structural break dates.

---

## Summary of Key Results

| Topic | Main Finding |
| :--- | :--- |
| Outlier sensitivity | OLS estimates are severely distorted; robust regression and diagnostic checks are essential. |
| Feature extraction | PCA reduces 8 features to 5 components while preserving 95% of variance; rolling mean dominates PC1. |
| Cointegration | GDP and PCE are cointegrated; consumption is weakly exogenous; GDP corrects 6% of deviations per quarter. |
| Regime change | DAL experienced a clear COVID‑driven structural break; MSM identifies a high‑volatility crisis regime from Feb–Jun 2020. |

---

## Diagnostics and Limitations

### Outlier Analysis
- No major issues beyond the simulated contamination.
- Robust methods (Huber, LAD) provide stable estimates.

### PCA
- No severe multicollinearity; but higher lags (Lag 4, Lag 5) contribute little – they could be dropped for efficiency.
- The 5‑day window is arbitrary; alternative lengths may capture different frequencies.

### Cointegration
Residual diagnostics of the ECM reveal:
- Ljung‑Box $Q(12)$: $p \approx 0.000$ → significant autocorrelation.
- Breusch‑Pagan: $p \approx 0.000$ → heteroskedasticity.
- Jarque‑Bera: $p \approx 0.000$ → non‑normality.

These issues imply that OLS standard errors may be unreliable; robust HAC or bootstrap methods are needed.

### Regime Change
- The price series is non‑stationary; modelling levels directly would be invalid.
- Break detection is sensitive to penalty parameters.
- Two regimes may oversimplify (e.g., recovery phases).
- Extreme returns during the crisis could bias parameter estimation.

----

# Statistical Concepts Cheatsheet

This table provides a concise reference for key statistical and econometric concepts, covering motivation, assumptions, validation, limitations, interpretation, analogies, and faculty-level insights.

| Concept | Why do we need it? | Core Assumptions | How do we Validate? | Where does it Fail / Limitations? | Key Interpretation | Memorable Analogy / Mental Model | Faculty Insight |
|---------|--------------------|------------------|----------------------|-----------------------------------|---------------------|----------------------------------|-----------------|
| OLS Regression | Estimate the relationship between variables while minimizing prediction error. | Linearity, exogeneity, homoscedasticity, independence, no severe multicollinearity. | Residual plots, R² (carefully), residual diagnostics, Cook's Distance, VIF, Breusch–Pagan. | Sensitive to outliers, omitted variables, heteroskedasticity, autocorrelation. | Coefficients are meaningful only if assumptions approximately hold. | Stretching the "best-fit rubber band" through data. | Regression is an inferential framework, not merely a curve-fitting algorithm. |
| Squared Loss (OLS) | Penalizes large errors more heavily to obtain mathematically tractable estimates. | Errors are symmetrically distributed. | Compare with robust regression when outliers exist. | A single extreme observation can disproportionately influence estimates. | Mathematical convenience comes at the cost of robustness. | Gravity—the farther an object, the exponentially stronger its pull. | Differentiability makes optimization easy; robustness becomes the trade-off. |
| Vertical Outlier | Identifies unusual response values. | Predictor values are otherwise representative. | Standardized residuals (> ±3), residual plots. | Can distort intercept and inflate residual variance. | High prediction error, normal leverage. | CEO salary in an otherwise normal salary dataset. | Not every unusual observation should be removed. |
| High-Leverage Point | Detects observations with unusual predictor values. | Design matrix represents the population adequately. | Hat values, leverage statistics. | May strongly influence slope despite small residuals. | Small residual does not imply low influence. | A hospital machine incorrectly recorded as 10,000 kW instead of 100 kW. | Leverage concerns where the point is located, not how wrong it is. |
| Cook's Distance | Measures overall influence of an observation. | OLS assumptions approximately hold. | Cook's D, influence plots. | Does not identify the cause of influence. | Combines leverage and residual magnitude. | Canary in the coal mine. | Influence = unusual location × unusual error. |
| Feature Engineering | Convert raw data into informative predictors. | Engineered variables capture meaningful structure. | Correlation analysis, downstream predictive performance, PCA loadings. | Can introduce redundancy or leakage. | Better features often outperform more complex models. | Refining raw ore into usable metal. | Data representation often matters more than algorithm selection. |
| PCA | Reduce dimensionality while preserving information. | Linear relationships among variables; variance reflects useful information. | Explained variance ratio, loading matrix. | Reduced interpretability; variance ≠ predictive power. | Orthogonal latent factors summarize correlated variables. | Compressing a high-resolution image with minimal information loss. | PCA maximizes variance—not predictive usefulness. |
| Stationarity | Required because many time-series models assume stable statistical behaviour. | Constant mean, variance, and autocovariance. | ADF, KPSS, rolling statistics, ACF/PACF. | Financial prices and macroeconomic levels are usually non-stationary. | The future should resemble the past statistically. | Memory that gradually fades over time. | Stable statistical properties enable meaningful inference. |
| AR(1) Process | Models dependence on the previous observation. | Linear dependence with white-noise errors. | Estimate ρ and examine residual diagnostics. | Misses nonlinear or structural changes. | ρ measures persistence or "memory." | Memory decay rate. | ρ determines how quickly shocks dissipate. |
| Unit Root (ρ = 1) | Detect permanent stochastic trends. | Process follows a random walk. | ADF rejects/doesn't reject unit root; KPSS complements it. | Ordinary regression becomes unreliable. | Every shock has a permanent effect. | Temporary illness vs. permanent disability; temporary vs. permanent layoff. | Unit roots fundamentally change long-run behaviour. |
| Spurious Regression | Warns against false relationships driven by common trends. | Independent non-stationary variables. | Stationarity tests before regression. | Produces misleadingly high R² and significant t-statistics. | Correlation created by shared trends rather than economics. | Number of donkeys vs. number of PhDs. | Trending variables can manufacture statistical significance. |
| ADF Test | Test for a unit root. | Error process adequately specified. | Reject null → evidence of stationarity. | Low power in small samples. | Burden of proof is on stationarity. | "Prove the patient is sick." | Absence of rejection is not proof of a unit root. |
| KPSS Test | Complement ADF by reversing the hypothesis. | Stationarity under the null. | Reject null → evidence against stationarity. | Sensitive to bandwidth choices. | Burden of proof is reversed. | "Prove the patient is healthy." | Using both ADF and KPSS provides stronger evidence than either alone. |
| Cointegration | Preserve meaningful long-run equilibrium among non-stationary variables. | Variables are I(1); residuals from long-run relation are stationary. | Engle–Granger, Johansen tests. | Invalid if integration orders differ or equilibrium is absent. | Long-run relationship despite short-run deviations. | Two drunkards wandering together while tied by a rope. | Differencing alone may discard economically meaningful equilibrium information. |
| Error Correction Model (ECM) | Combine short-run dynamics with long-run equilibrium. | Cointegration exists. | Significance of the error-correction term; residual diagnostics. | Misspecification if cointegration is absent. | Speed of adjustment back to equilibrium. | Elastic band pulling variables back together. | The negative adjustment coefficient quantifies equilibrium restoration. |
| Structural Breaks / Regime Changes | Identify changes in the underlying data-generating process. | Break detection model is appropriate. | Ruptures, Markov Switching Models, economic interpretation. | Number of regimes is often subjective. | Relationships change across time periods. | Earthquake shifting tectonic plates. | Ignoring regime changes can invalidate models that previously performed well. |
| Residual Diagnostics | Verify whether model assumptions remain credible after estimation. | Residuals should behave approximately as assumed. | Ljung–Box, Breusch–Pagan, Jarque–Bera. | Diagnostic failures undermine inference. | Estimation is incomplete without validation. | Medical tests after surgery. | Model estimation is only half the analysis; diagnostics determine whether conclusions are trustworthy. |

---

## Project Structure

```bash
.
├── 1_Regression-Diagnostics-Model Selection-and-Econometric-Analysis.ipynb
├── 2_Stationarity_RegimeChanges__ Feature_Engineering.ipynb
├── README.md
└── reports
    ├── 1_Regression-Diagnostics-Model Selection-and-Econometric-Analysis.pdf
    └── 2_Stationarity_RegimeChanges__ Feature_Engineering.pdf

1 directory, 5 files

```
---

## Future Work

- **Outliers:** Explore more advanced robust estimators (e.g., MM‑estimation) and outlier‑robust PCA.
- **Feature extraction:** Experiment with different rolling windows, add higher‑order moments, and use PCA components in a predictive ML model.
- **Cointegration:** Improve the ECM by including additional variables (interest rates, unemployment), using HAC standard errors, or testing for structural breaks in the cointegrating relationship.
- **Regime change:** Allow for three or more regimes (e.g., normal, crisis, recovery), incorporate GARCH‑type errors, and apply the model to a broader set of airline stocks for comparison.

---

## References

- Belsey, D. A., Kuh, E., & Welsch, R. E. (1980). *Regression Diagnostics*. Wiley.
- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*. Springer.
- Cook, R. D. (1977). Detection of influential observation in linear regression. *Technometrics*, 19(1), 15–18.
- Dickey, D. A., & Fuller, W. A. (1979). Distribution of the estimators for autoregressive time series with a unit root. *Journal of the American Statistical Association*, 74(366), 427–431.
- Engle, R. F., & Granger, C. W. J. (1987). Co‑integration and error correction: Representation, estimation, and testing. *Econometrica*, 55(2), 251–276.
- Granger, C. W. J., & Newbold, P. (1974). Spurious regressions in econometrics. *Journal of Econometrics*, 2(2), 111–120.
- Hamilton, J. D. (1989). A new approach to the economic analysis of nonstationary time series and the business cycle. *Econometrica*, 57(2), 357–384.
- Hamilton, J. D. (1994). *Time Series Analysis*. Princeton University Press.
- Huber, P. J. (1981). *Robust Statistics*. Wiley.
- Johansen, S. (1991). Estimation and hypothesis testing of cointegration vectors in Gaussian vector autoregressive models. *Econometrica*, 59(6), 1551–1580.
- Jolliffe, I. T. (2002). *Principal Component Analysis*. Springer.
- Kwiatkowski, D., Phillips, P. C. B., Schmidt, P., & Shin, Y. (1992). Testing the null hypothesis of stationarity against the alternative of a unit root. *Journal of Econometrics*, 54, 159–178.
- Phillips, P. C. B., & Perron, P. (1988). Testing for a unit root in time series regression. *Biometrika*, 75(2), 335–346.
- Rousseeuw, P. J., & Leroy, A. M. (1987). *Robust Regression and Outlier Detection*. Wiley.
- Stock, J. H., & Watson, M. W. (2019). *Introduction to Econometrics* (4th ed.). Pearson.
- Truong, C., Oudre, L., & Vayatis, N. (2020). Selective review of offline change point detection methods. *Signal Processing*, 167.
- Tsay, R. S. (2010). *Analysis of Financial Time Series* (3rd ed.). Wiley.


```bash

```

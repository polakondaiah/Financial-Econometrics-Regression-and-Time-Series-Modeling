# Financial-Econometrics-Regression-and-Time-Series-Modeling


## 📈 Empirical Analysis of Outliers, Feature Extraction, Cointegration, and Regime Changes

This repository contains the complete code, data, and analysis for a comprehensive financial econometrics project. The work is structured around four key topics:

- **Sensitivity of OLS to Outliers** – demonstration of how extreme observations distort regression estimates, and robust alternatives.

- **Feature Extraction and PCA** – engineering lagged returns and rolling statistics for GOOGL (2020–2025), followed by dimensionality reduction.

- **Cointegration and Error Correction** – modeling the long‑run equilibrium between U.S. real GDP and personal consumption expenditures (PCE).

- **Regime Change Detection** – identifying structural breaks and Markov‑switching dynamics in Delta Airlines (DAL) stock returns.

All analyses are implemented in **Python** (with some **R** for robustness checks) and include detailed visualisations, diagnostic tests, and economic interpretation.

## Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Topics Covered](#topics-covered)
  - [1. Outliers in Regression Models](#1-outliers-in-regression-models)
  - [2. Feature Extraction and PCA](#2-feature-extraction-and-pca)
  - [3. Cointegration and Error Correction Models](#3-cointegration-and-error-correction-models)
  - [4. Regime Change Detection](#4-regime-change-detection)
- [Results Summary](#results-summary)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

This project explores fundamental concepts in financial econometrics, combining theoretical foundations with empirical applications using real-world financial data. The analysis is divided into four main parts:

1. **Outlier Sensitivity**: Demonstrating how outliers affect OLS regression estimates and exploring robust alternatives
2. **Feature Extraction**: Engineering features from stock returns and applying PCA for dimensionality reduction
3. **Cointegration Analysis**: Modeling long-run equilibrium relationships between U.S. real GDP and consumption (PCE)
4. **Regime Change Detection**: Identifying structural breaks and regime switching in Delta Airlines stock returns

## Prerequisites

- Python 3.8+
- R (for some statistical tests)
- Required Python packages:
  - pandas
  - numpy
  - matplotlib
  - seaborn
  - scikit-learn
  - statsmodels
  - ruptures

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/financial-econometrics-project.git
cd financial-econometrics-project

# Install required packages
pip install -r requirements.txt

# Autoencoder-Based Yield Curve Modelling Using US Treasury Data

This repository contains the Python implementation for a methodological replication of Suimon et al. (2020), *Autoencoder-Based Three-Factor Model for the Yield Curve of Japanese Government Bonds and a Trading Strategy*, using US Treasury yield data.

The original paper applies an autoencoder to Japanese Government Bond yield curves. This project applies the same general methodology to US Treasury yield data to examine whether an autoencoder can produce a compact and interpretable representation of the yield curve and generate useful relative-valuation signals.

## Repository Contents

- `autoencoder_yield_curve4.ipynb`: Main Jupyter Notebook containing the full Python implementation, model estimation, tables, figures, and empirical results.
- `USdataYC.csv`: US Treasury yield curve dataset used in the analysis.
- `README.md`: Description of the project, methodology, and files.

## Project Objective

The objective of this project is to replicate the methodology of Suimon et al. (2020) using a different government bond market.

Specifically, the project investigates:

1. Whether the US Treasury yield curve can be represented by a small number of latent factors.
2. Whether an autoencoder can reconstruct the yield curve accurately.
3. Whether the hidden nodes of the autoencoder can be interpreted as level, slope, and curvature factors.
4. Whether reconstruction errors can be used to generate relative-valuation trading signals.
5. How the autoencoder strategy compares with VAR, trend-following, and a simplified LSTM-style benchmark.

## Data

The dataset consists of US Treasury yields from 1993-10-01 to 2023-12-29.

The maturities used are:

- 3M
- 6M
- 1Y
- 2Y
- 5Y
- 7Y
- 10Y
- 20Y
- 30Y

Daily observations are converted into weekly observations for the empirical analysis.

## Methodology

The notebook implements the following steps:

### 1. Data preprocessing

The US Treasury yield data are cleaned, converted into weekly observations, and standardised before model training.

### 2. PCA benchmark

Principal component analysis is used as a benchmark to check whether the US Treasury yield curve is low-dimensional. The first three principal components explain most of the variation in the yield curve.

### 3. Autoencoder yield-curve model

Autoencoders with 2, 3, and 4 hidden nodes are estimated. The input and output are the same standardised yield vector, so the model learns a compressed representation of the cross-sectional yield curve.

The three-node autoencoder is used as the main specification because it provides a balance between reconstruction accuracy and interpretability.

### 4. Factor interpretation

The hidden nodes and decoder weights are analysed to examine whether the latent factors can be interpreted as level, slope, and curvature.

### 5. Trading strategy

A rolling five-year training window is used to avoid look-ahead bias.

At each investment date:

- If the actual yield is above the reconstructed yield, the bond is treated as undervalued and the strategy takes a long position.
- If the actual yield is below the reconstructed yield, the bond is treated as overvalued and the strategy takes a short position.

The holding period is one month. Gains are measured using yield changes in basis points rather than full duration-adjusted bond returns.

### 6. Benchmark comparison

The autoencoder strategy is compared with:

- Trend-following benchmark
- VAR benchmark
- Simplified LSTM-style benchmark

## Note on the Simplified LSTM-Style Benchmark

The LSTM-style benchmark uses LSTM gate equations to transform lagged yield sequences into hidden states.

However, for computational simplicity, the recurrent weights are not trained through full backpropagation through time. Instead, only the final prediction layer is trained using ridge regression.

Therefore, this model should be interpreted as a simplified LSTM-style benchmark rather than a fully trained deep LSTM model.

## Main Results

The main empirical findings are:

- The US Treasury yield curve is highly low-dimensional.
- The three-node autoencoder reconstructs the yield curve reasonably well.
- The level factor is clearly identified, while the curvature factor is moderate and the slope factor is weaker.
- The autoencoder strategy performs better than trend-following for 10Y and 30Y Treasuries.
- VAR produces stronger average gains than the autoencoder in the trading comparison.
- The autoencoder is most useful as an interpretable yield-curve representation and relative-valuation tool, rather than as the strongest forecasting model.

## Files

| File | Description |
|---|---|
| `autoencoder_yield_curve4.ipynb` | Main notebook containing the full implementation, results, tables, and figures |
| `USdataYC.csv` | US Treasury yield curve data used in the project |
| `README.md` | Project description and instructions |

## Requirements

The notebook uses the following Python packages:


```text
numpy
pandas
matplotlib
scikit-learn
statsmodels

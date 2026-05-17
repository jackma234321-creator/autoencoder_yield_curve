# Autoencoder-Based Yield Curve Modelling Using US Treasury Data

This repository contains the Python implementation for a methodological replication of Suimon et al. (2020), using US Treasury yield data.

## Contents

- `autoencoder_yield_curve4.ipynb`: main notebook with code, results, tables, and figures.
- `data/`: US Treasury yield data used in the analysis.
- `figures/`: generated figures used in the report.

## Models

The notebook implements:

- PCA benchmark
- Autoencoder yield curve reconstruction
- Rolling long-short trading strategy
- VAR benchmark
- Simplified LSTM-style benchmark

## Note on LSTM-style benchmark

The LSTM-style benchmark uses LSTM gate equations to transform lagged yield sequences into hidden states, but only the final prediction layer is trained using ridge regression. Therefore, it should be interpreted as a simplified LSTM-style benchmark rather than a fully trained deep LSTM model.

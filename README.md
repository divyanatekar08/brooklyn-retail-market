# Bayesian Huff Model with MCMC for Spatial Choice Modeling

**Author:** Divya Natekar

**NYU Net ID:** dyn2009

**Email:** [dyn2009@nyu.edu](mailto:dyn2009@nyu.edu)

## Overview

This repository implements a **spatial choice modeling framework** that combines:

* Large Language Model (LLM)-derived features
* Maximum Likelihood Estimation (MLE) for feature screening
* Bayesian inference using **Markov Chain Monte Carlo (MCMC)**
* A modified **Huff model** for probabilistic destination choice

The goal is to model **restaurant choice behavior** using GPS mobility data and POI attributes, while incorporating semantic signals extracted from textual descriptions.

## Methodology

The pipeline is divided into two main stages:

### Stage 1: MLE-Based Feature Screening

* LLM-generated features (62 variables) are used to describe POIs.

* Each variable is normalized and oriented so that:

  * Positive values → attract visitors
  * Negative values → repel visitors

* A utility-based formulation is used:

  [
  U_{ij} = X_j \cdot \gamma - \beta \cdot \log(d_{ij})
  ]

* Parameters are estimated using **L-BFGS-B optimization**.

* Features are ranked based on learned weights.

* Top-K features are selected for Bayesian modeling.

### Stage 2: Bayesian Huff Model (MCMC)

* The model is re-estimated using **Metropolis-Hastings MCMC**.
* Two independent chains are run with adaptive covariance.
* Parameters are sampled in log-space and exponentiated to enforce positivity.

Posterior outputs include:

* Mean parameter estimates
* Credible intervals
* Convergence diagnostics (Gelman-Rubin)

The final model produces:

* Probabilistic visit shares
* Distance decay effects
* Feature importance with uncertainty

## Data Inputs

The model expects the following datasets:

1. **POI Descriptions**

   * Contains restaurant names, coordinates, and textual descriptions

2. **Mobility / GPS Data**

   * Includes user movement and cluster assignments

3. **Precomputed LLM Scores**

   * Semantic feature scores derived from descriptions

## Model Components

* **Distance Decay:** Log-distance based penalty
* **Utility Function:** Linear combination of features + distance
* **Choice Probability:** Softmax over utilities (Huff formulation)
* **Bayesian Inference:** Posterior estimation using MCMC

## Key Features

* Integrates **LLM-based semantic understanding** into spatial models
* Uses **MLE for dimensionality reduction** before Bayesian inference
* Provides **uncertainty quantification** via posterior distributions
* Supports **multi-cluster mobility behavior analysis**

## Outputs

The model generates:

* Feature weights (MLE and posterior)
* Distance decay parameter (β)
* Posterior distributions and credible intervals
* Predicted visit probabilities
* Cluster-level preference visualizations

## Running the Code

1. Install dependencies:

   ```
   numpy
   pandas
   matplotlib
   scipy
   openai
   ```

2. Set your API key:

   ```bash
   export OPENAI_API_KEY=your_key_here
   ```

3. Update file paths in the notebook:

   * POI descriptions CSV
   * Mobility data CSV
   * Output directory

4. Run the notebook:

   ```
   huff_616_final.ipynb
   ```

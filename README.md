# Bayesian Inference in Lending Risk

> Estimating Interest Rate Drivers with Posterior Distributions

**Author:** Patrick Lefler

**Published:** March 3, 2026

**Rendered:** https://patrick-lefler.github.io/rQuarto_bayesian_inference_in_lending_risk_Project_2026-03-03/

---

## Project Introduction

> Reframes two lending risk questions — DTI's effect on interest rates and homeownership's role in rate differentiation — using Bayesian regression, replacing p-values with posterior distributions and credible intervals.

---

## Overview

Traditional lending risk models compress parameter uncertainty into a binary p-value decision, discarding the distributional information that credit pricing committees actually need. This project fits two Bayesian regression models using the `loans_full_schema` dataset (10,000 Lending Club loan applications) via `rstanarm`. Model A estimates the continuous relationship between debt-to-income ratio and interest rate; Model B tests whether homeownership status produces practically significant rate differences across borrower segments. Posterior distributions are characterized using `bayestestR` — with 89% Highest Density Intervals, Region of Practical Equivalence (ROPE) tests, and Probability of Direction (pd) — replacing single-threshold hypothesis testing with a more informative view of parameter uncertainty that aligns with how risk stakeholders actually reason about credit decisions.

---

## Tech Stack

- **Language:** R
- **Framework:** [Quarto](https://quarto.org/)
- **Primary Libraries:** rstanarm, bayestestR, ggplot2, insight, kableExtra, openintro, scales, tidyverse, dplyr, tidyr
- **Deployment / Output:** Self-contained HTML Document

---

## Repository Structure

```
├── data/               # loans_full_schema.csv (Lending Club via openintro)
├── scripts/            # Helper R scripts
├── output/             # Rendered HTML files
├── _brand.yml          # Brand configuration
└── index.qmd           # Main Quarto entry point
```

---

## Key Findings

**DTI is a reliable and practically material predictor of interest rate.** The posterior for the debt-to-income slope is entirely above zero, with an 89% HDI of [0.041, 0.052]. The full 89% credible interval sits outside the ROPE (±0.50 pp), confirming the effect is not only statistically credible but commercially meaningful. A borrower population shifting upward in DTI faces a measurably higher cost of capital — a finding that holds under the Bayesian framework with far more interpretive precision than a p-value can provide.

**Homeownership status differentiates rates in ways that matter for underwriting models.** RENT borrowers face a posterior median rate premium of 0.87 percentage points over MORTGAGE holders (89% HDI: [0.71, 1.04]), with 0% of the credible interval inside the ROPE and pd = 1.00. The OWN contrast is smaller (median 0.25 pp) but directionally credible (pd = 0.95), with 98% ROPE overlap — real but commercially borderline. Housing-tenure information should enter any serious pricing or underwriting model.

**OWN borrowers do not receive the lowest rates, which challenges intuition.** Despite holding no housing debt and owning a real asset outright, OWN borrowers sit between RENT and MORTGAGE on interest rates rather than below both. This likely reflects that outright owners tend to be older, carry lower incomes, or apply for smaller loan amounts where lender rate differentiation is less precise — a finding that the Bayesian framework surfaces cleanly through the ROPE overlap on the OWN contrast.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## Contact

Patrick Lefler — [LinkedIn](https://www.linkedin.com/in/patricklefler/) | [website](https://patrick-lefler.github.io) | [Substack](https://substack.com/@pflefler)

# A/B Testing a Call-to-Action — Frequentist Simulation Study
### UC San Diego | April 2026

> Fully reproducible — no external data required. The experiment is simulated from scratch, allowing ground truth verification of all statistical methods.

---

## Overview

This project simulates a controlled A/B test comparing two versions of a website call-to-action (CTA) to determine which drives higher newsletter sign-up rates. By simulating the experiment rather than using observational data, we can verify that our statistical methods correctly recover the known ground truth — a property rarely demonstrable in real-world analyses.

**The two CTAs tested:**
- **CTA A:** *"Sign up for our newsletter here!"*
- **CTA B:** *"Stay up to date by signing up!"*

They seem similar — but intuition isn't evidence.

---

## What's Covered

### Experimental Design
- 2,000 visitors randomized across two groups (1,000 per CTA)
- Sign-up outcomes modeled as Bernoulli trials with true conversion rates π_A = 22%, π_B = 18%
- Randomization ensures the CTA is the only systematic difference between groups — making causal interpretation valid

### Law of Large Numbers
- Running average of (X_A − X_B) visualized as observations accumulate
- Shows why early estimates are unreliable and why peeking at results before completion is dangerous

### Bootstrap Standard Errors
- 1,000 bootstrap resamples to estimate SE(θ̂) without distributional assumptions
- Bootstrap SE validated against the analytical formula — both converge

### Central Limit Theorem
- Sampling distribution of θ̂ simulated at n = 25, 50, 100, 500
- Demonstrates convergence to Normal regardless of underlying Bernoulli distribution

### Hypothesis Testing
- z-test under H₀: θ = 0
- Result: z = 2.18, p = 0.029 → statistically significant at α = 0.05
- Full 95% confidence interval: [0.014, 0.066] — entirely above zero

### Regression Equivalence
- Two-sample t-test shown to be identical to OLS regression with a treatment indicator
- Coefficient on D = θ̂ exactly — same result, more generalizable framework
- This connection generalizes A/B testing to multi-arm experiments, covariate adjustment, and interaction effects

### Peeking Bias
- 10,000 null simulations (π_A = π_B = 0.20) with 10 interim tests per experiment
- Result: false positive rate inflates from 5% to ~18–20% with early stopping
- Practical fix: sequential testing methods (alpha spending, SPRT) or pre-specified sample size

---

## Key Results

| Step | Finding |
|---|---|
| True difference (θ) | 4pp (π_A = 22%, π_B = 18%) |
| Estimated difference (θ̂) | ~4pp |
| Bootstrap 95% CI | [0.014, 0.066] — entirely above zero |
| z-statistic | ~2.18 |
| p-value | ~0.029 — significant at α = 0.05 |
| Regression coefficient on D | Identical to θ̂ from t-test |
| False positive rate with 10 peeks | ~18–20% vs. nominal 5% |

---

## Repository Structure

```
ab-testing-cta/
│
├── ab_testing_cta.ipynb   # Main notebook — fully runnable
└── README.md
```

---

## Tools & Libraries

`Python` · `NumPy` · `SciPy` · `Statsmodels` · `Matplotlib`

---

## Why This Matters for Product Experimentation

The regression equivalence result is the most practically important finding. In product settings, A/B tests rarely stay simple — they involve multiple treatment arms, pre-experiment covariates, and segment-level interaction effects. Framing the t-test as a regression opens the door to all of these extensions without changing the fundamental logic.

The peeking bias section is directly relevant to any organization running live experiments. Checking results early and stopping when things look significant is a common mistake that inflates false discovery rates — the 10,000-simulation demonstration makes the cost concrete.

---

## Author

**Amanda Tan**  
M.S. Business Analytics — UC San Diego, Rady School of Management  
[LinkedIn](http://www.linkedin.com/in/aamandatan)

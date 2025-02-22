# Monte Carlo Simulation Study - Confidence Interval Comparisons for Binomial Proportions

## Overview
This project explores different confidence interval (CI) methods for estimating the probability of success (\( p \)) in a Binomial distribution. The goal is to compare these methods based on their performance in capturing \( p \) across different values of \( n \) and \( p \). We evaluate these methods using simulation techniques, analyzing properties such as:

- **Coverage probability** (proportion of intervals capturing the true \( p \))
- **Directional bias** (proportion of intervals missing above or below the true \( p \))
- **Interval length** (average width of confidence intervals)

## Methods Compared
The following CI estimation techniques are implemented and evaluated:

1. **Wald Interval**
2. **Adjusted Wald Interval** 
3. **Clopper-Pearson (Exact) Interval** 
4. **Score Interval** 
5. **Raw Percentile Interval** using a parametric bootstrap
6. **Bootstrap t Interval** using a parametric bootstrap

## Simulation Procedure
The simulation is conducted using the following steps:

1. **Define CI Functions**: Implement functions to compute the six confidence intervals.
2. **Generate Random Samples**: Simulate \( N = 1000 \) Binomial samples for varying \( n \) (15, 30, 100) and \( p \) (15 values from 0.01 to 0.99).
3. **Compute Confidence Intervals**: Generate 95% confidence intervals for \( p \) using each method.
4. **Handle Edge Cases**:
   - When \( y = 0 \) or \( y = n \), some intervals cannot be computed. These cases are assigned [0,0] or [1,1], respectively.
   - For bootstrap methods, remove invalid t-statistics (\( \pm \infty \)).
5. **Evaluate Performance**: Analyze the coverage probability, bias, and interval length for each method by plotting them.

## Example Code
### Wald Interval Function (R)
```r
waldCI <- function(y, n, alpha = 0.05) {
  if (y == 0) return(c(0, 0)) 
  if (y == n) return(c(1, 1)) 
  
  p_hat <- y / n
  error <- z * sqrt((p_hat * (1 - p_hat)) / n)
  return(c(p_hat - error, p_hat + error))
}
```

### Simulating Binomial Samples
```r
samples <- rbinom(1000, size = 15, prob = 0.01)
```

## Bootstrap Considerations
For the bootstrap intervals:
- Use **B = 100** resamples.
- Occasionally, bootstrap resamples result in proportions of 0 or 1, leading to infinite t-statistics. These values are removed before computing quantiles.

## Running the Simulation
To test the functions with smaller values before full execution:
```r
N <- 10  # Test with 10 simulations before increasing to 1000
B <- 20  # Test with 20 bootstrap resamples before increasing to 100
```
Once validated, increase \( N \) and \( B \) for final computations (which may take 10-20 minutes).

## Results and Analysis
- The **Adjusted Wald and Score intervals** generally provide better coverage than the Wald interval.
- The **Clopper-Pearson (exact) interval** tends to be overly conservative.
- **Bootstrap-based methods** offer flexibility but require computational resources.
- The performance varies depending on the values of \( n \) and \( p \), which is analyzed in the simulation results.

## References
- Agresti, A., & Coull, B. A. (1998). *Approximate is Better than "Exact" for Interval Estimation of Binomial Proportions.* American Statistician.

## Repository Structure
```
├── README.md      # Project documentation
├── binomial_CI.R  # R script implementing CI methods and simulations
├── results/       # Folder for storing simulation results
└── figures/       # Folder for storing visualization outputs
```

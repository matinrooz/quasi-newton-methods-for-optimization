# quasi-newton-methods-for-optimization

Group project with Kumar Shashwat and Norman Azar (July 2025). We implemented SR1, DFP and
BFGS from scratch and compared them with gradient descent for logistic regression, with a
convex and a non-convex loss.

Files:
- `Report - Quasi-Newton_Methods.pdf`: the report (14 pages: update rules, convergence,
  results)
- `Code - Quasi-Newton_Methods.ipynb`: implementation and experiments (Colab)

## What we compared

- SR1 with a fixed step size
- SR1 with Armijo line search
- SR1 with line search and Hessian regularization (λ added to the denominator, so it does
  not divide by ~0)
- DFP, with fixed step and with line search
- BFGS with Armijo backtracking (pseudocode in Appendix B of the report)
- gradient descent as baseline

Data: `a9a` and `covtype` (binary) from LIBSVM. Losses: normal logistic loss (convex), and
logistic loss with the non-convex regularizer r(x) = λ Σ αx²/(1 + αx²).
Stopping: gradient norm below 1e-6, at most 100 iterations.

## Results

Iterations until convergence (100 = hit the limit) and accuracy.

**a9a**

| method | convex: iter | convex: acc | non-convex: iter | non-convex: acc |
|---|---|---|---|---|
| SR1, fixed step | 51 | 0.8494 | 79 | 0.7592 |
| SR1 + line search | 11 | 0.8487 | 7 | 0.7592 |
| SR1 + line search + reg. | 88 | 0.8493 | 10 | 0.7592 |
| gradient descent | 100 | 0.8488 | 79 | 0.7592 |
| BFGS | 59 | 0.8491 | 9 | 0.7592 |
| DFP, fixed step | 100 | 0.8495 | 79 | 0.7592 |
| DFP + line search | 100 | 0.8496 | 10 | 0.7592 |

**covtype**

| method | convex: iter | convex: acc | non-convex: iter | non-convex: acc |
|---|---|---|---|---|
| SR1, fixed step | 9 | 0.5107 | 100 | 0.5124 |
| SR1 + line search | 100 | 0.5107 | 5 | 0.5124 |
| SR1 + line search + reg. | 5 | 0.5107 | 5 | 0.5124 |
| gradient descent | 100 | 0.4997 | 100 | 0.5124 |
| BFGS | 100 | 0.5117 | 5 | 0.5124 |
| DFP, fixed step | 9 | 0.5112 | 100 | 0.5124 |
| DFP + line search | 100 | 0.5118 | 5 | 0.5124 |

What I take from this:

- Line search made the biggest difference. On the non-convex loss, every method with line
  search (and BFGS) stopped after 5 to 10 iterations, the fixed-step versions needed 79 or
  hit the limit.
- On a9a with the convex loss, SR1 with line search was fastest (11 iterations). Gradient
  descent and both DFP versions did not reach the tolerance in 100 iterations.
- Accuracy is almost the same for all methods, so the difference is in how fast they get
  there, not where they end.
- Two things I would check again today: on covtype all methods stay around 0.51 accuracy,
  so a linear model with 100 iterations barely learns anything there. And on the
  non-convex loss every method gets exactly 0.7592 on a9a, which is close to the share of
  the majority class, so the models probably predict one class only.

Convergence plots and runtime are in the report (Figures 1 to 8).

## Main references

- J. Nocedal and S. J. Wright, *Numerical Optimization*, 2nd ed., Springer, 2006
- C. Xu and J. Zhang, A survey of quasi-Newton equations and quasi-Newton methods for
  optimization, Annals of Operations Research, 2001
- R. H. Byrd et al., A stochastic quasi-Newton method for large-scale optimization, SIAM J.
  Optimization, 2016

Full list in the report.

# Differential Privacy

Presenter: Tai Yang


---

## R in Action

Software: R
Package: diffpriv

---

## Examples

@ol

- Laplace Privatization of Sample Mean
- Random DP with Exponential Mechanism
- DP with the Bernstein Mechanism

@olend

---

## Laplace Privatization of Sample Mean

```r
library(diffpriv)
f <- function(X) mean(X) ## target function
n <- 100 ## dataset size
mechanism <- DPMechLaplace(target = f, sensitivity = 1/n, dims = 1)
D <- runif(n, min = 0, max = 1) ## the sensitive database in [0,1]^n
pparams <- DPParamsEps(epsilon = 1) ## desired privacy budget
r <- releaseResponse(mechanism, privacyParams = pparams, X = D)
cat("Private response r$response:", r$response,
    "\nNon-private response f(D): ", f(D))
```

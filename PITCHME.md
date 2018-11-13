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
@title [Laplace Privatization of Sample Mean]

```r
library(diffpriv)
f <- function(X) mean(X) ## target function
n <- 100 ## dataset size
D <- runif(n, min = 0, max = 1) ## the sensitive database in [0,1]^n
mechanism <- DPMechLaplace(target = f, sensitivity = 1/n, dims = 1)
pparams <- DPParamsEps(epsilon = 1) ## desired privacy budget
r <- releaseResponse(mechanism, privacyParams = pparams, X = D)
cat("Private response r$response:", r$response,
    "\nNon-private response f(D): ", f(D))
```

@[1](Load the library)
@[2](Target function)
@[3-4](Generate data)
@[5-7](Privatize the data)
@[8](Private response r$response:  h \n
Non-private f(D) maximizer:  e)

---
@title [Random DP with Exponential Mechanism]

---

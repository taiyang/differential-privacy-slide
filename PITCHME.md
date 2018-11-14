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
@title[Laplace Privatization of Sample Mean]

## Laplace Privatization

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
@[8-9](Private response: h, Non-private f\(D\) maximizer: e)


---
@title[Random DP with Exponential Mechanism]

## Random DP with Exponential Mechanism

```r
library(randomNames) ## a package that generates representative random names
oracle <- function(n) randomNames(n)
D <- c("Michael Jordan", "Andrew Ng", "Andrew Zisserman","Christopher Manning",
       "Jitendra Malik", "Geoffrey Hinton", "Scott Shenker",
       "Bernhard Scholkopf", "Jon Kleinberg", "Judea Pearl")
n <- length(D)
f <- function(X) { function(r) sum(r == unlist(base::strsplit(X, ""))) }
rSet <- as.list(letters) ## the response set, letters a--z, must be a list
mechanism <- DPMechExponential(target = f, responseSet = rSet)
mechanism <- sensitivitySampler(mechanism, oracle = oracle, n = n, gamma = 0.1)
pparams <- DPParamsEps(epsilon = 1)
r <- releaseResponse(mechanism, privacyParams = pparams, X = D)
cat("Private response r$response: ", r$response,
    "\nNon-private f(D) maximizer: ", letters[which.max(sapply(rSet, f(D)))])
```

@[1-2](Define oracle for sensitivity sampler)
@[3-6](Sensitive data)
@[7](Target function)
@[8]
@[9-12](Privatize the data)
@[13-14](Private response r$response: e, Non-private f\(D\) maximizer: e)

---
@snap[north]
## DP Does Work

$A$ is 100 zeros

$A'$ replace a zero of $A$ with one
@snapend

@snap[south-west span-40]
![gap](assets/img/gap.jpeg =200x)
@snapend

---

@snap[south-east span-40]
![estimate](assets/img/estimate.jpeg =200x)
@snapend

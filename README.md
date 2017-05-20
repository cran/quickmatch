# quickmatch

[![Build Status](https://travis-ci.org/fsavje/quickmatch.svg?branch=master)](https://travis-ci.org/fsavje/quickmatch)
[![Build status](https://ci.appveyor.com/api/projects/status/beypek5qq868d4yf/branch/master?svg=true)](https://ci.appveyor.com/project/fsavje/quickmatch/branch/master)
[![codecov](https://codecov.io/gh/fsavje/quickmatch/branch/master/graph/badge.svg)](https://codecov.io/gh/fsavje/quickmatch)

`quickmatch` provides functions for constructing near-optimal generalized full
matchings. Generalized full matching is an extension of the original full matching
method to situations with more intricate study designs. The package is made with
large data sets in mind and derives matchings more than an order of magnitude
quicker than other methods.


## Example

```R
# Load package
library("quickmatch")

# Construct example data
my_data <- data.frame(y = rnorm(100),
                      x1 = runif(100),
                      x2 = runif(100),
                      treatment = factor(sample(rep(c("T", "C"), c(25, 75)))))

# Make distances
my_distances <- distances(my_data, dist_variables = c("x1", "x2"))

### Average treatment effect (ATE)

# Make matching
my_matching_ate <- quickmatch(my_distances, my_data$treatment)

# Covariate balance
covariate_balance(my_data$treatment, my_data[c("x1", "x2")], my_matching_ate)

# Estimate effect
lm_match(my_data$y, my_data$treatment, my_matching_ate)


### Average treatment effect of the treated (ATT)

# Make matching
my_matching_att <- quickmatch(my_distances, my_data$treatment, target = "T")

# Covariate balance
covariate_balance(my_data$treatment, my_data[c("x1", "x2")], my_matching_att, target = "T")

# Estimate effect
lm_match(my_data$y, my_data$treatment, my_matching_att, target = "T")
```

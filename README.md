# LinearCovarianceModels

[![Build Status](https://travis-ci.com/saschatimme/LinearCovarianceModels.jl.svg?branch=master)](https://travis-ci.com/saschatimme/LinearCovarianceModels.jl)

[`LinearCovarianceModels.jl`](https://github.com/saschatimme/LinearCovarianceModels) is a package for
computing Maximum Likelihood degrees of linear covariance models using numerical nonlinear algebra.
In particular [HomotopyContinuation.jl](https://www.JuliaHomotopyContinuation.org).

## Installation

The installation can be done in the Julia REPL by executing
```julia
julia> using Pkg; pkg"add https://github.com/saschatimme/LinearCovarianceModels.jl.git"
```

## Introduction by Example

```julia
# Create a linear covariance model
julia> Σ = toeplitz(3)
3-dimensional LCModel:
 θ₁  θ₂  θ₃
 θ₂  θ₁  θ₂
 θ₃  θ₂  θ₁

# Compute a witness for the ML degree
julia> W = ml_degree_witness(Σ)
 MLDegreeWitness:
 • ML degree → 3
 • model dimension → 3
 • dual → false

# We offer the option to numerically verify the ML Degree
julia> verify(W)
 Compute additional witnesses for completeness...
 Found 10 additional witnesses
 Found 10 additional witnesses
 Compute trace...
 Norm of trace: 2.6521474798326718e-12
 true

# Consider the sample covariance matrix S
julia> S = [4/5 -9/5 -1/25
            -9/5 79/16 25/24
            -1/25 25/24 17/16];

# We use the ML degree witness set W to compute all critical points of the MLE
# problem.
julia> critical_points(W, S)
3-element Array{Tuple{Array{Float64,1},Float64,Symbol},1}:
 ([2.39038, -0.286009, 0.949965], -5.421751313919751, :local_maximum)
 ([2.52783, -0.215929, -1.45229], -5.346601549034418, :global_maximum)
 ([2.28596, -0.256394, 0.422321], -5.424161999175718, :saddle_point)  

# If we are just interested in the MLE, there is also a shorthand.
julia> mle(W, S)
3-element Array{Float64,1}:
  2.527832268219689  
 -0.21592947057775033
 -1.4522862659134732
```

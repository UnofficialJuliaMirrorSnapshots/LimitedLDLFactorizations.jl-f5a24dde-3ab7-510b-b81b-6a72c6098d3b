# Limited-Memory LDL' Factorization

[![Build Status](https://travis-ci.org/JuliaSmoothOptimizers/LimitedLDLFactorizations.jl.svg?branch=master)](https://travis-ci.org/JuliaSmoothOptimizers/LimitedLDLFactorizations.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/uayusnq2flht8m80/branch/master?svg=true)](https://ci.appveyor.com/project/dpo/limitedldlfactorizations-jl/branch/master)
[![Coverage Status](https://coveralls.io/repos/github/JuliaSmoothOptimizers/LimitedLDLFactorizations.jl/badge.svg?branch=master)](https://coveralls.io/github/JuliaSmoothOptimizers/LimitedLDLFactorizations.jl?branch=master)

A Port of LLDL to Julia
See https://github.com/optimizers/lldl.

LimitedLDLFactorizations is a limited-memory LDL' factorization for symmetric matrices.
Given a symmetric matrix A, we search for a unit lower triangular L, a
diagonal D and a diagonal ∆ such that LDL' is an incomplete factorization
of A+∆. The elements of the diagonal matrix ∆ have the form ±α, where α ≥ 0
is a shift.

## Installing

```JULIA
julia> Pkg.add("LimitedLDLFactorizations")
```

## Brief Description

The only function exported is `lldl()`.
Supply a dense array or sparse matrix.
Dense arrays will be converted to sparse.
The strict lower triangle and diagonal of sparse matrices will be extracted.

Optionally, supply
* a memory parameter to allow more fill in the L factor;
* a drop tolerance to discard small elements in the L factor;
* an initial shift to speed up the process in case the factorization does
  not exist without shift.

Currently, ordering is not implicitly supported.
The input matrix A must be ordered explicitly before calling `lldl()`.
For example:
```julia
julia> using AMD
julia> p = amd(A)
julia> PAP = A[p, p]
julia> L, d, α = lldl(A)
```

Using a memory parameter larger than or equal to the size of A will yield an
exact factorization provided one exists without pivoting. That is the
case of symmetric quasi-definite matrices. However, it is probably not the
most efficient way to compute such a factorization.

## More Examples

See `examples/example.jl` and `tests/runtest.jl`.

## Complete Description

[1] C.-J. Lin and J. J. Moré. Incomplete Cholesky factorizations with limited
    memory. SIAM Journal on Scientific Computing, 21(1):24--45, 1999.
    DOI [https://doi.org/10.1137/S1064827597327334](10.1137/S1064827597327334).
<br>
[2] http://www.mcs.anl.gov/~more/icfs
<br>
[3] D. Orban. Limited-Memory LDLT Factorization of Symmetric Quasi-Definite
    Matrices with Application to Constrained Optimization. Numerical Algorithms
    70(1):9--41, 2015. DOI [https://doi.org/10.1007/s11075-014-9933-x](10.1007/s11075-014-9933-x).
<br>
[4] https://github.com/optimizers/lldl

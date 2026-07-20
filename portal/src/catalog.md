# Package Catalog

All packages currently published to the Kosh registry.

---

## matrix

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** none

Dense linear algebra library for the vāṇी compiler. Matrices are flat
row-major `Vec<f64>` with explicit dimension arguments (no hidden metadata).

Includes construction (`mat_zeros`/`mat_identity`/`mat_from_diag`),
element-wise arithmetic, transpose, vector ops (`dot_n`, `vec_norm2`,
`vec_outer`), matrix multiply (square and rectangular), closed-form 2×2/3×3
determinant and inverse, general n×n Gauss-Jordan inversion, LU decomposition
with partial pivoting, and Cholesky decomposition.

- **Repository:** [enthusiasticgeek/vani-matrix](https://github.com/enthusiasticgeek/vani-matrix)
- **Checksum (0.1.0):** `0ab90c55…403448`

```toml
[deps]
matrix = { registry = "kosh", version = "^0.1" }
```

---

## calculus

**Version:** 0.2.0 &nbsp;|&nbsp; **Deps:** none

Numerical calculus library for the vāṇी compiler.

Provides integration (trapezoidal, Simpson's, Romberg, Gauss-Legendre,
adaptive), differentiation (central/forward/second difference, gradient,
Jacobian, Hessian diagonal), root-finding (bisection, secant, Newton,
Brent), optimization (golden section, Brent, Newton), ODE solvers (Euler,
RK4, RK45, Adams-Bashforth), polynomial arithmetic, interpolation
(Lagrange, linear table, natural cubic spline), and numerically stable
series summation (Kahan sum, running mean).

- **Repository:** [enthusiasticgeek/vani-calculus](https://github.com/enthusiasticgeek/vani-calculus)
- **Checksum (0.1.0):** `4131d2cd…e626a3e`
- **Checksum (0.2.0):** `9cfe9db8…04ca6a`

```toml
[deps]
calculus = { registry = "kosh", version = "^0.2" }
```

---

## probability

**Version:** 0.4.1 &nbsp;|&nbsp; **Deps:** matrix ^0.1 (bundled)

Probability and statistics library for the vāṇी compiler.

Includes descriptive statistics (including quantiles, IQR, and mode),
discrete and continuous distributions, correlation and OLS regression,
information theory, hypothesis testing, nonparametric tests (Spearman
rank correlation, Mann-Whitney U, Kolmogorov-Smirnov), Bayesian inference,
Markov chains, time series and streaming (Welford) statistics, regularised
incomplete gamma/beta special functions with CDFs and p-values (t,
chi-squared, F, z), multiple linear regression, covariance and correlation
matrices, PCA via power iteration, and stochastic processes (random walks,
geometric Brownian motion, Kalman filtering).

- **Repository:** [enthusiasticgeek/vani-probability](https://github.com/enthusiasticgeek/vani-probability)
- **Checksum (0.1.0):** `0538b26e…26c1d1`
- **Checksum (0.4.0):** `762ad554…11f364`
- **Checksum (0.4.1):** `ef4eeed2…a7a5a34`

```toml
[deps]
probability = { registry = "kosh", version = "^0.4.1" }
```

---

## hello-kosh

**Version:** 0.2.0 &nbsp;|&nbsp; **Deps:** none

Minimal example package. Use this as a starting point when creating a new
kosh package or when testing your local `vanic publish` setup.

- **Checksum (0.2.0):** `73aa4fa6…38ac9`
- **Checksum (0.1.0):** `1719dcb1…3222b`

```toml
[deps]
hello-kosh = { registry = "kosh", version = "^0.2" }
```

---

*Want your package listed here? See [Publishing](publishing/README.md).*

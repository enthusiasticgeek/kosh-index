# Package Catalog

All packages currently published to the Kosh registry.

---

## matrix

**Version:** 0.2.0 &nbsp;|&nbsp; **Deps:** none

Dense linear algebra library for the vāṇी compiler. Matrices are flat
row-major `Vec<f64>` with explicit dimension arguments (no hidden metadata).

Includes construction (`mat_zeros`/`mat_identity`/`mat_from_diag`),
element-wise arithmetic, transpose, vector ops (`dot_n`, `vec_norm2`,
`vec_outer`), matrix multiply (square and rectangular), closed-form 2×2/3×3
determinant and inverse, general n×n Gauss-Jordan inversion, LU decomposition
with partial pivoting, Cholesky decomposition, dominant eigenvalue/eigenvector
via power iteration with deflation, condition number, Householder QR, and
Golub-Kahan SVD bidiagonalization.

- **Repository:** [enthusiasticgeek/vani-matrix](https://github.com/enthusiasticgeek/vani-matrix)
- **Checksum (0.1.0):** `0ab90c55…403448`
- **Checksum (0.2.0):** `9133b183…1877345`

```toml
[deps]
matrix = { registry = "kosh", version = "^0.2" }
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

**Version:** 0.4.2 &nbsp;|&nbsp; **Deps:** matrix ^0.1 (real registry dependency)

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
- **Checksum (0.4.2):** `0d1a729a…655680`

```toml
[deps]
probability = { registry = "kosh", version = "^0.4.2" }
```

---

## complex

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** none

Complex number library for the vāṇी compiler. `Complex` is a plain
`{ re: f64, im: f64 }` struct (no heap-owning fields, so it's freely copyable --
no `ref`/`mut ref` ceremony anywhere in this API, unlike the `Vec<f64>`-based
sibling libraries).

Includes arithmetic (add/sub/mul/div/scale/reciprocal), modulus/argument/polar
form, exponential/logarithm/integer-power/square-root, trigonometric and
hyperbolic functions, and roots of unity.

- **Repository:** [enthusiasticgeek/vani-complex](https://github.com/enthusiasticgeek/vani-complex)
- **Checksum (0.1.0):** `ff1b45a2…9371cf`

```toml
[deps]
complex = { registry = "kosh", version = "^0.1" }
```

---

## optimize

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (real registry dependency)

Numerical optimization library for the vāṇी compiler.

Includes unconstrained gradient-based methods (fixed-step and
Armijo-backtracking gradient descent, Newton's method with analytic or
finite-difference derivatives, coordinate descent), numerical differentiation
(gradient and Hessian via finite differences), closed-form and iterative
quadratic-objective solvers, a penalty-method constrained optimizer, and a
tableau simplex linear-programming solver.

- **Repository:** [enthusiasticgeek/vani-optimize](https://github.com/enthusiasticgeek/vani-optimize)
- **Checksum (0.1.0):** `56db6d36…44b48`

```toml
[deps]
optimize = { registry = "kosh", version = "^0.1" }
```

---

## geometry

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** none

Computational and analytic geometry library for the vāṇी compiler.
`Point2D`/`Point3D`/`Plane` are plain `f64`-field structs (freely copyable,
same convention as `complex`'s `Complex`).

Includes 2D/3D point and vector arithmetic, line/segment distance and
intersection, triangle/polygon area/perimeter/centroid, point-in-polygon,
convex hull (Andrew's monotone chain), brute-force closest pair, circumcircle
construction, conic-section classification (ellipse/parabola/hyperbola), 3D
planes and skew-line distance, and the law-of-cosines triangle angle.

- **Repository:** [enthusiasticgeek/vani-geometry](https://github.com/enthusiasticgeek/vani-geometry)
- **Checksum (0.1.0):** `372340c0…9818e86`

```toml
[deps]
geometry = { registry = "kosh", version = "^0.1" }
```

---

## signal

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** complex ^0.1 (real registry dependency)

Digital signal processing library for the vāṇी compiler. Time/sample-domain
data is `Vec<f64>`; frequency-domain data is `Vec<Complex>` (from `complex`).

Includes naive DFT/IDFT (any length), Cooley-Tukey radix-2 FFT/IFFT (power-of-2
length, with a real-input convenience wrapper), magnitude/power spectrum and
frequency-bin helpers, zero-padding utilities, linear/circular convolution,
cross-correlation, Hann/Hamming/Blackman/rectangular windowing, and numeric
(trapezoidal) Laplace and Z transforms.

- **Repository:** [enthusiasticgeek/vani-signal](https://github.com/enthusiasticgeek/vani-signal)
- **Checksum (0.1.0):** `225861fb…c7425817`

```toml
[deps]
signal = { registry = "kosh", version = "^0.1" }
```

---

## tensor

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (real registry dependency)

N-dimensional array library for the vāṇी compiler. A tensor is a flat
row-major `Vec<f64>` plus an explicit `Vec<i64>` shape (no hidden metadata) --
a rank-2 tensor's data is byte-for-byte the same layout as a `matrix` matrix.

Includes shape/stride/index utilities (with flatten/unflatten round-tripping),
construction (zeros/ones/full/copy), get/set indexing, reshape, elementwise
arithmetic and reductions (add/sub/mul/scale/sum/mean/max/min), last-axis
broadcasting, general N-D axis permutation, and tensor contraction
(implemented as a thin wrapper over `matrix`'s `mat_mul_rect`, since a
row-major tensor's data is already exactly the matrix that function expects).

- **Repository:** [enthusiasticgeek/vani-tensor](https://github.com/enthusiasticgeek/vani-tensor)
- **Checksum (0.1.0):** `e6c2475c…16f56a`

```toml
[deps]
tensor = { registry = "kosh", version = "^0.1" }
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

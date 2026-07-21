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

**Version:** 0.3.0 &nbsp;|&nbsp; **Deps:** none

Numerical calculus library for the vāṇी compiler.

Provides integration (trapezoidal, Simpson's, Romberg, Gauss-Legendre,
adaptive), differentiation (central/forward/second difference, gradient,
Jacobian, Hessian diagonal), root-finding (bisection, secant, Newton,
Brent), optimization (golden section, Brent, Newton), explicit ODE solvers
(Euler, RK4, RK45, Adams-Bashforth), implicit ODE solvers (backward Euler,
Crank-Nicolson -- A-stable, stay bounded on stiff systems where the
explicit solvers diverge, each step solved via Newton's method), an ODE
boundary-value-problem solver (shooting method: secant search over the
initial slope, paired RK4 for the underlying first-order system),
polynomial arithmetic, interpolation (Lagrange, linear table, natural
cubic spline), and numerically stable series summation (Kahan sum, running
mean).

- **Repository:** [enthusiasticgeek/vani-calculus](https://github.com/enthusiasticgeek/vani-calculus)
- **Checksum (0.1.0):** `4131d2cd…e626a3e`
- **Checksum (0.2.0):** `9cfe9db8…04ca6a`
- **Checksum (0.3.0):** `b3a728d6…de24b20e`

```toml
[deps]
calculus = { registry = "kosh", version = "^0.3" }
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

## pde

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (real registry dependency)

Finite-difference PDE solver library for the vāṇी compiler. Grids are flat
row-major `Vec<f64>` (1D: length `n`; 2D: length `nx*ny`, same layout as a
`matrix` matrix), Dirichlet boundary conditions only.

Includes 1D/2D Laplace-Poisson (elliptic, solved directly by assembling the
stencil into a dense matrix and calling `matrix`'s `mat_solve`), 1D/2D heat
(parabolic, explicit FTCS time marching with a stability-number helper), and
1D/2D wave (hyperbolic, explicit central-difference time marching with a
Courant-number helper and a Taylor-series first step). Every solver is
validated against a closed-form solution (harmonic polynomials for Laplace,
decaying/standing sine solutions for heat/wave), not just plausible-looking
output.

- **Repository:** [enthusiasticgeek/vani-pde](https://github.com/enthusiasticgeek/vani-pde)
- **Checksum (0.1.0):** `a1de34fd…7fdd3224`

```toml
[deps]
pde = { registry = "kosh", version = "^0.1" }
```

---

## algebra

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** matrix ^0.2, calculus ^0.2 (real registry dependencies)

Polynomial root-finding and nonlinear equation system library for the vāṇी
compiler. Polynomial coefficients are ascending `Vec<f64>`, matching
`calculus`'s `poly_eval`/`poly_deriv_coeffs`/`poly_mul` convention.

Includes a closed-form cubic real-root solver (Cardano's method), a general
real-root finder for any degree (degree >= 4 via companion matrix +
`matrix`'s `mat_eig_power` + synthetic polynomial deflation + Newton
polishing -- deliberately not a hand-derived quartic closed form, see the
package's own README), a `algebra_quartic_roots_real` convenience wrapper
over that same path, and Newton-Raphson solvers for systems of nonlinear
equations (analytic or finite-difference Jacobian, `matrix`'s `mat_solve`
at every step). Real roots only -- no complex-root support in v0.1.0.

- **Repository:** [enthusiasticgeek/vani-algebra](https://github.com/enthusiasticgeek/vani-algebra)
- **Checksum (0.1.0):** `c4f8f759…d42b169fdb`

```toml
[deps]
algebra = { registry = "kosh", version = "^0.1" }
```

---

## sparse

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (test/interop only, not used by src/lib.vani itself)

Sparse matrix format and operations library for the vāṇी compiler. Two
struct types: `SparseCOO` (easy to build incrementally) and `SparseCSR`
(the efficient format every operation works on); a CSR matrix's dense form
is byte-for-byte vani-matrix's row-major layout.

Includes COO/CSR/dense conversions (with duplicate-entry summing),
`sparse_csr_matvec` (O(nnz) matrix-vector product), transpose, scale, add,
and `sparse_csr_matmul` (sparse-sparse multiply via Gustavson's algorithm).
Every operation is cross-checked against the equivalent dense vani-matrix
operation on the same data.

- **Repository:** [enthusiasticgeek/vani-sparse](https://github.com/enthusiasticgeek/vani-sparse)
- **Checksum (0.1.0):** `2b0cd8e9…2e5b26e3b`

```toml
[deps]
sparse = { registry = "kosh", version = "^0.1" }
```

---

## vectorcalc

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** calculus ^0.2 (real registry dependency)

Vector calculus library for the vāṇी compiler: gradient, divergence, curl,
Laplacian (2D and 3D, central finite differences), and double/triple/line
integrals.

Multi-dimensional integration reuses `calculus`'s `integrate_simpson` via
pre-sampling instead of closures (vāṇी has no closures) -- see the
package's own README for how. Validated against hand-computed closed
forms plus two composed identities: `curl(grad f) = 0` for any scalar
field, and the 2D divergence theorem on a square (double integral of
divergence equals the sum of outward flux across all four edges).

- **Repository:** [enthusiasticgeek/vani-vectorcalc](https://github.com/enthusiasticgeek/vani-vectorcalc)
- **Checksum (0.1.0):** `280173fa…7024967d1ab1661`

```toml
[deps]
vectorcalc = { registry = "kosh", version = "^0.1" }
```

---

## discrete

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** none

Graph algorithms and combinatorics enumeration library for the vāṇी
compiler. Uses its own flat row-major `Vec<f64>` adjacency-matrix encoding
rather than the compiler's builtin `Graph` type, which is opaque from vāṇी
source (no accessor to enumerate edges/neighbors).

Includes all-pairs shortest path (Floyd-Warshall), strongly-connected
components (Kosaraju), max-flow/min-cut (Edmonds-Karp), bipartite matching
(Kuhn's algorithm), greedy graph coloring, permutation/combination/subset
enumeration (iterator-style, matching `std::next_permutation`), and integer
partition counting. Validated against textbook examples plus two composed
checks: the max-flow-min-cut theorem, and permutation/combination totals
cross-checked against the compiler's `i64_factorial`/`i64_binomial`.

- **Repository:** [enthusiasticgeek/vani-discrete](https://github.com/enthusiasticgeek/vani-discrete)
- **Checksum (0.1.0):** `7fba0d10…7a7c21ecf60`

```toml
[deps]
discrete = { registry = "kosh", version = "^0.1" }
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

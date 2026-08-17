# Package Catalog

All packages currently published to the Kosh registry.

---

## matrix

**Version:** 0.2.0 &nbsp;|&nbsp; **Deps:** none

Dense linear algebra library for the vāṇī compiler. Matrices are flat
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

**Version:** 0.3.1 &nbsp;|&nbsp; **Deps:** none

Numerical calculus library for the vāṇī compiler.

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

**Version:** 0.4.7 &nbsp;|&nbsp; **Deps:** matrix ^0.1 (real registry dependency)

Probability and statistics library for the vāṇī compiler.

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

**Version:** 0.1.2 &nbsp;|&nbsp; **Deps:** none

Complex number library for the vāṇī compiler. `Complex` is a plain
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

**Version:** 0.1.6 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (real registry dependency)

Numerical optimization library for the vāṇī compiler.

Includes unconstrained gradient-based methods (fixed-step and
Armijo-backtracking gradient descent, Newton's method with analytic or
finite-difference derivatives, coordinate descent), numerical differentiation
(gradient and Hessian via finite differences), closed-form and iterative
quadratic-objective solvers, a penalty-method constrained optimizer, and a
tableau simplex linear-programming solver.

- **Repository:** [enthusiasticgeek/vani-optimize](https://github.com/enthusiasticgeek/vani-optimize)
- **Checksum (0.1.0):** `56db6d36…44b48`
- **Checksum (0.1.6):** `22817b79…fcb2ed`

```toml
[deps]
optimize = { registry = "kosh", version = "^0.1" }
```

---

## geometry

**Version:** 0.1.2 &nbsp;|&nbsp; **Deps:** none

Computational and analytic geometry library for the vāṇī compiler.
`Point2D`/`Point3D`/`Plane` are plain `f64`-field structs (freely copyable,
same convention as `complex`'s `Complex`).

Includes 2D/3D point and vector arithmetic, line/segment distance and
intersection, triangle/polygon area/perimeter/centroid, point-in-polygon,
convex hull (Andrew's monotone chain), brute-force closest pair, circumcircle
construction, conic-section classification (ellipse/parabola/hyperbola), 3D
planes and skew-line distance, and the law-of-cosines triangle angle.
`circumcenter` and `convex_hull` reject degenerate input (collinear points,
fewer than 3 points) with a clean assertion rather than returning a
meaningless result (0.1.2).

- **Repository:** [enthusiasticgeek/vani-geometry](https://github.com/enthusiasticgeek/vani-geometry)
- **Checksum (0.1.2):** `e423941b…3a1a0d7`

```toml
[deps]
geometry = { registry = "kosh", version = "^0.1" }
```

---

## signal

**Version:** 0.1.5 &nbsp;|&nbsp; **Deps:** complex ^0.1 (real registry dependency)

Digital signal processing library for the vāṇī compiler. Time/sample-domain
data is `Vec<f64>`; frequency-domain data is `Vec<Complex>` (from `complex`).

Includes naive DFT/IDFT (any length), Cooley-Tukey radix-2 FFT/IFFT (power-of-2
length, with a real-input convenience wrapper), magnitude/power spectrum and
frequency-bin helpers, zero-padding utilities, linear/circular convolution,
cross-correlation, Hann/Hamming/Blackman/Bartlett/Kaiser/Tukey/rectangular
windowing, and numeric (trapezoidal) Laplace and Z transforms.

- **Repository:** [enthusiasticgeek/vani-signal](https://github.com/enthusiasticgeek/vani-signal)
- **Checksum (0.1.4):** `43fb2c64…a7a3293`
- **Checksum (0.1.5):** `ace61620…59f111`

```toml
[deps]
signal = { registry = "kosh", version = "^0.1" }
```

---

## tensor

**Version:** 0.1.4 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (real registry dependency)

N-dimensional array library for the vāṇī compiler. A tensor is a flat
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
- **Checksum (0.1.4):** `567bf7a0…e2e777`

```toml
[deps]
tensor = { registry = "kosh", version = "^0.1" }
```

---

## pde

**Version:** 0.1.4 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (real registry dependency)

Finite-difference PDE solver library for the vāṇī compiler. Grids are flat
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
- **Checksum (0.1.4):** `34e7fccf…daa4c3`

```toml
[deps]
pde = { registry = "kosh", version = "^0.1" }
```

---

## algebra

**Version:** 0.1.4 &nbsp;|&nbsp; **Deps:** matrix ^0.2, calculus ^0.2 (real registry dependencies)

Polynomial root-finding and nonlinear equation system library for the vāṇī
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
- **Checksum (0.1.4):** `14ab167a…9d7eac`

```toml
[deps]
algebra = { registry = "kosh", version = "^0.1" }
```

---

## sparse

**Version:** 0.1.4 &nbsp;|&nbsp; **Deps:** matrix ^0.2 (test/interop only, not used by src/lib.vani itself)

Sparse matrix format and operations library for the vāṇī compiler. Two
struct types: `SparseCOO` (easy to build incrementally) and `SparseCSR`
(the efficient format every operation works on); a CSR matrix's dense form
is byte-for-byte vani-matrix's row-major layout.

Includes COO/CSR/dense conversions (with duplicate-entry summing),
`sparse_csr_matvec` (O(nnz) matrix-vector product), transpose, scale, add,
and `sparse_csr_matmul` (sparse-sparse multiply via Gustavson's algorithm).
`sparse_csr_get` looks up a single element via binary search within its row
(0.1.3), relying on `col_idx`'s existing per-row-sorted invariant. Every
operation is cross-checked against the equivalent dense vani-matrix
operation on the same data.

- **Repository:** [enthusiasticgeek/vani-sparse](https://github.com/enthusiasticgeek/vani-sparse)
- **Checksum (0.1.3):** `b3d59282…366a4c0`

```toml
[deps]
sparse = { registry = "kosh", version = "^0.1" }
```

---

## vectorcalc

**Version:** 0.1.4 &nbsp;|&nbsp; **Deps:** calculus ^0.2 (real registry dependency)

Vector calculus library for the vāṇī compiler: gradient, divergence, curl,
Laplacian (2D and 3D, central finite differences), and double/triple/line
integrals.

Multi-dimensional integration reuses `calculus`'s `integrate_simpson` via
pre-sampling instead of closures (vāṇī has no closures) -- see the
package's own README for how. Validated against hand-computed closed
forms plus two composed identities: `curl(grad f) = 0` for any scalar
field, and the 2D divergence theorem on a square (double integral of
divergence equals the sum of outward flux across all four edges).

- **Repository:** [enthusiasticgeek/vani-vectorcalc](https://github.com/enthusiasticgeek/vani-vectorcalc)
- **Checksum (0.1.0):** `280173fa…7024967d1ab1661`
- **Checksum (0.1.4):** `68f88155…ea3995`

```toml
[deps]
vectorcalc = { registry = "kosh", version = "^0.1" }
```

---

## discrete

**Version:** 0.1.3 &nbsp;|&nbsp; **Deps:** none

Graph algorithms and combinatorics enumeration library for the vāṇī
compiler. Uses its own flat row-major `Vec<f64>` adjacency-matrix encoding
rather than the compiler's builtin `Graph` type, which is opaque from vāṇī
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
- **Checksum (0.1.3):** `47d2ba94…4d2b62`

```toml
[deps]
discrete = { registry = "kosh", version = "^0.1" }
```

---

## interval

**Version:** 0.1.4 &nbsp;|&nbsp; **Deps:** none

Rigorous interval arithmetic and first-order error propagation for
the vāṇī compiler. Two distinct techniques share this package: interval
arithmetic (`iv_*`), an `[lo, hi]` range provably containing the true
value; and error propagation (`ep_*`), the linearized
`σ_f ≈ sqrt(Σ(∂f/∂xi)²σxi²)` formula, which reuses `calculus`'s
`diff_central` for the single-variable case.

Includes core interval arithmetic (add/sub/mul/div/reciprocal/neg),
interval elementary functions (sqrt/exp/log/integer-power/sin/cos),
set operations (contains/width/midpoint/intersect/union-hull), and a
multi-candidate interval-bisection root-finder (`iv_bisect_root`
returns `Vec<Interval>`, not a single `Interval` -- a single-bracket
version isn't actually rigorous once interval arithmetic's
"dependency problem" is accounted for; see the package's own README).
Error propagation covers single-variable, n-variable independent, and
n-variable-with-covariance forms, plus closed-form shortcuts for
sums/products/quotients.

- **Repository:** [enthusiasticgeek/vani-interval](https://github.com/enthusiasticgeek/vani-interval)
- **Checksum (0.1.0):** `f93ca7f2…f6f7ef36f`
- **Checksum (0.1.1):** `e79dfac2…08d0b43f3`
- **Checksum (0.1.2):** `1d004b40…2897cc227`
- **Checksum (0.1.3):** `104ce8cf…16540ed92`
- **Checksum (0.1.4):** `3579ca9b…9b6c74`

```toml
[deps]
interval = { registry = "kosh", version = "^0.1" }
```

---

## bignum

**Version:** 0.2.0 &nbsp;|&nbsp; **Deps:** none

Arbitrary-precision integer library for the vāṇī compiler -- numeric
foundation for the symbolic-math tier (`symbolic`, below). `BigInt` owns a `Vec<i64>`
of base-1,000,000,000 (1e9) digit limbs (not Copy, unlike `complex`'s
`Complex`), chosen because vāṇī has no integer type wider than `i64` and
`+`/`-`/`*` trap on overflow rather than wrap.

Includes construction/IO (from `i64`, from a decimal string, to a decimal
string, to `i64`), sign/predicates, comparison (`bn_cmp` plus wrappers,
and `implement Eq for BigInt` so `==`/`!=` work directly), arithmetic
(add/sub/mul -- schoolbook multiply with immediate per-product carry
propagation, the overflow-safety-critical design choice), truncating
division/modulo (long division, binary-searching each quotient digit
against the running remainder), GCD (Euclidean algorithm), and
exponentiation -- `bn_pow_i64` (squaring) and `bn_pow_mod` (modular,
reducing after every multiply so intermediates stay bounded by the
modulus) (0.1.2). A full `Rational` type as of v0.2.0: `struct Rational
{ num: BigInt, den: BigInt }`, always kept in lowest terms via a smart
constructor (`bn_gcd` reduction + sign normalization onto the
denominator), with arithmetic, comparison, and string conversion.

- **Repository:** [enthusiasticgeek/vani-bignum](https://github.com/enthusiasticgeek/vani-bignum)
- **Checksum (0.1.2):** `f16d692c…e4e12b2`
- **Checksum (0.2.0):** `28a11584…82276b`

```toml
[deps]
bignum = { registry = "kosh", version = "^0.2" }
```

---

## symbolic

**Version:** 0.7.0 &nbsp;|&nbsp; **Deps:** algebra ^0.1 (real registry
dependency, equation solving + factorization); calculus ^0.3
(tests/examples-only cross-check, not a production dependency)

Symbolic-math (CAS) foundation for the vāṇī compiler -- expression
construction, numeric evaluation, precedence-aware printing,
simplification, differentiation, equation solving, integration, and
polynomial factorization. The full roadmap shipped 2026-08-16.
`ExprNode` is a Copy struct (four `i64` fields) living in a flat
`Vec<ExprNode>` arena with `i64` child indices instead of pointers,
chosen because a recursive `Box<Self>` enum doesn't compile in vāṇī
today (single-payload-field enum restriction plus no self-referential
`box()` payloads) -- the same workaround `ml`'s autodiff `GraphNode`
arena reuses.

Includes construction (`sym_num`/`sym_var`/`sym_add`/`sym_sub`/`sym_mul`/
`sym_div`/`sym_pow`/`sym_neg`, symbol-table interning), introspection
(`sym_kind`/`sym_is_leaf`), numeric evaluation (`sym_eval`, nonnegative-
integer `Pow` exponents only), a precedence-aware pretty-printer
(`sym_to_str`, matching SymPy's `str()` spacing convention), structural
equality (`sym_eq_structural`, same-shape only -- not commutative),
simplification (`sym_simplify`: constant folding + identities for
`Mul`/`Div`/`Pow`/`Neg`, plus flatten-and-collect like-term collection
for `Add`/`Sub` -- deliberately scoped to flat linear combinations of
monomials, not general polynomial normal form), and symbolic
differentiation (`sym_diff`: sum/product/quotient/power+chain rules,
one per node kind, cross-checked against `calculus::diff_central`).

Equation solving (`sym_solve_poly_le2`/`sym_solve_equation_le2`, degree
<=2): extracts polynomial coefficients from an expression tree, then
reuses `algebra::algebra_poly_roots_real` rather than reimplementing a
closed-form solver. Polynomial integration (`sym_integrate`): a fixed
pattern table over the power rule, linearity, constant-multiple/divisor
rules, and linear `u`-substitution (`(ax+b)^n`), scoped to polynomials
only -- not the transcendental (`exp`/`ln`/`sin`/`cos`) antiderivatives
originally proposed, since `ExprNode`'s kind set never grew past its
original 8 kinds. Validated via the fundamental theorem of calculus
(`sym_diff(sym_integrate(f))` evaluated against `f` at sample points).
Polynomial factorization (`poly_rational_roots`/`poly_factor_rational`,
plus symbolic-tree wrappers `sym_poly_coeffs`/`sym_factor_rational`):
rational-root theorem via exact integer arithmetic (zero floating-point
error in detection) plus synthetic division, reusing
`algebra::algebra_poly_deflate` for repeated-root multiplicity and
whatever's left over. Gröbner bases and non-rational factorization stay
out of scope -- this folds in what would have been a separate
`vani-polyalgebra` repo.

Every simplification/differentiation phase was validated via property-
based random-sampling or cross-package numeric checks
(`seed_rng`/`rand_in_range`, `calculus::diff_central`), not just
hand-picked examples.

- **Repository:** [enthusiasticgeek/vani-symbolic](https://github.com/enthusiasticgeek/vani-symbolic)
- **Checksum (0.7.0):** `a9574c5c…4b8ff2fd`

```toml
[deps]
symbolic = { registry = "kosh", version = "^0.7" }
```

---

## ml

**Version:** 0.6.0 &nbsp;|&nbsp; **Deps:** probability ^0.4, optimize ^0.1 (real registry dependencies)

Machine learning library for the vāṇī compiler. Staged: classical ML
first (v0.1.0-v0.2.0), then a full autodiff/neural-net engine on top
(v0.3.0-v0.6.0) -- the whole roadmap now shipped.

Classical ML: linear regression (thin wrapper over `probability::mlr_*`),
logistic regression (own gradient-descent loop -- `optimize`'s fixed
function-pointer signature can't thread training data through without a
ref-capturing closure), k-means clustering (Lloyd's algorithm), train/test
split, core metrics. Data utilities (v0.2.0): feature scaling, one-hot
encoding, k-fold cross-validation.

Autodiff core (v0.3.0): a flat-arena computation graph (`GraphNode` +
`i64` child indices, same workaround `symbolic` uses for its expression
tree) with reverse-mode automatic differentiation -- both the forward
and backward passes are single linear loops (no recursion, no
topological sort needed, since the append-only arena is already in
topological order by construction). Validated via finite-difference
gradient checking cross-checked against `calculus::diff_central`,
including a dedicated test for correct gradient accumulation when one
value is used by multiple graph nodes.

Layers/activations/losses (v0.4.0): `relu`/`sigmoid`/`tanh`/`log` as new
graph node kinds; dense layers and MSE/binary-cross-entropy losses
composed from existing primitives rather than new kinds (matmul emerges
from scalar-op composition -- no `matrix`/`tensor` dependency needed for
any of this).

Optimizers (v0.5.0): SGD, Momentum, Adam over the graph's parameter
vector, each returning a fresh state struct per step.

v0.6.0 closes the roadmap with a full worked example: a 2-2-1 MLP
trained on XOR (the canonical problem a single dense+sigmoid layer
provably cannot solve), converging from ~0.72 to ~0.0001 loss over 3000
Adam epochs, identical on both backends -- real end-to-end proof the
whole stack composes correctly.

- **Repository:** [enthusiasticgeek/vani-ml](https://github.com/enthusiasticgeek/vani-ml)
- **Checksum (0.6.0):** `0689a73b…77d7aa4`
- **Checksum (0.5.0):** `e16747c8…19d8d96`
- **Checksum (0.4.0):** `0fa33001…9add90ef6`
- **Checksum (0.3.0):** `aeabc328…be31aa`
- **Checksum (0.1.0):** `7dab3082…480880`

```toml
[deps]
ml = { registry = "kosh", version = "^0.6" }
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

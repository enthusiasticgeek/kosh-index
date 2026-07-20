# Kosh Math Ecosystem Roadmap

This tracks which **packages** should exist to give vāṇी broad mathematical library
coverage — comparable to SciPy / Eigen / Armadillo / Boost.Math for the numeric tier,
with an optional, much larger symbolic tier on top. `catalog.md` lists what's actually
published right now; this file is the forward-looking plan.

Compiler-level items that this roadmap depends on (if any) are tracked in
[vani-compiler's docs/TODO_CURRENT.md](https://github.com/enthusiasticgeek/vani-compiler/blob/main/docs/TODO_CURRENT.md)
under "Kosh math-library ecosystem" and cross-linked from here.

Last updated: 2026-07-20

---

## Already published

| Package | Version | Domain |
|---|---|---|
| [matrix](https://github.com/enthusiasticgeek/vani-matrix) | 0.2.0 | Dense linear algebra: construction, arithmetic, multiply, closed-form 2×2/3×3, Gauss-Jordan inverse, LU, Cholesky, eigenvalues (power iteration + deflation), condition number, Householder QR, SVD bidiagonalization |
| [calculus](https://github.com/enthusiasticgeek/vani-calculus) | 0.2.0 | Integration, differentiation, root-finding, 1D optimization, ODE solvers, polynomials, interpolation, series |
| [probability](https://github.com/enthusiasticgeek/vani-probability) | 0.4.2 | Descriptive/inferential stats, distributions, Bayesian inference, Markov chains, time series, CDFs/p-values, MLR, PCA, stochastic processes |
| [complex](https://github.com/enthusiasticgeek/vani-complex) | 0.1.0 | Complex numbers: arithmetic, polar form, exp/log/power/sqrt, trig/hyperbolic, roots of unity |
| [optimize](https://github.com/enthusiasticgeek/vani-optimize) | 0.1.0 | Numerical optimization: gradient descent (fixed/backtracking), Newton's method (analytic/finite-difference), coordinate descent, quadratic solvers, penalty-method constrained optimization, tableau simplex LP |
| [geometry](https://github.com/enthusiasticgeek/vani-geometry) | 0.1.0 | Computational + analytic geometry: 2D/3D point/vector arithmetic, line/segment distance and intersection, polygon area/perimeter/centroid, point-in-polygon, convex hull, closest pair, circumcircle, conic classification, 3D planes and skew-line distance |
| [signal](https://github.com/enthusiasticgeek/vani-signal) | 0.1.0 | Signal processing: naive DFT/IDFT, Cooley-Tukey radix-2 FFT/IFFT, magnitude/power spectrum and frequency-bin helpers, zero-padding, linear/circular convolution, cross-correlation, Hann/Hamming/Blackman windowing, numeric Laplace/Z transforms |
| [tensor](https://github.com/enthusiasticgeek/vani-tensor) | 0.1.0 | N-dimensional arrays: flat Vec<f64> + explicit shape encoding, shape/stride/index utilities, construction, reshape, elementwise arithmetic/reductions, last-axis broadcasting, general N-D axis permutation, contraction (via matrix's mat_mul_rect) |
| [pde](https://github.com/enthusiasticgeek/vani-pde) | 0.1.0 | Finite-difference PDE solvers: 1D/2D Laplace-Poisson (elliptic, via matrix's mat_solve), 1D/2D heat (parabolic, explicit FTCS), 1D/2D wave (hyperbolic, explicit central-difference), Dirichlet BCs only |

## Already covered by vani-compiler builtins (no package needed)

Confirmed present in `checker.rs`'s builtin allowlist -- do not duplicate these in any
new package:

- **Number theory**: `i64_gcd`, `i64_lcm`, `i64_is_prime`, `i64_next_prime`,
  `i64_prev_prime`, `i64_totient`, `i64_divisor_count`, `i64_divisor_sum`,
  `i64_mod_inverse`, `i64_is_perfect_square`
- **Combinatorics**: `i64_factorial`, `i64_binomial`, `i64_perm`, `i64_fibonacci`
- **Graph algorithms**: `graph_new`/`add_edge`/`bfs_reach`/`dfs_reach`/`dijkstra`/
  `has_cycle`/`mst_kruskal`/`mst_prim`/`astar`/`topo_sort`
- **Special functions**: `f64_erf`/`erfc`/`tgamma`/`lgamma`, `f64_quadratic_root`
- Generic data structures: heaps, tries, union-find, skip lists, bloom filters, BSTs

This is why "elementary math", "number theory", and most of "discrete math" from a
naive coverage table don't need their own repos -- they're already in the language.

---

## Gap analysis — field by field

The full picture against a standard "what does a scientific-computing stack cover"
breakdown (elementary → graduate-level applied math). Anything marked ✅ needs no new
work; 🟡 is partially covered by an existing package; ❌ needs a new repo.

| Field | Status | Repo |
|---|---|---|
| Elementary mathematics | ✅ done | builtin |
| Number theory | ✅ done | builtin |
| Discrete math (graphs, basic combinatorics) | ✅ mostly done | builtin |
| Algebra (equations, polynomial roots) | 🟡 partial (poly ops in vani-calculus) | extend vani-calculus, or new **vani-algebra** |
| Linear algebra — dense | ✅ done | vani-matrix |
| Linear algebra — eigenvalues/QR/SVD (dense) | ✅ done (v0.2.0) | vani-matrix |
| Linear algebra — sparse matrices | ❌ not done | new **vani-sparse**, or extend vani-matrix further |
| Calculus — single-variable/1D | ✅ done | vani-calculus |
| Calculus — vector (div/curl/multi-integral) | ❌ not done | extend vani-calculus, or new **vani-vectorcalc** |
| Differential equations — ODE | ✅ done | vani-calculus |
| Differential equations — PDE | ✅ done (v0.1.0) | vani-pde |
| Complex analysis | ✅ done (v0.1.0) | vani-complex |
| Numerical analysis | ✅ mostly done | vani-calculus |
| Probability | ✅ done | vani-probability |
| Statistics | ✅ done | vani-probability |
| Optimization — 1D | ✅ done | vani-calculus |
| Optimization — multivariable/constrained/convex/LP | ✅ done (v0.1.0) | vani-optimize |
| Geometry (computational + analytic) | ✅ done (v0.1.0) | vani-geometry |
| Fourier/signal processing (FFT/DFT/Laplace/Z) | ✅ done (v0.1.0) | vani-signal |
| Tensor math (N-D beyond matrices) | ✅ done (v0.1.0) | vani-tensor |
| Scientific computing (aggregate) | ✅ substantially covered | vani-matrix + vani-calculus + vani-probability |

### What's out of scope for this roadmap (research-tier math)

Abstract algebra (groups/rings/fields), category theory, topology, differential
geometry, algebraic geometry, functional analysis, measure theory, Lie groups/algebras,
representation theory, algebraic topology, noncommutative geometry, tensor calculus for
general relativity, exterior algebra/differential forms, Clifford/geometric algebra.
These serve much narrower audiences than the core scientific stack below and aren't
planned here — flag if a real use case shows up.

---

## Planned: numeric/scientific tier

Ordered by recommended build sequence (earlier entries unblock later ones).

| # | Repo | Depends on | Scope | Rough size |
|---|---|---|---|---|
| 1 | ~~**matrix v0.2**~~ ✅ shipped 2026-07-20 | -- | Eigenvalues (power iteration + deflation), QR (Householder), SVD (bidiagonalization), condition number. | 5 functions |
| 2 | ~~**vani-complex**~~ ✅ shipped 2026-07-20 | -- | `Complex { re: f64, im: f64 }` struct + arithmetic, polar form, complex exp/log/trig, roots of unity. | 24 functions |
| 3 | ~~**vani-optimize**~~ ✅ shipped 2026-07-20 | -- | Multivariable unconstrained (fixed-step and Armijo-backtracking gradient descent, Newton's method with analytic or finite-difference derivatives, coordinate descent), quadratic specialty solvers, constrained (penalty method), linear programming (tableau simplex). | 20 functions |
| 4 | ~~**vani-geometry**~~ ✅ shipped 2026-07-20 | -- | Computational: convex hull (Andrew's monotone chain), closest pair, point-in-polygon, segment intersection. Analytic: circumcircle, conic-section classification, 2D/3D distance/angle formulas, 3D planes and skew-line distance. | 38 functions |
| 5 | ~~**vani-signal**~~ ✅ shipped 2026-07-20 | vani-complex | FFT/DFT (Cooley-Tukey radix-2), convolution/correlation, numeric Laplace/Z transforms, windowing functions. | 21 functions |
| 6 | ~~**vani-tensor**~~ ✅ shipped 2026-07-20 | matrix | N-dimensional arrays: flat `Vec<f64>` + shape `Vec<i64>` encoding (matching vani-matrix's row-major convention, not nested `Vec<Vec<...>>`), reshape, broadcast, contraction, N-D elementwise ops. | 23 functions |
| 7 | ~~**vani-pde**~~ ✅ shipped 2026-07-20 | matrix | Finite-difference solvers for classic PDEs (heat/wave/Laplace equation) on a 1D/2D grid, Dirichlet BCs, explicit time marching for heat/wave, direct dense solve via mat_solve for Laplace/Poisson. Shipped without a vani-calculus dependency -- no natural reuse point was found (see vani-pde's README "Design decisions"). | 21 functions |
| 8 | **vani-algebra** (new, lower priority) | calculus (reuses poly_* ops) | Polynomial root-finding beyond quadratic (cubic/quartic closed forms, numeric for higher degree via companion-matrix eigenvalues -- needs #1), linear/nonlinear equation systems. | ~15-20 functions |

## Planned: symbolic tier (optional, separate scope)

This is **not** a scaled-up version of the numeric tier -- it's a qualitatively
different problem. Correctness bugs compound silently (a wrong simplification rule
poisons everything built on top of it), and the design surface (expression
representation, canonicalization, rule ordering) needs to be settled before most
functions can even be written. Treat this as optional and budget it separately from
the numeric tier above.

| Repo | Depends on | Scope |
|---|---|---|
| **vani-bignum** | -- | Arbitrary-precision integers and rationals (digit-array + carry/borrow arithmetic in pure vāṇी, no compiler support needed). Numeric foundation everything else in this tier needs. |
| **vani-symbolic** | vani-bignum | Expression trees, simplification rules, symbolic differentiation/integration, equation solving. |
| **vani-polyalgebra** | vani-bignum, vani-symbolic | Polynomial factorization, Gröbner bases. Could fold into vani-symbolic instead of being standalone. |

If the goal is SciPy/Eigen/Boost.Math-class coverage, the numeric tier above is the
whole job. If the goal is closer to Mathematica/Maple/SageMath, the symbolic tier is
required on top -- and is a fundamentally larger undertaking than everything else in
this document combined.

---

## Effort estimates

Calibrated against what's actually happened building the three published packages --
vani-probability alone went from ~42 functions to ~90+ across four version bumps, each
with hand-verified reference values, tests, examples, and docs, plus two real compiler
bugs found and fixed along the way, all within a bounded body of work.

| Tier | Repos | Rough shape per repo | Relative effort |
|---|---|---|---|
| **Matrix extension** | eigenvalues/QR/SVD in vani-matrix | ~8-10 functions, numerically fussier than what's there (iterative eigensolvers are easy to get subtly wrong) | 0.5–1 unit |
| **New numeric repos** | vani-complex, vani-optimize, vani-geometry, vani-signal | ~25-40 functions each, same validate-against-known-values discipline as the existing 3 repos | ~1 unit each |
| **Bigger numeric repos** | vani-tensor, vani-pde | Wider design surface (N-D indexing scheme; PDE needs a discretization strategy decision up front) | ~1.5–2 units each |
| **CAS tier** | vani-bignum, vani-symbolic, vani-polyalgebra | Open-ended -- correctness bugs are subtle and compound (a wrong simplification rule silently poisons everything built on it); real CAS projects are multi-year efforts even at small scale | Not comparable to the above; expect several units minimum for a minimal symbolic core, and treat "done" as aspirational |

"Unit" here is a relative measure, not a wall-clock estimate -- a lot of the effort in
each published package so far was validation, compiler-bug archaeology, and doc/registry
mechanics rather than raw function-writing, and that ratio should hold for the planned
repos too.

---

## Recommended order

1. ~~**matrix v0.2** (eigen/QR/SVD)~~ ✅ shipped 2026-07-20 -- unblocks #6 and #8.
2. ~~**vani-complex**~~ ✅ shipped 2026-07-20 -- unblocks #5.
3. ~~**vani-optimize**~~ ✅ shipped 2026-07-20 and ~~**vani-geometry**~~ ✅ shipped 2026-07-20 -- both independent of each other and of everything above.
4. ~~**vani-signal**~~ ✅ shipped 2026-07-20 (needed #2) and ~~**vani-tensor**~~ ✅ shipped 2026-07-20 (needed #1).
5. ~~**vani-pde**~~ ✅ shipped 2026-07-20 -- used matrix's mat_solve for the elliptic solvers; calculus's ODE machinery didn't end up fitting (see vani-pde README).
6. **vani-algebra** -- lowest priority; niche once the above exist. **Next up.**
7. Symbolic tier only if full Mathematica/SageMath-class capability is actually wanted -- start with **vani-bignum**, since vani-symbolic can't do much without exact arithmetic underneath it.

---

## Notes for whoever picks up a repo from this list

- Match the established conventions: flat `Vec<f64>` encodings with explicit dimension
  args (not nested Vecs or hidden metadata), `#[bounded_stack(bytes=N)]` on every
  function with the budget set to `vanic check`'s **exact** reported worst-case (hand
  estimates are consistently wrong on the first try -- this has held for every package
  published so far), and validate every function against a hand-computed or
  textbook reference value before writing it into the library.
- Add new dependencies via `vanic add <name>` (vendors + checksum-verifies against the
  registry into `./vendor/<name>`), not a hand-copied bundle -- `vanic publish`
  correctly includes `vendor/` in the tarball as of vani-compiler commit `5732ba4`.
- `authors` in `vani.toml` must be a plain quoted string (`authors = "name"`), not a
  TOML array -- the manifest parser used by `vanic publish` doesn't support arrays.
- Validate functions **together**, not just in isolation. matrix v0.2's
  `mat_eig_power` passed every isolated unit test (positive/negative/symmetric
  eigenvalues) but had a real bug -- a uniform starting vector can land exactly on a
  *non-dominant* eigenvector of a symmetric matrix by construction -- that only
  surfaced once `mat_eig_deflate`'s output was fed back into it in an example. Write
  the example that chains functions together before calling a module done, not after.
- Not every package needs the flat-`Vec<f64>` convention -- vani-complex confirmed
  structs without heap-owning fields are `Copy` (freely reusable by value, no
  `ref`/`mut ref` needed) and that `Vec<StructType>` works with zero compiler changes.
  Use a real struct (vani-geometry's `Point`, vani-optimize's result type, etc.) where
  the data is naturally a small fixed record rather than forcing it into flat `Vec<f64>`
  + an indexing convention.

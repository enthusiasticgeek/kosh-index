# Kosh Math Ecosystem Roadmap

This tracks which **packages** should exist to give vāṇी broad mathematical library
coverage — comparable to SciPy / Eigen / Armadillo / Boost.Math for the numeric tier,
with an optional, much larger symbolic tier on top. `catalog.md` lists what's actually
published right now; this file is the forward-looking plan.

Compiler-level items that this roadmap depends on (if any) are tracked in
[vani-compiler's docs/TODO_CURRENT.md](https://github.com/enthusiasticgeek/vani-compiler/blob/main/docs/TODO_CURRENT.md)
under "Kosh math-library ecosystem" and cross-linked from here.

Last updated: 2026-07-21

**Status: the numeric/scientific tier (all 12 packages below) is complete,
every row in the gap-analysis table below is ✅, and every itemized gap
within those rows (G1-G7, N1-N2, and now N3) is shipped.** What remains
is only the optional, much larger symbolic tier — see "Planned: symbolic
tier" below.

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
| [algebra](https://github.com/enthusiasticgeek/vani-algebra) | 0.1.0 | Polynomial root-finding (closed-form cubic; degree >= 4 via companion matrix + mat_eig_power + synthetic deflation + Newton polish, real roots only) and nonlinear equation systems (Newton-Raphson, analytic/finite-difference Jacobian, via mat_solve) |
| [sparse](https://github.com/enthusiasticgeek/vani-sparse) | 0.1.0 | Sparse matrices: COO (build) / CSR (operate) formats, dense conversions, matvec, transpose, scale, add, matmul (Gustavson's algorithm); every op cross-checked against the equivalent dense vani-matrix operation |
| [vectorcalc](https://github.com/enthusiasticgeek/vani-vectorcalc) | 0.1.0 | Vector calculus: gradient/divergence/curl/Laplacian (2D/3D, central differences), double/triple/line integrals via nested reuse of calculus's integrate_simpson; composed checks for curl(grad f)=0 and the 2D divergence theorem |
| [discrete](https://github.com/enthusiasticgeek/vani-discrete) | 0.1.0 | Graph algorithms (Floyd-Warshall, Kosaraju SCC, Edmonds-Karp max-flow/min-cut, Kuhn's bipartite matching, greedy coloring) and combinatorics enumeration (permutations, combinations, subsets, partition counting), own adjacency-matrix encoding |
| [interval](https://github.com/enthusiasticgeek/vani-interval) | 0.1.0 | Rigorous interval arithmetic (core arithmetic, elementary functions, set ops, interval-bisection root-finding returning every surviving candidate bracket) and first-order error propagation (single-var, n-var independent/covariance, closed-form sum/product/quotient shortcuts) |

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
| Discrete math (graphs, basic combinatorics) | ✅ done¹ (v0.1.0) | builtin + vani-discrete |
| Algebra (equations, polynomial roots) | ✅ done (v0.1.0, real roots only) | vani-algebra |
| Linear algebra — dense | ✅ done | vani-matrix |
| Linear algebra — eigenvalues/QR/SVD (dense) | ✅ done (v0.2.0) | vani-matrix |
| Linear algebra — sparse matrices | ✅ done (v0.1.0) | vani-sparse |
| Calculus — single-variable/1D | ✅ done | vani-calculus |
| Calculus — vector (div/curl/multi-integral) | ✅ done (v0.1.0) | vani-vectorcalc |
| Differential equations — ODE | ✅ done | vani-calculus |
| Differential equations — PDE | ✅ done (v0.1.0) | vani-pde |
| Complex analysis | ✅ done (v0.1.0) | vani-complex |
| Numerical analysis | ✅ done² | vani-calculus + vani-interval |
| Probability | ✅ done | vani-probability |
| Statistics | ✅ done | vani-probability |
| Optimization — 1D | ✅ done | vani-calculus |
| Optimization — multivariable/constrained/convex/LP | ✅ done (v0.1.0) | vani-optimize |
| Geometry (computational + analytic) | ✅ done (v0.1.0) | vani-geometry |
| Fourier/signal processing (FFT/DFT/Laplace/Z) | ✅ done (v0.1.0) | vani-signal |
| Tensor math (N-D beyond matrices) | ✅ done (v0.1.0) | vani-tensor |
| Scientific computing (aggregate) | ✅ substantially covered³ | vani-matrix + vani-calculus + vani-probability |

### ¹²³ Known gaps within "mostly done" / "substantially covered" rows

Three rows above are marked done with a qualifier rather than a plain ✅ —
this section spells out exactly what's still missing under each, as a
tracked TODO list distinct from the (already-tracked, already-shipped)
items elsewhere in this document.

**¹ Discrete math (graphs, basic combinatorics)** — builtin coverage:
BFS/DFS/Dijkstra/A*/cycle-detection/MST(Kruskal+Prim)/topo-sort (graphs),
factorial/binomial/perm/fibonacci (combinatorics, counting only). **All
7 gaps below shipped in vani-discrete v0.1.0 (2026-07-20)**, on the
package's own adjacency-matrix encoding (the builtin `Graph` type is
opaque from vāṇी source, so these couldn't be built on top of it):

| # | Gap | Notes |
|---|---|---|
| G1 | ~~All-pairs shortest path~~ ✅ `disc_floyd_warshall` | Floyd-Warshall, not Johnson's |
| G2 | ~~Strongly-connected components~~ ✅ `disc_scc_kosaraju` | Kosaraju (two iterative DFS passes + transpose), not Tarjan's |
| G3 | ~~Max-flow / min-cut~~ ✅ `disc_max_flow` / `disc_min_cut_nodes` | Edmonds-Karp, not Dinic's; min-cut verified against the max-flow-min-cut theorem in tests |
| G4 | ~~Bipartite matching~~ ✅ `disc_bipartite_matching` | Kuhn's augmenting-path algorithm, not Hopcroft-Karp |
| G5 | ~~Graph coloring~~ ✅ `disc_greedy_coloring` | greedy by node index -- a valid coloring, not a minimum-color one (exact coloring is NP-hard) |
| G6 | ~~Permutation/combination/subset enumeration~~ ✅ `disc_next_permutation` / `disc_next_combination` / `disc_subset_from_bitmask` | iterator-style (`std::next_permutation` convention), not materialized into a nested `Vec<Vec<i64>>` |
| G7 | ~~Integer partition functions~~ ✅ `disc_partition_count` | counting (p(n) via O(n²) DP) only, not enumeration -- see vani-discrete README for why |

**² Numerical analysis** — covered: integration (trapz/Simpson/Romberg/
Gauss-Legendre 5-point/adaptive), differentiation (central/forward/second,
1D gradient/Jacobian/Hessian-diag), root-finding (bisection/secant/Newton/
Brent), 1D optimization (golden-section/Brent/Newton), ODE (Euler/RK4/
RK45/Adams-Bashforth-2 explicit, **plus backward Euler/Crank-Nicolson
implicit and a shooting-method BVP solver as of v0.3.0**), polynomials,
interpolation (Lagrange/linear-table/natural-cubic-spline), stable
summation, and (as of vani-interval v0.1.0) rigorous interval arithmetic
plus first-order error propagation. All itemized gaps below are shipped:

| # | Gap | Notes |
|---|---|---|
| N1 | ~~Implicit/stiff ODE solvers~~ ✅ shipped in vani-calculus v0.3.0 (2026-07-20) | backward Euler + Crank-Nicolson, each step solved via Newton's method (not fixed-point iteration, which wouldn't converge on stiff problems); BDF2 intentionally left out, see vani-calculus TODO.md |
| N2 | ~~ODE boundary-value-problem solver~~ ✅ shipped in vani-calculus v0.3.0 (2026-07-20) | shooting method: secant search over the initial slope, paired RK4 for the underlying first-order system |
| N3 | ~~Interval arithmetic / rigorous error-propagation~~ ✅ shipped in **vani-interval v0.1.0** (2026-07-21) | broken into N3.1-N3.6 below, all shipped in one repo |

**N3 breakdown** — this row actually bundled two distinct techniques, not
one: rigorous interval arithmetic (an `[lo,hi]` range provably containing
the true value, vs. a single float with hand-waved error) and first-order
error propagation (the linearized `σ_f ≈ sqrt(Σ(∂f/∂xi)²σxi²)` formula
science/engineering actually asks for day to day, not rigorous but far
more commonly needed). Shipped as one package, `vani-interval`. Itemized:

| # | Item | Notes |
|---|---|---|
| N3.1 | ~~`Interval` struct + core arithmetic (add/sub/mul/div/neg)~~ ✅ `iv_new`/`iv_add`/`iv_sub`/`iv_mul`/`iv_div`/`iv_neg`/etc. | mul/div need all corner-pair combinations since operand signs matter; div by an interval spanning zero is unbounded -- caller-trust limitation, not a runtime error |
| N3.2 | ~~Interval elementary functions (sqrt/exp/log/pow/sin/cos)~~ ✅ `iv_sqrt`/`iv_exp`/`iv_log`/`iv_pow_i64`/`iv_sin`/`iv_cos` | monotonic ones (sqrt/exp/log) just apply to both endpoints; sin/cos need a critical-point check (does the interval span a max/min?) -- the fiddly part, solved with a fixed-size critical-point search |
| N3.3 | ~~Interval set ops (contains/width/midpoint/intersect/union_hull)~~ ✅ `iv_contains`/`iv_width`/`iv_midpoint`/`iv_is_empty`/`iv_intersect`/`iv_union_hull` | |
| N3.4 | ~~Rigorous interval-bisection root-finding~~ ✅ `iv_bisect_root` | unlike vani-calculus's point-sample `bisect`, provably doesn't miss a root inside the starting bracket -- returns `Vec<Interval>` (every surviving candidate), not a single `Interval`, since a single-bracket version turns out not to be rigorous once interval arithmetic's "dependency problem" is accounted for; see vani-interval's README |
| N3.5 | ~~First-order error propagation: single-var, n-var independent, n-var with covariance~~ ✅ `ep_propagate_1var`/`ep_propagate_independent`/`ep_propagate_covariance` | n-var forms reuse vani-optimize's `fn(ref Vec<f64>, i64)->f64` multivariate convention via a small private helper, not vani-calculus's `gradient_1d`/`jacobian_1d` as originally sketched here -- those turned out to be single-var-sampled-at-multiple-points, not multivariate partials, so they didn't actually fit |
| N3.6 | ~~Closed-form propagation shortcuts (sum/product/quotient)~~ ✅ `ep_sum2`/`ep_sum_n`/`ep_product2`/`ep_quotient2` | the textbook `σ_f=sqrt(σx²+σy²)` (sum) and relative-error-in-quadrature (product/quotient) forms, worth having directly rather than always going through N3.5's general path |

**³ Scientific computing (aggregate)** — this row is a rollup, not a
concrete deliverable, and is now largely redundant with the specific rows
above it (signal processing, sparse matrices, and PDEs all graduated into
their own rows already). No new tracked items here; candidate for
downgrading to a footnote or removing outright next time this file gets a
structural pass, rather than a source of real gaps.

#### Where did G1-G7 / N1-N3 land?

- **G1-G7** (graph algorithms + combinatorics enumeration) ✅ shipped in
  **vani-discrete v0.1.0** (2026-07-20) — its own adjacency-matrix
  representation, since the compiler's builtin `Graph` type is opaque from
  vāṇी source (no accessor to enumerate edges/neighbors) and G1-G7
  couldn't be built on top of it.
- **N1-N2** (stiff/BVP ODE solvers) ✅ shipped in **vani-calculus v0.3.0**
  (2026-07-20) — same package, same `poly_*`/ODE conventions, no new
  dependency, exactly as planned above.
- **N3** (interval arithmetic + error propagation, broken into N3.1-N3.6
  above) ✅ shipped in **vani-interval v0.1.0** (2026-07-21) — new repo,
  depends on vani-calculus for `diff_central` only (N3.1-N3.4 are fully
  self-contained). Every gap in this document, and every itemized gap
  within a "mostly done" row, is now shipped — only the optional symbolic
  tier remains anywhere in this file.

### What's out of scope for this roadmap (research-tier math)

Abstract algebra (groups/rings/fields), category theory, topology, differential
geometry, algebraic geometry, functional analysis, measure theory, Lie groups/algebras,
representation theory, algebraic topology, noncommutative geometry, tensor calculus for
general relativity, exterior algebra/differential forms, Clifford/geometric algebra.
These serve much narrower audiences than the core scientific stack below and aren't
planned here — flag if a real use case shows up.

---

## Numeric/scientific tier — complete ✅

All 10 items below have shipped; kept as a build-sequence record (earlier
entries unblocked later ones) and for the size/dependency data. Items 1-8
were the original ordered sequence; #9 (vani-sparse) and #10
(vani-vectorcalc) each filled a gap-analysis row that was never part of
that sequence.

| # | Repo | Depends on | Scope | Rough size |
|---|---|---|---|---|
| 1 | ~~**matrix v0.2**~~ ✅ shipped 2026-07-20 | -- | Eigenvalues (power iteration + deflation), QR (Householder), SVD (bidiagonalization), condition number. | 5 functions |
| 2 | ~~**vani-complex**~~ ✅ shipped 2026-07-20 | -- | `Complex { re: f64, im: f64 }` struct + arithmetic, polar form, complex exp/log/trig, roots of unity. | 24 functions |
| 3 | ~~**vani-optimize**~~ ✅ shipped 2026-07-20 | -- | Multivariable unconstrained (fixed-step and Armijo-backtracking gradient descent, Newton's method with analytic or finite-difference derivatives, coordinate descent), quadratic specialty solvers, constrained (penalty method), linear programming (tableau simplex). | 20 functions |
| 4 | ~~**vani-geometry**~~ ✅ shipped 2026-07-20 | -- | Computational: convex hull (Andrew's monotone chain), closest pair, point-in-polygon, segment intersection. Analytic: circumcircle, conic-section classification, 2D/3D distance/angle formulas, 3D planes and skew-line distance. | 38 functions |
| 5 | ~~**vani-signal**~~ ✅ shipped 2026-07-20 | vani-complex | FFT/DFT (Cooley-Tukey radix-2), convolution/correlation, numeric Laplace/Z transforms, windowing functions. | 21 functions |
| 6 | ~~**vani-tensor**~~ ✅ shipped 2026-07-20 | matrix | N-dimensional arrays: flat `Vec<f64>` + shape `Vec<i64>` encoding (matching vani-matrix's row-major convention, not nested `Vec<Vec<...>>`), reshape, broadcast, contraction, N-D elementwise ops. | 23 functions |
| 7 | ~~**vani-pde**~~ ✅ shipped 2026-07-20 | matrix | Finite-difference solvers for classic PDEs (heat/wave/Laplace equation) on a 1D/2D grid, Dirichlet BCs, explicit time marching for heat/wave, direct dense solve via mat_solve for Laplace/Poisson. Shipped without a vani-calculus dependency -- no natural reuse point was found (see vani-pde's README "Design decisions"). | 21 functions |
| 8 | ~~**vani-algebra**~~ ✅ shipped 2026-07-20 | matrix, calculus (reuses poly_* ops) | Polynomial root-finding: closed-form cubic, general real-root finder for degree >= 4 (companion matrix + mat_eig_power + synthetic deflation, not a hand-derived quartic closed form -- see package README), nonlinear equation systems via Newton-Raphson. Real roots only. | 11 functions |
| 9 | ~~**vani-sparse**~~ ✅ shipped 2026-07-20 | matrix (test/interop only) | Sparse matrix formats (COO to build, CSR to operate on) and core ops -- matvec, transpose, scale, add, matmul (Gustavson's algorithm). Filled the "Linear algebra — sparse matrices" gap-analysis row; added after the original 8-item sequence was already complete. | 17 functions |
| 10 | ~~**vani-vectorcalc**~~ ✅ shipped 2026-07-20 | calculus | Gradient/divergence/curl/Laplacian (2D/3D, central finite differences) and double/triple/line integrals via nested reuse of calculus's integrate_simpson (no closures in vāṇी, so integration reuses pre-sampled Vec<f64> instead of a captured function pointer). Filled the "Calculus — vector" gap-analysis row. | 11 functions |

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

Calibrated against what's actually happened building the published packages --
vani-probability alone went from ~42 functions to ~90+ across four version bumps, each
with hand-verified reference values, tests, examples, and docs, plus two real compiler
bugs found and fixed along the way, all within a bounded body of work. The numeric tier
is now fully shipped, so this table doubles as a retrospective: the estimates held up.

| Tier | Repos | Rough shape per repo | Relative effort |
|---|---|---|---|
| **Matrix extension** | ✅ eigenvalues/QR/SVD in vani-matrix | ~8-10 functions, numerically fussier than what's there (iterative eigensolvers are easy to get subtly wrong) | 0.5–1 unit |
| **New numeric repos** | ✅ vani-complex, vani-optimize, vani-geometry, vani-signal | ~20-40 functions each, same validate-against-known-values discipline as the existing repos | ~1 unit each |
| **Bigger numeric repos** | ✅ vani-tensor, vani-pde | Wider design surface (N-D indexing scheme; PDE needs a discretization strategy decision up front) | ~1.5–2 units each |
| **Smaller-than-expected numeric repo** | ✅ vani-algebra | Estimated ~15-20 functions, shipped at 11 -- scope was deliberately narrowed (real roots only, no hand-derived Ferrari's-method quartic) rather than forcing a riskier implementation to hit the estimate | ~0.75 unit |
| **Gap-fill repos, requested separately** | ✅ vani-sparse, vani-vectorcalc, vani-discrete, vani-calculus v0.3.0, vani-interval | ~8-28 functions each; validated via cross-checks against dense vani-matrix ops (sparse), composed identities like curl(grad f)=0 and the divergence theorem (vectorcalc), the max-flow-min-cut theorem and enumeration totals against builtins (discrete), a stiff-system stability demonstration (calculus v0.3.0), and a rigorous-vs-linearized cross-check on the same problem plus a deliberate interval-arithmetic "dependency problem" test case (interval) -- composed/theorem checks throughout, not isolated hand-computed values alone | ~0.75-1 unit each |
| **CAS tier** | vani-bignum, vani-symbolic, vani-polyalgebra (not started; optional) | Open-ended -- correctness bugs are subtle and compound (a wrong simplification rule silently poisons everything built on it); real CAS projects are multi-year efforts even at small scale | Not comparable to the above; expect several units minimum for a minimal symbolic core, and treat "done" as aspirational |

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
6. ~~**vani-algebra**~~ ✅ shipped 2026-07-20 -- completed the original 8-item ordered sequence.
7. ~~**vani-sparse**~~ ✅ shipped 2026-07-20 and ~~**vani-vectorcalc**~~ ✅ shipped 2026-07-20 -- filled the "Linear algebra — sparse matrices" and "Calculus — vector" gap-analysis rows, requested separately from the ordered sequence above. Every gap-analysis row is now ✅.
8. ~~**vani-discrete**~~ ✅ shipped 2026-07-20 (G1-G7), ~~**vani-calculus v0.3.0**~~ ✅ shipped 2026-07-20 (N1-N2), and ~~**vani-interval**~~ ✅ shipped 2026-07-21 (N3) -- itemized follow-up gaps under the "mostly done" rows, each requested separately after step 7. Every itemized gap in this document is now shipped.
9. Symbolic tier only if full Mathematica/SageMath-class capability is actually wanted -- start with **vani-bignum**, since vani-symbolic can't do much without exact arithmetic underneath it. **Optional; confirm before starting.** This is the only work left anywhere in this document.

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
- **`use "../vendor/<dep>/src/lib.vani";` for a `[deps]`-declared package is redundant**
  (confirmed 2026-07-21 by direct test) -- `vanic check`/`build`/`run`/`test` already
  auto-discover `vani.toml` and prepend every dependency's entry source before
  compiling, independent of any `use` statement. It's still needed for pulling in
  *your own* package's other files (a test importing `../src/lib.vani`, which isn't a
  `[deps]` entry). New packages don't need to add the vendor `use` line at all; see
  [MAINT-2](#maintenance--audit-findings-added-2026-07-21) below for the cleanup pass
  on already-shipped packages.

---

## Maintenance / audit findings (added 2026-07-21)

Surfaced by user questions about hardware I/O, Big-O propagation, WCET/stack-safety
uniformity, and import ergonomics — not new package scope, upkeep on what's shipped.

| ID | Task | Effort | Status |
|---|---|---|---|
| MAINT-1 | **WCET (`#[wcet(cycles=N)]`) coverage backfill** — in progress, package-by-package. | large, multi-session | **11/12 done (vani-complex, vani-vectorcalc, vani-algebra, vani-discrete, vani-sparse, vani-pde, vani-interval, vani-tensor, vani-signal, vani-optimize, vani-geometry)** |
| MAINT-2 | ~~Strip the now-redundant `use "../vendor/<dep>/src/lib.vani";` line from every shipped package that has one~~ ✅ done 2026-07-21 | ~15-30 min/package | **done, all 9** |
| MAINT-3 | ~~`kosh_design.md`'s `vani.toml` example doesn't mention deps are auto-scoped~~ ✅ done 2026-07-21 | ~10 min | done |

**MAINT-1 methodology + progress (2026-07-21)**: vani-complex (24 functions, self-contained,
no deps) done first as the pattern-setter. **Real finding that changes the original
estimate**: `#[wcet]` is NOT a hand-derive-from-a-cost-table exercise — like
`#[bounded_stack]`, it's a real `vanic check`-enforced budget (`enforce_wcet` in
`safety.rs`), so the only correct workflow is the same "set a deliberately-low
placeholder, read the tool's exact reported number, fix it" loop used for
`#[bounded_stack]`. One extra wrinkle WCET has that `#[bounded_stack]` doesn't:
the estimator uses a callee's *declared* `#[wcet]` budget for cross-function calls
rather than re-deriving the callee's real body cost, so composite functions
(`complex_div`, `complex_tan`, etc.) give a wrong number on the first pass and need
a second pass once their dependencies are fixed to correct values — genuinely
bottom-up, not a single mechanical sweep. vani-complex took 3 rounds (leaf fns →
one-level-composite fns → `complex_tan`/`complex_tanh`, which depend on
`complex_div`) to converge.

**Checker limitation discovered along the way** (documented in vani-complex's
module header, and worth a compiler-side TODO): the WCET estimator gives a flat
cost to any top-level struct-literal return expression and does **not** recurse
into its field expressions. `complex_log`'s real enforced budget is only 10 cycles
despite its body calling `complex_abs`+`complex_arg`+`log()`, because that whole
call chain sits inside a `Complex { ... }` literal. This means `#[wcet]` budgets
on any struct-literal-returning function in this ecosystem are under-counted by
the checker itself, not just by whoever wrote the annotation — the tool will
happily accept a budget far lower than the function's real cost. Not a vani-complex
bug; a `vanic check` gap that affects every future package doing this backfill.

**Honest pace assessment**: vani-complex (24 fns, all pure arithmetic, zero
dependencies) took a full multi-round investigation-and-verification cycle to get
right. The remaining 11 packages are mostly larger (vani-probability alone has 106
functions) and many have loops over `Vec<f64>` (which get a WCET *formula* comment,
not an attribute, per the established convention — still requires per-function
judgment about which category a function falls into). The original "large,
multi-session" scope estimate holds; this is not a task that compresses into a
single sitting even at a good pace. Continuing package-by-package;
commit+publish happens after each package, same as MAINT-2.

**MAINT-2 done (2026-07-21)**: 9 packages actually had the redundant line (not 7 —
vani-probability→vani-matrix and vani-optimize→vani-matrix were missed in the original
scan above): vani-probability, vani-optimize, vani-signal, vani-tensor, vani-pde,
vani-algebra (2 lines: matrix + calculus), vani-sparse, vani-vectorcalc, vani-interval.
Each: line removed, `vanic check --no-verify` run on every test file in every package
(all clean), one full `vanic test` run per package as a runtime confidence check (all
passed), patch version bumped, republished. New versions: probability 0.4.3, optimize
0.1.1, signal 0.1.1, tensor 0.1.1, pde 0.1.1, algebra 0.1.1, sparse 0.1.1, vectorcalc
0.1.1, interval 0.1.1. Self-contained packages (vani-matrix, vani-complex,
vani-geometry, vani-discrete) needed no change (confirmed no vendor `use` line in any
of them).

**Why these are separate from the package-scope roadmap above**: MAINT-1/2/3 don't add
new mathematical coverage — they're consistency/correctness upkeep on packages already
shipped. Confirm before starting MAINT-1 specifically (large, ambiguous scope); MAINT-2
and MAINT-3 are small and mechanical enough to just do.

# Kosh Math Ecosystem Roadmap

This tracks which **packages** should exist to give vāṇī broad mathematical library
coverage — comparable to SciPy / Eigen / Armadillo / Boost.Math for the numeric tier,
with an optional, much larger symbolic tier on top, plus an ML tier built on both.
`catalog.md` lists what's actually published right now; this file is the
forward-looking plan.

Compiler-level items that this roadmap depends on (if any) are tracked in
[vani-compiler's docs/TODO_CURRENT.md](https://github.com/enthusiasticgeek/vani-compiler/blob/main/docs/TODO_CURRENT.md)
under "Kosh math-library ecosystem" and cross-linked from here.

Last updated: 2026-08-20

**Status: the numeric/scientific tier (all 14 packages below) is complete,
every row in the gap-analysis table below is ✅, and every itemized gap
within those rows (G1-G7, N1-N2, and now N3) is shipped.** The optional
symbolic tier and the ML tier are both also fully shipped (2026-08-16) --
see "Symbolic tier — complete" and "ML tier — complete" below. The
hardware-acceleration tier (scoped 2026-08-17) is now also fully
IMPLEMENTED as of 2026-08-20 -- `vani-cuda`, `vani-rocm`, `vani-tensorrt`
all exist as real, pushed repos with type-checked, codegen-verified
bindings -- but **none of the three are published to the Kosh registry
yet**, held back deliberately pending real-hardware testing (see
"Hardware-acceleration tier" below for the full status,
including each package's hardware/API-verification caveats).

---

## Already published

| Package | Version | Domain |
|---|---|---|
| [matrix](https://github.com/enthusiasticgeek/vani-matrix) | 0.2.0 | Dense linear algebra: construction, arithmetic, multiply, closed-form 2×2/3×3, Gauss-Jordan inverse, LU, Cholesky, eigenvalues (power iteration + deflation), condition number, Householder QR, SVD bidiagonalization |
| [calculus](https://github.com/enthusiasticgeek/vani-calculus) | 0.3.2 | Integration, differentiation, root-finding, 1D optimization, ODE solvers (incl. BDF2), polynomials, interpolation, series |
| [probability](https://github.com/enthusiasticgeek/vani-probability) | 0.4.8 | Descriptive/inferential stats, distributions, Bayesian inference, Markov chains, time series, CDFs/p-values, MLR, PCA, stochastic processes |
| [complex](https://github.com/enthusiasticgeek/vani-complex) | 0.1.2 | Complex numbers: arithmetic, polar form, exp/log/power/sqrt, trig/hyperbolic, roots of unity |
| [optimize](https://github.com/enthusiasticgeek/vani-optimize) | 0.1.7 | Numerical optimization: gradient descent (fixed/backtracking), Newton's method (analytic/finite-difference), coordinate descent, quadratic solvers, penalty-method constrained optimization, tableau simplex LP |
| [geometry](https://github.com/enthusiasticgeek/vani-geometry) | 0.1.2 | Computational + analytic geometry: 2D/3D point/vector arithmetic, line/segment distance and intersection, polygon area/perimeter/centroid, point-in-polygon, convex hull, closest pair, circumcircle, conic classification, 3D planes and skew-line distance |
| [signal](https://github.com/enthusiasticgeek/vani-signal) | 0.1.5 | Signal processing: naive DFT/IDFT, Cooley-Tukey radix-2 FFT/IFFT, magnitude/power spectrum and frequency-bin helpers, zero-padding, linear/circular convolution, cross-correlation, Hann/Hamming/Blackman/Bartlett/Kaiser/Tukey windowing, numeric Laplace/Z transforms |
| [tensor](https://github.com/enthusiasticgeek/vani-tensor) | 0.1.4 | N-dimensional arrays: flat Vec<f64> + explicit shape encoding, shape/stride/index utilities, construction, reshape, elementwise arithmetic/reductions, last-axis broadcasting, general N-D axis permutation, contraction (via matrix's mat_mul_rect) |
| [pde](https://github.com/enthusiasticgeek/vani-pde) | 0.1.5 | Finite-difference PDE solvers: 1D/2D Laplace-Poisson (elliptic, via matrix's mat_solve), 1D/2D heat (parabolic, explicit FTCS), 1D/2D wave (hyperbolic, explicit central-difference), Dirichlet BCs only |
| [algebra](https://github.com/enthusiasticgeek/vani-algebra) | 0.1.5 | Polynomial root-finding (closed-form cubic; degree >= 4 via companion matrix + mat_eig_power + synthetic deflation + Newton polish, real roots only) and nonlinear equation systems (Newton-Raphson, analytic/finite-difference Jacobian, via mat_solve) |
| [sparse](https://github.com/enthusiasticgeek/vani-sparse) | 0.1.4 | Sparse matrices: COO (build) / CSR (operate) formats, dense conversions, matvec (binary-search element lookup), transpose, scale, add, matmul (Gustavson's algorithm); every op cross-checked against the equivalent dense vani-matrix operation |
| [vectorcalc](https://github.com/enthusiasticgeek/vani-vectorcalc) | 0.1.4 | Vector calculus: gradient/divergence/curl/Laplacian (2D/3D, central differences), double/triple/line integrals via nested reuse of calculus's integrate_simpson; composed checks for curl(grad f)=0 and the 2D divergence theorem |
| [discrete](https://github.com/enthusiasticgeek/vani-discrete) | 0.1.3 | Graph algorithms (Floyd-Warshall, Kosaraju SCC, Edmonds-Karp max-flow/min-cut, Kuhn's bipartite matching, greedy coloring) and combinatorics enumeration (permutations, combinations, subsets, partition counting), own adjacency-matrix encoding |
| [interval](https://github.com/enthusiasticgeek/vani-interval) | 0.1.4 | Rigorous interval arithmetic (core arithmetic, elementary functions, set ops, interval-bisection root-finding returning every surviving candidate bracket) and first-order error propagation (single-var, n-var independent/covariance, closed-form sum/product/quotient shortcuts) |

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

*(A "Scientific computing (aggregate)" rollup row previously lived here;
removed 2026-07-25 as redundant with the specific rows above it -- signal
processing, sparse matrices, and PDEs had all already graduated into their
own rows, leaving it a summary of summaries with no gaps of its own to
track.)*

### ¹² Known gaps within "mostly done" rows

Three rows above are marked done with a qualifier rather than a plain ✅ —
this section spells out exactly what's still missing under each, as a
tracked TODO list distinct from the (already-tracked, already-shipped)
items elsewhere in this document.

**¹ Discrete math (graphs, basic combinatorics)** — builtin coverage:
BFS/DFS/Dijkstra/A*/cycle-detection/MST(Kruskal+Prim)/topo-sort (graphs),
factorial/binomial/perm/fibonacci (combinatorics, counting only). **All
7 gaps below shipped in vani-discrete v0.1.0 (2026-07-20)**, on the
package's own adjacency-matrix encoding (the builtin `Graph` type is
opaque from vāṇī source, so these couldn't be built on top of it):

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

#### Where did G1-G7 / N1-N3 land?

- **G1-G7** (graph algorithms + combinatorics enumeration) ✅ shipped in
  **vani-discrete v0.1.0** (2026-07-20) — its own adjacency-matrix
  representation, since the compiler's builtin `Graph` type is opaque from
  vāṇī source (no accessor to enumerate edges/neighbors) and G1-G7
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
| 10 | ~~**vani-vectorcalc**~~ ✅ shipped 2026-07-20 | calculus | Gradient/divergence/curl/Laplacian (2D/3D, central finite differences) and double/triple/line integrals via nested reuse of calculus's integrate_simpson (no closures in vāṇī, so integration reuses pre-sampled Vec<f64> instead of a captured function pointer). Filled the "Calculus — vector" gap-analysis row. | 11 functions |

## Symbolic tier — complete ✅ (optional, separate scope)

This is **not** a scaled-up version of the numeric tier -- it's a qualitatively
different problem. Correctness bugs compound silently (a wrong simplification rule
poisons everything built on top of it), and the design surface (expression
representation, canonicalization, rule ordering) needs to be settled before most
functions can even be written. Treat this as optional and budget it separately from
the numeric tier above.

| Repo | Depends on | Scope |
|---|---|---|
| ~~**vani-bignum**~~ ✅ v0.2.1 published 2026-08-17 | -- | Arbitrary-precision integers (rationals deferred to v0.2.0, matching this ecosystem's narrow-then-widen precedent). Numeric foundation everything else in this tier needs. Base-1e9 digit-array `BigInt`, construction/comparison/add/sub/mul/div/mod/gcd/pow (24 functions), full test suite passes both `--no-verify` and full-SMT `vanic check`. Published to kosh-index and verified via a fresh `vanic add bignum` in a scratch project (namespaced calls, e.g. `bignum::bn_add`, per the MAINT-5 per-package-namespacing rule). Surfaced two real compiler bugs along the way, both tracked in `vani-compiler/docs/TODO_CURRENT.md` and **both ✅ fixed 2026-07-24**: BUG-3 (a `--backend=c`-only false-abort in a Vec-bounds optimizer hint) and BUG-4 (`implement` blocks reject `#[attr]`-prefixed methods). **v0.1.1 (2026-07-25)** dropped the `--allow-partial-safety-coverage` escape hatch v0.1.0 needed for `BigInt_eq` -- `#[bounded_stack(bytes=257)]` added now that BUG-4 is fixed, `vanic audit-safety` reports full clean coverage. Re-verified end-to-end via AOT build on both backends (LLVM JIT/`vanic run` currently hits an unrelated, separately-tracked Windows JIT symbol-resolution issue in `mingw_ansi_stdio_shared_lib`, being worked on in a parallel session -- AOT is unaffected). **v0.1.2 (2026-07-27)** added `bn_pow_i64` (exponentiation by squaring) and `bn_pow_mod` (modular exponentiation, reducing after every multiply so intermediates stay bounded by the modulus). **v0.2.1 (2026-08-17)** adds direct test coverage for `rat_cmp` (Rational comparison), previously exercised only transitively through `rat_eq`/`rat_ne`/`rat_gt`/etc. |
| ~~**vani-symbolic**~~ ✅ full roadmap shipped 2026-08-16, published as package v0.7.1 | vani-bignum, vani-algebra | Expression trees, simplification rules, symbolic differentiation/integration, equation solving, polynomial factorization. v0.1.0 (construction/eval/print), v0.2.0 (simplification), v0.3.0 (`sym_diff`, published 2026-07-26), v0.5.0 (linear/quadratic equation solving via `vani-algebra`, published as package v0.5.0), v0.4.0 (polynomial integration -- power/sum/constant-multiple rules + linear u-substitution, published as package v0.6.0 since v0.5.0 had already claimed that version number), and v0.6.0+ (`vani-polyalgebra`'s scope folded in: rational-root theorem + synthetic division, published as package v0.7.0) all published; verified via a fresh `vanic add symbolic` across all four run/build × backend combinations (namespaced calls, e.g. `symbolic::sym_diff`). v0.2.0's validation leaned on property-based random sampling; v0.3.0/v0.4.0 leaned on evaluation-based checks (`diff_central` cross-check, then the fundamental theorem of calculus for integration); v0.6.0+'s rational-root detection is exact integer arithmetic (zero floating-point error) with a numeric fallback (reusing `vani-algebra`'s `algebra_poly_deflate`) for repeated-root multiplicity. Package version numbers diverge from the roadmap's own phase numbers below because v0.5.0 shipped before v0.4.0 per this roadmap's own recommended reordering -- each phase's TODO.md writeup notes its actual published version. See the scoping breakdown below for the full phased plan and what each phase actually delivered. |
| **vani-polyalgebra** | vani-bignum, vani-symbolic | Polynomial factorization, Gröbner bases. **Decision: fold into `vani-symbolic` as a later phase rather than a standalone repo** (see below) -- no separate repo planned. |

If the goal is SciPy/Eigen/Boost.Math-class coverage, the numeric tier above is the
whole job. If the goal is closer to Mathematica/Maple/SageMath, the symbolic tier is
required on top -- and is a fundamentally larger undertaking than everything else in
this document combined.

### `vani-symbolic` scoping breakdown (added 2026-07-24)

**Architecture decision: flat arena, not a recursive `Box<Self>` tree.**
The obvious design -- `enum Expr { Num(i64), Add(Box<Expr>, Box<Expr>), ... }`,
the standard shape in Rust/Lisp-family CAS toy implementations -- **does not
compile in vāṇī today**, confirmed by direct test while scoping this package.
Two independent compiler restrictions each block it on their own: enum
variants admit only a single payload field in v1 (so a two-child variant is
rejected outright), and `box()`'s admitted payload types don't include a
type that (transitively) contains itself. Full writeup, including the exact
repro and error messages, filed as a new entry in vani-compiler's
[`docs/missing_features.md`](https://github.com/enthusiasticgeek/vani-compiler/blob/main/docs/missing_features.md)
("Recursive / self-referential types"). This is a real, previously-undocumented
gap -- not something to wait on fixing before starting; the workaround is
solid and arguably a better fit for this ecosystem anyway:

Represent every expression tree as a `Vec<Node>` arena with `i64` child
indices (`-1` = none) instead of pointers -- the same "flat `Vec` + explicit
index" convention already used by `vani-tensor` (shape encoding),
`vani-discrete` (adjacency matrix), and `vani-sparse` (COO/CSR). Confirmed
working end-to-end (both backends, `vanic check`) with a minimal
`struct Node { kind: i64, value: i64, left: i64, right: i64 }` arena and a
recursive `eval(arena: ref Vec<Node>, idx: i64) -> i64` walker before writing
this breakdown. `kind` is a small integer tag (0=Num, 1=Var, 2=Add, 3=Sub,
4=Mul, 5=Div, 6=Pow, 7=Neg, plus one per elementary function needed later);
node payloads that don't fit in two `i64` fields (variable names, `BigInt`
coefficients) live in side tables (`Vec<OwnedStr>` for names, a parallel
arena for `BigInt` limbs) indexed the same way.

**Numeric layer**: v0.1.0 uses plain `i64` coefficients, not `BigInt` --
matches this ecosystem's narrow-then-widen precedent (`vani-algebra` shipped
real-roots-only, `vani-discrete` shipped counting-only). Switch `Num` nodes
to reference `vani-bignum`'s `BigInt` once exact-arithmetic overflow actually
becomes a problem in practice, and defer symbolic *rational* coefficients
entirely until `vani-bignum` ships `Rational` (its own documented v0.2.0).

**Phased breakdown** (version numbers are proposed, not committed):

| Phase | Scope | Depends on | Risk / notes |
|---|---|---|---|
| ~~v0.1.0~~ ✅ published 2026-07-26 | Arena + node-kind tags; builder functions (`sym_num`, `sym_var`, `sym_add`, `sym_mul`, ...) returning node indices; `sym_eval` (numeric substitution given `var_id`→value pairs); `sym_to_str` (precedence-aware pretty-printer); `sym_eq_structural` (same-shape equality, not semantic equality). | -- | Low. Same shape as every other package's "construction + IO" opening phase. Landed as scoped. |
| ~~v0.2.0~~ ✅ published 2026-07-26 | Simplification: constant folding, identities (`x+0`, `x*1`, `x*0`, `x-x`), canonical ordering of commutative `Add`/`Mul` operands (needed before "collect like terms" is even well-defined), like-term collection. | v0.1.0 | **Highest risk phase in the whole tier.** A wrong rule silently poisons every later phase built on top of it. Validated primarily via property-based sampling (`sym_eval(simplify(e)) == sym_eval(e)` at many randomly-generated sample points), not just hand-picked examples -- every bug found during development was a test-expectation bug, not an algorithm bug. Scoped deliberately narrow: flat linear combinations of monomials only, not general polynomial normal form. |
| ~~v0.3.0~~ ✅ published 2026-07-26 | Symbolic differentiation (`sym_diff`): sum/product/quotient/chain/power rules, building new arena nodes. | v0.2.0 (raw differentiation output needs the simplifier to stay readable) | Medium. Validated via cross-check against `vani-calculus`'s `diff_central` (symbolic derivative evaluated at a point ≈ numeric derivative at the same point, within tolerance) -- a genuine cross-package composed check, matching this ecosystem's established validation discipline. `sym_diff` appends to the SAME arena `root` lives in (reusing existing indices for unchanged subexpressions), unlike `sym_simplify`'s separate-`dst` design -- a deliberate departure once it became clear differentiation's outputs mostly reuse, rather than rebuild, their inputs. |
| ~~v0.4.0~~ ✅ published 2026-08-16 (as package v0.6.0 -- v0.5.0 had already claimed that version number) | Basic symbolic integration: a **fixed pattern table** (term-by-term polynomial power rule, elementary `exp`/`ln`/`sin`/`cos` antiderivatives, a small set of recognized `u`-substitution shapes) that returns an explicit "no rule matched" result rather than guessing. **Not** a general algorithm (no Risch) -- that stays out of scope indefinitely, consistent with this roadmap's existing "aspirational" framing for the CAS tier. | v0.3.0 (validation reuses differentiation) | High, confirmed. **Scope correction found during implementation**: `ExprNode`'s kind set never grew past the 8 kinds v0.1.0 shipped with, so the `exp`/`ln`/`sin`/`cos` antiderivatives above were dropped -- adding those kinds would mean extending every existing walker, far bigger than integration alone. Shipped as polynomial-only integration (power rule, linearity, constant-multiple/divisor rules, linear `u`-substitution). Validated exactly as planned: `sym_diff(sym_integrate(f))` evaluated (via `sym_eval`) against `f` at sample points -- landed clean on the first full test pass. |
| ~~v0.5.0~~ ✅ published 2026-08-16 (as package v0.5.0) | Simple equation solving: linear (`ax+b=0`) directly; quadratic by reusing `vani-algebra`'s existing closed-form solver rather than reimplementing it. General polynomial/system solving explicitly deferred. | v0.2.0, `vani-algebra` | Low -- thin layer, most of the hard work is already done in `vani-algebra`. Landed as scoped; the real work was extracting `[c0,c1,c2]` coefficients from a symbolic tree, reusing the existing Add/Sub-flattening walker `sym_simplify` already had. |
| ~~v0.6.0+~~ ✅ published 2026-08-16 (as package v0.7.0) | `vani-polyalgebra`'s scope folded in here: polynomial factorization (rational-root theorem + synthetic division, mirroring `vani-algebra`'s existing "closed-form + numeric fallback, real roots only" precedent). Gröbner bases stay out of scope unless a real use case shows up -- already flagged as research-tier math elsewhere in this document. | v0.1.0-v0.5.0 | Landed as scoped, no longer open-ended. Rational-root detection is exact integer arithmetic (`q^deg * P(p/q)`, multiplying through rather than dividing -- zero floating-point error); multiplicity recovery after synthetic division uses a numeric epsilon fallback, reusing `vani-algebra`'s already-published `algebra_poly_deflate`. A real bug (a zero constant term silently skipping the rest of the root search, e.g. `x^2-4x` reporting only root `0`) was caught by the test suite on the first run, not by inspection. |

**Explicitly out of scope, all phases**: general integration (Risch
algorithm); symbolic linear algebra (matrices of symbolic expressions --
would need `vani-matrix`'s whole `Vec<f64>` convention rethought for a
`Vec<Expr>`-arena-index type, a different and harder problem, not a small
extension); multivariable symbolic calculus beyond partial derivatives.

**Actual order taken**: v0.1.0 → v0.2.0 → v0.3.0 → v0.5.0 (published
before v0.4.0, per this section's own recommendation to jump ahead
since it's cheap and reuses `vani-algebra`) → v0.4.0 → v0.6.0+. All six
phases shipped 2026-08-16, closing out the `vani-symbolic` tier
entirely -- see the top-level table entry above for the full
published-version mapping (roadmap phase numbers don't match package
version numbers, because of the v0.5.0-before-v0.4.0 reordering).

---

## ML tier — complete ✅ (scoped 2026-07-25, shipped 2026-08-16)

A third tier, distinct from both the numeric and symbolic tiers above --
not a numeric-tier gap-fill row and not CAS. `vani-ml`'s full v0.1.0-v0.6.0+
roadmap shipped 2026-08-16 (see the table row below for what each phase
delivered).

**Deliberately staged in one repo, not split across repos** -- consistent
with how this ecosystem draws repo boundaries by *domain*, not by
architectural layer (e.g. `vani-tensor` is a separate repo from
`vani-matrix` because N-D arrays are a different domain from dense 2D
linear algebra, not because of a "storage layer vs. algorithm layer"
split). Classical ML and an autodiff/NN engine are one domain (machine
learning) at two capability levels, matching how `vani-symbolic` stages
construction → simplification → differentiation as versions within a
single repo rather than as separate repos.

| Repo | Depends on | Scope |
|---|---|---|
| **vani-ml** | vani-probability, vani-optimize (v0.1.0) | Classical ML (regression, clustering, metrics) staged first; autodiff + neural-net engine staged on top. **Full v0.1.0-v0.6.0+ roadmap shipped 2026-08-16** (the whole tier never needed vani-tensor or vani-matrix as a dependency -- see the scoping breakdown below for why at each phase). |

### Compiler-feature question resolved before scoping (2026-07-25)

An autodiff engine needs *something* like a differentiable computation
graph. The naive design -- closures that capture a mutable gradient
buffer by reference -- **does not compile in vāṇī today**: closures with
move/Copy captures and full `FnOnce` semantics for non-Copy captures
shipped 2026-07-15, but *ref-capturing* closures are explicitly out of
scope, filed under `docs/missing_features.md`'s "Lifetime variables"
entry as **path-D, deferred indefinitely** -- blocked on multi-parameter
lifetime variables, which the compiler's own `docs/decisions.md`
(2026-06-09) already declined to build ("adding explicit lifetime
variables for the rare N-ref case would add syntax complexity
disproportionate to its use"). There is no existing scope estimate for
path-D anywhere in the compiler docs, because it was sized for rejection,
not implementation; a real implementation would mean lifetime-parameter
syntax, propagation through fn signatures *and* struct defs, multi-lifetime
borrow-check rules, and closure-env lifetime tracking -- multiple weeks
minimum, with real risk of interaction with SIMD/safety-attribute/SMT
machinery already in the checker, for a payoff (ref-captured closures)
this project doesn't actually need.

**Decision: no compiler prerequisite.** The autodiff graph uses the same
flat-arena pattern `vani-symbolic` already uses for its expression tree
(`Vec<Node>` + `i64` child indices) -- refs are passed as explicit
`ref`/`mut ref` fn arguments (e.g. a `mut ref Vec<f64>` gradient-accumulator
buffer threaded through every backward-pass call), never captured. This is
a proven pattern, not a workaround invented for this package: three
existing/planned packages (`vani-vectorcalc`, `vani-symbolic`, now
`vani-ml`) already route around the no-ref-capture gap the same way.
Copy-only closures (already shipped) remain fine for elementwise ops with
no gradient tracking, e.g. an activation function applied via `vec_map`.

### `vani-ml` scoping breakdown

| Phase | Scope | Depends on | Risk / notes |
|---|---|---|---|
| ~~v0.1.0~~ ✅ published 2026-07-26 | Classical ML: linear regression (thin wrapper over `vani-probability`'s existing MLR), logistic regression (cross-entropy loss, **own gradient-descent loop -- not `vani-optimize`'s**, see below), k-means clustering, train/test split, core metrics (accuracy, MSE, precision/recall). | vani-probability, vani-optimize | Turned out lower-risk than even "Low" suggested, but the scope note itself needed a correction found during implementation, not before: `vani-optimize`'s `gradient_descent_fixed`/`backtracking` take a fixed `fn(ref Vec<f64>, i64) -> f64` objective/gradient signature with no way to thread `(X_flat, y)` through without a ref-capturing closure (the same gap the "Compiler-feature question" above already flagged for v0.3.0 -- it fired one phase earlier than expected). `logreg_fit` has its own small dedicated loop instead, same algorithm shape. 4 test files pass `vanic test` + full SMT `vanic check`; `vanic audit-safety` reports full `#[bounded_stack]` coverage. Found and filed a real LLVM-backend crash along the way (BUG-6, `vani-compiler/docs/TODO_CURRENT.md`: a standalone unary-minus float literal panics codegen even though `vanic check` accepts it) -- not fixed, worked around with `0.0 - 3.0`. |
| ~~v0.2.0~~ ✅ built 2026-07-27 | Data utilities: feature scaling (standardize/normalize), one-hot encoding, a `Dataset` struct (row-major `Vec<f64>` features + labels, matching `vani-matrix`/`vani-tensor`'s row-major convention), k-fold cross-validation. | v0.1.0 | Low. New but small and mechanical, landed as scoped. |
| ~~v0.3.0~~ ✅ published 2026-08-16 | Autodiff core: flat arena (`GraphNode` + `i64` child indices, node-kind tags -- direct reuse of `vani-symbolic`'s pattern, see above; deliberately no symbol table, unlike `vani-symbolic` -- graphs are built directly via function calls, never parsed from names, so a caller reuses the same `i64` index for a repeated value, producing real DAGs), forward evaluation and reverse-mode backward pass as two single linear loops (no recursion, no topological sort -- the append-only arena is already topologically ordered by construction), gradients returned as a fresh `Vec<f64>` (this ecosystem's dominant convention, not a `mut ref` out-parameter). | v0.2.0 | **Highest risk phase in this tier**, same designation as `vani-symbolic`'s v0.2.0 for the same reason: a wrong gradient rule silently poisons everything built on top. Primary correctness gate was finite-difference gradient checking against every node kind, cross-checked against `vani-calculus::diff_central` (same validation `vani-symbolic`'s own v0.3.0 used) -- including a dedicated test for the DAG/shared-node case (`f(x)=x*x`, `d/dx=2x`), the actual failure mode a wrong "overwrite instead of accumulate" implementation would produce. **Scope correction found during implementation**: the roadmap listed `vani-tensor` ("value storage") as a dependency here, but the design never touches its N-D functionality -- one `f64` per node is enough; not declared as a dependency, deferred to v0.4.0 where matmul/dense layers actually need it. |
| ~~v0.4.0~~ ✅ published 2026-08-16 | Dense/linear layer, activations, losses as graph node kinds/composed helpers. | v0.3.0 | Medium, landed as scoped with one correction: `graph_dense` (dot product + bias) and the losses (`graph_mse_loss`, `graph_binary_cross_entropy_loss`) are COMPOSED from existing `graph_mul`/`graph_add`/`graph_sub`/`graph_const` primitives rather than needing new dedicated node kinds -- matmul emerges from scalar-op composition, same "vani-tensor turned out unnecessary" pattern v0.3.0 already found. Only `relu`/`sigmoid`/`tanh` (new unary kinds) plus `log` (needed for cross-entropy) were actually added as kinds. `softmax` deliberately NOT added -- inherently multi-output (every output in a group depends on every input), a structural mismatch with this one-node-one-scalar design; this package's classification support has always been binary-only, so binary cross-entropy covers the loss side without it. 9 new finite-difference tests, including Relu split across its positive/negative branches separately (the kink at 0) and an activation-composition test proving the new kinds thread through the same backward machinery with no special-casing. |
| ~~v0.5.0~~ ✅ published 2026-08-16 | Optimizers over the graph's parameter vector: SGD, momentum, Adam. | v0.3.0 | Medium, landed as scoped. `vani-optimize`'s existing solvers confirmed (not just anticipated) not to fit: their fixed `fn(ref Vec<f64>, i64) -> f64` objective/gradient signature can't carry a per-graph gradient vector sparse over a larger arena. `MomentumState`/`AdamState` are indexed by position in the caller's params list (not arena index) and each `*_step` function returns a FRESH state rather than mutating in place -- sidesteps needing direct struct-field-assignment syntax, which has no precedent anywhere in this ecosystem's code to confirm works. 6 new tests: hand-computed single-step exactness per optimizer (fabricated grads, isolating optimizer arithmetic from autodiff correctness) plus a real convergence check per optimizer (minimizing `(x-3)^2` over 200-300 iterations). |
| ~~v0.6.0+~~ ✅ published 2026-08-16, CLOSED not left open-ended | Training-loop utilities, batching, a worked small-MLP example end-to-end. | v0.1.0-v0.5.0 | Deliberately narrowed from "a generic trainer + a couple of examples" to "one small utility (`shuffled_indices`, factored out of the seeded shuffle already duplicated in `train_test_split`/`k_fold_split`) + one thorough example" -- a genuinely generic trainer would need a ref-capturing closure (same wall v0.1.0 and v0.3.0 already hit), and a second toy problem wouldn't validate anything the first doesn't already cover. The one example: a 2-2-1 MLP trained on XOR (dense→sigmoid→dense→sigmoid→binary-cross-entropy, Adam), the canonical toy problem a single dense+sigmoid layer provably cannot solve. All 9 trainable params are reused across all 4 training examples in ONE arena -- real exercise of the DAG/shared-node gradient-accumulation property v0.3.0 validated in isolation, this time for real. Loss drops from ~0.72 to ~0.0001 over 3000 Adam epochs, identical on both backends -- the concrete "actually used" signal this phase's original "revisit once used" caveat was waiting for, so it's now considered closed rather than left open. |

A real registry-integrity bug (`vanic publish`) was found and fixed along
the way: publishing this tier's 4 version bumps back-to-back exposed a
CDN-staleness race in how the publish command re-fetched a package's
current index file before appending to it, which silently DROPPED the
`ml` v0.5.0 index entry (its GitHub Release still existed; only the
sparse-index line was lost). Fixed upstream in `vani-compiler`
(`BUG-197`, see its `docs/TODO_CURRENT.md`) and the dropped entry was
manually restored -- `index/ml.json` now correctly lists all 5 published
versions.

**Recommended order**: v0.1.0 → v0.2.0 → v0.3.0 (budget the most review
time here, it's the risk concentration point) → v0.4.0 → v0.5.0 → v0.6.0+
(optional/stretch). **Confirm before starting each phase**, same rule as
the symbolic tier -- this is a scoping breakdown, not a start signal.

---

## Hardware-acceleration tier — IMPLEMENTED 2026-08-20, unpublished

A fourth tier, orthogonal to numeric/symbolic/ML above: GPU-accelerated
math via `extern "C"` FFI bindings to vendor SDKs (CUDA, ROCm, TensorRT),
not new vāṇी-native algorithms. Asked directly: "how would you approach
hardware-accelerated math (CUDA, TensorRT, DLA, ROCm)?" -- this section
is that answer.

**Status as of 2026-08-20**: all three repos below exist, are pushed,
and their `.vani` bindings type-check cleanly with codegen confirmed to
reference every shim symbol by the exact correct name. **None are
published to the Kosh registry (`vanic publish`) yet** -- held back
deliberately pending real-hardware testing, not because anything failed:

| Repo | Repo status | Registry status |
|---|---|---|
| [`vani-cuda`](https://github.com/enthusiasticgeek/vani-cuda) | ✅ v0.1.0 implemented, pushed, tutorial site live | ⬜ not published |
| [`vani-rocm`](https://github.com/enthusiasticgeek/vani-rocm) | ✅ v0.1.0 implemented, pushed, tutorial site live | ⬜ not published |
| [`vani-tensorrt`](https://github.com/enthusiasticgeek/vani-tensorrt) | ✅ v0.1.0 implemented, pushed, tutorial site live | ⬜ not published |

All three target **2026-era SDKs and GPU generations exclusively, by
explicit design direction -- no backward compatibility with older
CUDA/ROCm/TensorRT releases, and no note is made when a function might
not work on older hardware beyond "it may or may not."** `vani-cuda`/
`vani-rocm` add the stream-ordered async allocator
(`cuda_malloc_async`/`hip_malloc_async` etc., 32 functions each) as
the modern recommended pattern alongside the classic synchronous one.
`vani-tensorrt` was fully rewritten from its original, deliberately
version-hedged design (the OLDER, now-removed positional-"bindings"
TensorRT API) to bind ONLY TensorRT 10+'s current tensor-name-based
execution API (`setTensorAddress`/`enqueueV3`, 15 functions) -- older
TensorRT releases are not supported at all, by design, not by gap.

Each repo's own README has the full, honest verification-status
writeup -- summarized: `vani-cuda`/`vani-rocm` are "compiles clean,
codegen-verified, hardware-unverified" (no GPU in this environment);
`vani-tensorrt` carries a second, larger layer of risk on top of that
-- no TensorRT SDK was even available to compile-check the shim
against at all in this environment. Building these surfaced two real
`vani-compiler` packaging fixes along the way: `vanic publish`'s
tarball builder silently dropped native FFI shim source files
(`.c`/`.h`/`.cu`/`.cuh`/`.cpp`/`.hpp`) before commits `9cc8108e` and
`8c315655` fixed it -- without that fix, none of these three packages
could have shipped their shim through the registry at all once
published.

### Compiler-feature question resolved before scoping (2026-08-17)

Same shape of question the ML tier's autodiff design faced: does this
need a compiler feature that doesn't exist? **No** -- confirmed directly
against the FFI ABI rules already documented in
`vani-compiler/tutorials/src/intermediate/09a_ffi_primer.md` and
`docs/simd_ffi_shims.md`, not assumed:

- Scalars, `Str`, `ref T`/`mut ref T`, and `#[repr(C)]` structs (subject
  to per-platform small-struct size caps) already cross the FFI boundary
  cleanly -- no new ABI work needed for any GPU vendor API's ordinary
  function signatures (`cudaMalloc`, `cudaMemcpy`, `cudaLaunchKernel`,
  HIP's near-identical mirror of the same calls).
- **Bulk buffer transfer is already proven**, not theoretical:
  `docs/simd_ffi_shims.md`'s existing NEON/AVX2/RVV shims already pass
  `ref Vec<i64>` as a contiguous C pointer across the boundary
  (`neon_matmul(a: ref Vec<i64>, b: ref Vec<i64>, ...)`). A `cudaMemcpy`-
  style host-side transfer binds the same way.
- Raw pointer types (`*const T`/`*mut T`) do **not** cross the FFI
  boundary in v1 (confirmed in `09a_ffi_primer.md`) -- device pointers
  are represented as an opaque address-sized `i64` handle instead
  (`mut ref i64` for an out-parameter, matching `cudaMalloc(void**,
  size_t)`'s device-pointer-writeback slot). This is a naming
  convention for the binding layer, not a compiler gap.
- Function-pointer callback parameters already work
  (`09a_ffi_primer.md`'s "Function pointers" section) -- lower priority
  here since the CUDA/HIP Runtime APIs are call-in, not callback-driven,
  but relevant if a driver-level API is bound later.
- **What genuinely can't be done, independent of FFI capability**: vāṇī
  itself can never emit GPU device code (no PTX/SPIR-V/HSA backend
  exists or is planned -- see `docs/missing_features.md`'s new "GPU /
  hardware-accelerated math" entry). Kernels stay written in CUDA C/C++
  or HIP C++, compiled by the vendor's own toolchain (`nvcc`/`hipcc`),
  and linked in via `--link-with`/`-l<name>` -- the package supplies the
  host-side orchestration (allocate, copy, launch, synchronize, free),
  not the device-side math.

**Decision: no compiler prerequisite**, same conclusion the ML tier
reached for ref-capturing closures -- this tier is pure Kosh-package
FFI-binding work.

### Real constraint this tier has that no prior tier did

Every published package so far was validated by actually running it
(hand-computed reference values, real `vanic test`/`vanic run` execution
on both backends). **This environment has no GPU** -- CI runs on
GitHub-hosted runners with no NVIDIA/AMD hardware, and there's no
self-hosted GPU runner configured for this ecosystem. A `vani-cuda`/
`vani-rocm`/`vani-tensorrt` package can be written, and can be confirmed
to *compile and link* against the vendor SDK's headers, but genuine
on-device execution can't be verified the way every other package in
this roadmap was -- the honest status would be "implemented from the
vendor's documented API contract, untested on real hardware," the same
caveat this session already used for `cancel <name>;`'s Windows/macOS
paths in `vani-compiler`. Whoever picks this tier up needs either real
GPU access or to accept that caveat explicitly, not silently.

### Scoping breakdown (historical -- all three now ✅ implemented, see status table above)

| Repo | Depends on | Scope | Risk / notes |
|---|---|---|---|
| **vani-cuda** ✅ | none (vendors nothing -- links against the system CUDA Toolkit's headers/libs, not a Kosh dependency) | Host-side CUDA Runtime API bindings: `cuda_malloc`/`cuda_free`/`cuda_memcpy_h2d`/`cuda_memcpy_d2h`/`cuda_memset`/`cuda_device_synchronize`/basic stream + event functions, `cuda_launch_kernel` wrapping `cudaLaunchKernel` (NOT the `<<<...>>>` syntax, which is a CUDA C++ extension, not plain C-ABI). Device pointers as opaque `i64` handles. Shipped with 3 starter kernels (vector add, SAXPY, naive matmul) as `.cu` files under `kernels/`, each with a `--link-with`-friendly plain-C shim entry point -- not as the package's real scope, callers are expected to bring their own kernels for anything beyond the starter set. 30 functions shipped, matching the ~20-30 estimate exactly. | Medium, as estimated. The real risk isn't the vani side, it's the **untestable-on-real-hardware** constraint above -- shipped as "compiles + links clean" verified, not "confirmed correct," pending a real hardware run. |
| **vani-rocm** ✅ | none (same as vani-cuda, links against ROCm/HIP) | Same shape as vani-cuda -- HIP's API is deliberately CUDA-API-compatible (`hipMalloc`/`hipMemcpy` mirror `cudaMalloc`/`cudaMemcpy` almost 1:1), so this largely reused vani-cuda's binding pattern and i64-handle convention directly rather than being a separate design problem. 30 functions, identical shape to vani-cuda's, `hip_`/`vr_`-prefixed. | Confirmed Low-Medium as estimated -- genuinely mostly find-and-adapt from vani-cuda's already-proven pattern, took a fraction of vani-cuda's own effort. Same untestable-on-real-hardware caveat (needs AMD hardware instead of NVIDIA). |
| **vani-tensorrt** ✅ (DLA folds in here, not a separate repo) | none directly, though a real user's model/weights come from elsewhere | Inference only (no training), and further narrowed to **loading an already-built engine only** -- the Builder/NetworkDefinition/BuilderConfig/ONNX-parser pipeline that actually constructs an engine was deliberately cut from v0.1.0's scope (a real, documented scope-narrowing decision made during implementation, mirroring vani-algebra's own precedent for dropping a hand-derived quartic closed form): `trtexec` (TensorRT's own bundled CLI) already covers engine-building well as a one-time offline step. TensorRT's C++-object-shaped API (`IRuntime`/`ICudaEngine`/`IExecutionContext`, classes with virtual dispatch, no C-callable subset at all) needed the anticipated **hand-written C++ shim layer wrapping the objects as opaque handles + free functions** -- confirmed real, genuinely different work from vani-cuda/vani-rocm's shims, which stayed plain C. 11 functions shipped (narrower than vani-cuda/vani-rocm's 30, matching the narrowed scope). DLA is not a separate binding surface -- it's a TensorRT execution-provider config flag (`setDeviceType(DLA)` on the builder config, which this package's out-of-scope Builder API doesn't currently expose) -- unaffected by the scope narrowing since it was never meant to be separate binding surface. | Confirmed Medium-High, the highest-risk of the three as predicted -- and with an EXTRA layer of risk the original scoping didn't fully anticipate: no TensorRT SDK at all was available to compile-check the shim against in this environment (unlike CUDA/HIP's Toolkits, which were at least apt-installable, just not installed) -- TensorRT requires an NVIDIA Developer account, not distributed through any standard Linux package manager. The shim also had to commit to one specific TensorRT API generation (the classic bindings-based `executeV2`, stable ~TRT7-8.x) that TensorRT 10.x has partially deprecated -- a real, acknowledged version-compatibility risk beyond the usual "needs hardware to confirm" caveat the other two carry. See `vani-tensorrt/README.md`'s "Hardware AND API-version verification status" section for the full writeup. |

**Order actually followed, exactly as recommended**: vani-cuda first
(proved the i64-handle FFI pattern and the "compiles clean, hardware-
unverified" honesty convention) → vani-rocm (genuinely cheap once
vani-cuda's pattern existed, confirming the prediction) → vani-tensorrt
(the C++-shim design question WAS genuinely separate work, as
predicted, and surfaced the extra SDK-unavailability + API-version-
churn risk noted above, which the original scoping flagged as a
category of risk but couldn't fully size in advance without attempting
it). This is now a completion record, not a
start signal -- the remaining real-hardware-verification gap is
tracked per-repo above, not here.

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
| **CAS tier** | ✅ vani-bignum; ✅ vani-symbolic full roadmap shipped 2026-08-16 (v0.1.0-v0.3.0, v0.5.0, v0.4.0, v0.6.0+ -- published as package versions 0.1.0-0.3.0, 0.5.0, 0.6.0, 0.7.0); vani-polyalgebra folded into vani-symbolic v0.6.0+, no separate repo, CLOSED | Turned out less open-ended than expected -- every phase shipped, including the flagged-High-risk v0.4.0 (integration) and the originally-open-ended v0.6.0+ (factorization). See the [`vani-symbolic` scoping breakdown](#vani-symbolic-scoping-breakdown-added-2026-07-24) above for the phase-by-phase risk profile and what each phase actually delivered. | v0.1.0 took ~1 unit. v0.2.0 (the flagged highest-risk phase) also landed at roughly ~1 unit -- the deliberate scope narrowing plus property-based validation kept it from ballooning. v0.3.0 (differentiation) landed at well under ~1 unit. v0.5.0 (equation solving) was genuinely Low as predicted -- thin layer over `vani-algebra`. v0.4.0 (integration, the flagged highest-risk remaining phase) landed clean on the first full test pass once scoped to polynomials only (dropping the roadmap's original `exp`/`ln`/`sin`/`cos` wording, a real-during-implementation scope correction, not a shortfall). v0.6.0+ (factorization) landed clean after one real bug (a zero-constant-term edge case) was caught by its own test suite on the first run. No phase in this tier ballooned past ~1 unit once scoped; the "open-ended" framing for v0.6.0+ turned out to be conservative given the deliberate rational-only, no-Gröbner-bases scope |
| **ML tier** | ✅ vani-ml phased v0.1.0-v0.6.0+, all shipped 2026-08-16 | v0.1.0-v0.2.0 (classical ML + data utilities) were glue-shaped, close to the "New numeric repos" row above. v0.3.0 (autodiff core) was a new-architecture phase comparable in risk profile to `vani-symbolic`'s v0.2.0 -- validated via finite-difference gradient checking against every node kind, cross-checked against `vani-calculus::diff_central`. v0.4.0-v0.5.0 built directly on the graph without needing `vani-tensor`/`vani-matrix` (matmul emerges from composing existing scalar ops). v0.6.0+ closed out with a real 2-2-1 MLP trained on XOR converging to ~0.0001 loss on both backends -- the concrete validation signal its own "revisit once used" scope note was waiting for. See the [`vani-ml` scoping breakdown](#vani-ml-scoping-breakdown) above. | ~1 unit for v0.1.0-v0.2.0 combined; v0.3.0 landed close to the "Bigger numeric repo" estimate (~1.5-2 units); v0.4.0-v0.6.0+ landed at roughly ~0.5 unit each once v0.3.0's arena/traversal design was solid -- composing existing ops rather than adding new representations kept later phases cheaper than the original per-phase estimate |
| **Hardware-acceleration tier** | ✅ IMPLEMENTED 2026-08-20 (unpublished) -- `vani-cuda`, `vani-rocm`, `vani-tensorrt` (DLA folds into vani-tensorrt as a config path, no separate repo) | Pure FFI-binding work, no compiler prerequisite, exactly as predicted. All three repos exist, are pushed, and type-check/codegen-verify cleanly -- none published to the registry yet, held back pending real-hardware testing. "Shipped" here means "compiles and links against the vendor SDK," not "confirmed correct on hardware" -- see the tier's own status table above for the per-repo verification writeup. | vani-cuda: 30 functions shipped, ~1 unit as estimated. vani-rocm: 30 functions, confirmed ~0.5 unit once vani-cuda's pattern existed. vani-tensorrt: 11 functions (scope narrowed to engine-loading + inference only during implementation), landed in the predicted 1.5-2 unit Medium-High risk band, plus an extra SDK-unavailability + API-version-churn risk the original estimate flagged as a risk category but couldn't fully size in advance |

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
9. ~~Symbolic tier~~ ✅ CLOSED 2026-08-16. ~~**vani-bignum**~~ ✅ shipped 2026-07-24 -- the exact-arithmetic foundation. **vani-symbolic** phased breakdown (v0.1.0 construction/print/eval → v0.2.0 simplification → v0.3.0 differentiation → v0.5.0 equation solving → v0.4.0 integration → v0.6.0+ polynomial factorization, folding in what would've been vani-polyalgebra) scoped 2026-07-24, all six phases shipped and published (as package versions 0.1.0-0.3.0, 0.5.0, 0.6.0, 0.7.0 -- package version numbers diverge from phase numbers because v0.5.0 published before v0.4.0), see [above](#vani-symbolic-scoping-breakdown-added-2026-07-24).
10. ~~ML tier~~ ✅ CLOSED 2026-08-16, independent of the symbolic tier (depended only on the numeric tier, which was done). **vani-ml** phased breakdown (v0.1.0 classical ML → v0.2.0 data utilities → v0.3.0 autodiff core → v0.4.0 layers/activations/losses → v0.5.0 optimizers → v0.6.0+ training-loop utilities/stretch) scoped 2026-07-25, all phases shipped and published, see [above](#ml-tier-complete-scoped-2026-07-25-shipped-2026-08-16). Both the symbolic and ML tiers are now closed; every planned repo in this document has shipped.
11. **Hardware-acceleration tier -- IMPLEMENTED 2026-08-20, unpublished.** Independent of every prior tier (no dependency on numeric/symbolic/ML). **vani-cuda** → **vani-rocm** → **vani-tensorrt** (DLA folds in as a config path), all three now real, pushed repos -- see [above](#hardware-acceleration-tier--implemented-2026-08-20-unpublished). The real, material caveat this tier carried before starting held throughout: no GPU exists in this environment to validate on, so "implemented" here means "compiles and links," not "confirmed correct on hardware" -- publishing to the Kosh registry is being held back explicitly pending real-hardware testing, not happening automatically just because the code is written.

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
- **vāṇī has no `#[derive(...)]`.** `Eq`, print/debug formatting, cloning, etc. are
  never auto-generated -- every non-Copy struct that needs equality gets a hand-written
  `implement Eq for T { fn eq(self: ref T, other: ref T) -> bool { ... } }` (Copy
  structs can take `self: T, other: T` by value instead). `vani-bignum`'s `BigInt`
  is the worked example (`implement Eq for BigInt` in `src/lib.vani`). This is a
  deliberate language design choice, not a gap on the roadmap to fix -- budget the
  boilerplate per struct rather than waiting for it to arrive. One real wrinkle:
  `#[bounded_stack(...)]` on a method *inside* an `implement` block required
  vani-compiler's BUG-4 fix (2026-07-24) to even parse; packages published before
  that fix may still be using the `--allow-partial-safety-coverage` escape hatch for
  their `Eq` impl and could drop it on their next republish.

---

## Maintenance / audit findings (added 2026-07-21)

Surfaced by user questions about hardware I/O, Big-O propagation, WCET/stack-safety
uniformity, and import ergonomics — not new package scope, upkeep on what's shipped.

| ID | Task | Effort | Status |
|---|---|---|---|
| MAINT-1 | ~~WCET (`#[wcet(cycles=N)]`) coverage backfill~~ ✅ done 2026-07-21, all 12 packages | large, multi-session | **done, 12/12** |
| MAINT-2 | ~~Strip the now-redundant `use "../vendor/<dep>/src/lib.vani";` line from every shipped package that has one~~ ✅ done 2026-07-21 | ~15-30 min/package | **done, all 9** |
| MAINT-3 | ~~`kosh_design.md`'s `vani.toml` example doesn't mention deps are auto-scoped~~ ✅ done 2026-07-21 | ~10 min | done |
| MAINT-4 | ~~No sanity check enforced `#[bounded_stack]`/`#[wcet]` coverage before a package landed in kosh-index — MAINT-1 was a one-time manual pass with no lasting gate~~ ✅ done 2026-07-21, `vanic audit-safety` + `vanic publish` gate | ~1 session | **done** |
| MAINT-5 | ~~No namespace boundary between packages — a name collision (with a vāṇī builtin, or between two unrelated packages) was an unrecoverable compile error, and a "diamond" shared dependency silently produced missing-function errors~~ ✅ done 2026-07-21, Kosh namespacing arc (6 phases) + full ecosystem migration | large, multi-session | **done, 12/12** |

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

**MAINT-1 done (2026-07-21), all 12 packages**: vani-complex (24 fns, 24 real
`#[wcet]`), vani-vectorcalc (11 fns, 8 real), vani-algebra (11 fns, 0 real —
every fn loops over a Vec length or iteration count), vani-discrete (18 fns, 3
real), vani-sparse (16 fns, 3 real), vani-pde (21 fns, 5 real), vani-interval
(34 fns, 24 real), vani-tensor (23 fns, 0 real), vani-signal (23 fns, 0 real),
vani-optimize (23 fns, 2 real), vani-geometry (39 fns, 32 real), vani-probability
(106 fns, 24 real — this package already carried hand-written "WCET ~ N cycles"
comments from earlier work, turning most of the job into verifying/correcting
existing estimates rather than writing from scratch). ~230 functions got a real
`vanic check`-exact `#[wcet(cycles=N)]`; the rest got a WCET formula comment
(loop/recursion/fn-pointer-bearing, unbounded to the checker's WCET model).
Two real compiler findings surfaced along the way, tracked in
`vani-compiler/docs/TODO_CURRENT.md`: BUG-2 (the WCET estimator doesn't
recurse into struct-literal field expressions, found on vani-complex) —
✅ **fixed 2026-07-24** — and the Big-O tool's nesting-depth heuristic
disagreeing with correct algorithmic analysis on amortized bounds (found on
vani-discrete's `disc_scc_kosaraju`), which remains open.
Every package verified via `vanic check` on every test file plus at least one
full `vanic test` run per package before commit+publish. The original "large,
multi-session" estimate held — this took the rest of the session across many
tool-call rounds, not a quick pass.

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

**MAINT-4 done (2026-07-21)**: MAINT-1 was a one-time manual audit with nothing stopping
the *next* publish from skipping WCET/stack-safety coverage entirely. Added
`vanic audit-safety <path> [--format=text|json]` (vani-compiler), which reuses the
existing `wcet_body`/`compute_stack_depths` analyses to determine, per function,
whether `#[bounded_stack]`/`#[wcet]` is *computable* (i.e., the function is eligible)
and flags any eligible-but-missing case — not blanket 100% attribute presence, since
fn-pointer params make `#[bounded_stack]` uncomputable and unbounded loops/recursion
make `#[wcet]` uncomputable, and both are legitimately exempt. Vendored `[deps]`
functions are excluded. `vanic publish` now runs this audit before building the
tarball and hard-blocks on any gap, with `--allow-partial-safety-coverage` as an
explicit escape hatch. Packages are libraries (`src/lib.vani`, no `fn main()`), so
`compile_library`/`compile_library_path` (checker::check_library skips the
main-required check, otherwise identical to `compile`/`compile_path`) were added to
support auditing them directly.

Running the new tool against all 12 already-published MAINT-1 packages validated the
manual audit — and found 4 real gaps it missed: vani-discrete's `_disc_has_edge` and
`_disc_transpose` (missing `#[bounded_stack]`), vani-optimize's `penalty_value`
(missing `#[wcet]` — it takes fn-pointer params so is correctly `#[bounded_stack]`-exempt,
but indirect calls get a flat 10-cycle WCET charge, making it `#[wcet]`-eligible), and
vani-probability's `markov_is_absorbing_state` (missing `#[wcet]`, 1 of 106 functions).
All four fixed, republished: discrete 0.1.2, optimize 0.1.3, probability 0.4.5. All 12
packages now pass `vanic audit-safety` cleanly.

**MAINT-5 done (2026-07-21)**: sourced from a direct user question ("what happens if a
kosh-index package has the same function name as a vāṇī built-in — namespace or
modules?"). Testing the answer surfaced a second, more serious bug: a project depending
on two packages that each vendor their own copy of a shared dependency (`probability` +
`optimize`, both vendoring `matrix`) failed to compile with "unknown function" errors
for every matrix function — transitive dependencies were only resolved one level deep,
and (unrelated to that) `probability` and `optimize` had drifted to different pinned
`matrix` versions.

Fixed as a full 6-phase arc in vani-compiler (see
[`docs/kosh_namespacing_design.md`](https://github.com/enthusiasticgeek/vani-compiler/blob/main/docs/kosh_namespacing_design.md)
for the complete design + verification writeup):

- **Phase 1** — real transitive dependency graph, deduplicated by `(name, version)` so a
  diamond-shared package resolves to one compiled copy regardless of how many
  dependents pull it in or how deeply it's vendored. Along the way, found and fixed two
  latent bugs in `load_manifest`'s own unguarded recursion (crashed on a circular
  `[deps]` reference) — see Phase 2.
- **Phase 2** — package-level circular-dependency detection, reusing the same Tarjan SCC
  algorithm that backs `vanic acyclicity`'s function-call-graph analysis.
- **Phase 3** — automatic per-package namespacing: every `[deps]` package is compiled
  inside its own `module <pkg_name> { ... }`, so its functions (and any exported struct
  types) are referenced as `pkgname::item` — the actual fix for the original question.
  A dependency defining `fn abs(...)` no longer collides with the vāṇī builtin `abs`,
  or with any other package's `abs`, ever. Found and fixed a real parser gap along the
  way (module bodies had no support for `#[attr]`-prefixed items, needed by every real
  kosh package's `#[bounded_stack]`/`#[wcet]` annotations).
- **Phase 4** — `vani.lock` now records the full resolved transitive graph, not just
  direct `[deps]` entries.
- **Phase 5** — migration UX: an unqualified call to a dependency function now gets a
  "did you mean `pkgname::item`?" diagnostic instead of a bare unknown-function error.
  Also fixed a real bug in `vanic add` itself: it wrote the raw registry package name as
  the `[deps]` key verbatim, which broke for any hyphenated name (the real published
  `hello-kosh` package, for instance) since a `[deps]` key must now be a valid
  identifier. `vanic add` sanitizes automatically now.
- **Phase 6** — migrated and republished all 8 affected packages (every one with
  `[deps]`) to qualified call syntax: `vectorcalc` 0.1.3, `algebra` 0.1.3, `pde` 0.1.3,
  `interval` 0.1.3, `tensor` 0.1.3, `signal` 0.1.3, `optimize` 0.1.4, `probability`
  0.4.6. Fixed the `probability`/`optimize` `matrix` version drift (0.1.0 → 0.2.0,
  confirmed purely additive via diff) as part of this pass.

**Final verification**: a fresh project depending on the real, newly-published
`probability` + `optimize` (the exact diamond that started this) compiles clean with
zero version conflict, zero missing functions, zero namespace collision. All 12
published kosh packages pass `vanic audit-safety` cleanly.

**Why these are separate from the package-scope roadmap above**: MAINT-1 through MAINT-5 don't add
new mathematical coverage — they're consistency/correctness upkeep on packages already
shipped. All five are now done — every package-scope item AND every maintenance item
in this document is shipped; only the optional symbolic tier remains anywhere in this
roadmap.

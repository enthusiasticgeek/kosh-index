# Kosh Math Ecosystem Roadmap

This tracks which **packages** should exist to give vāṇी broad mathematical library
coverage — comparable to SciPy / Eigen / Armadillo / Boost.Math for the numeric tier,
with an optional, much larger symbolic tier on top. `catalog.md` lists what's actually
published right now; this file is the forward-looking plan.

Compiler-level items that this roadmap depends on (if any) are tracked in
[vani-compiler's docs/TODO_CURRENT.md](https://github.com/enthusiasticgeek/vani-compiler/blob/main/docs/TODO_CURRENT.md)
under "Kosh math-library ecosystem" and cross-linked from here.

Last updated: 2026-07-20

**Status: the numeric/scientific tier (all 12 packages below) is complete,
and every row in the gap-analysis table below is now ✅** (three rows carry
a "mostly done" qualifier — see "Known gaps within 'mostly done' rows" for
the itemized list of what's still missing under those). What remains is
the optional, much larger symbolic tier — see "Planned: symbolic tier"
below — plus the smaller, unscheduled items in that gaps section.

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
| Discrete math (graphs, basic combinatorics) | ✅ mostly done¹ | builtin |
| Algebra (equations, polynomial roots) | ✅ done (v0.1.0, real roots only) | vani-algebra |
| Linear algebra — dense | ✅ done | vani-matrix |
| Linear algebra — eigenvalues/QR/SVD (dense) | ✅ done (v0.2.0) | vani-matrix |
| Linear algebra — sparse matrices | ✅ done (v0.1.0) | vani-sparse |
| Calculus — single-variable/1D | ✅ done | vani-calculus |
| Calculus — vector (div/curl/multi-integral) | ✅ done (v0.1.0) | vani-vectorcalc |
| Differential equations — ODE | ✅ done | vani-calculus |
| Differential equations — PDE | ✅ done (v0.1.0) | vani-pde |
| Complex analysis | ✅ done (v0.1.0) | vani-complex |
| Numerical analysis | ✅ mostly done² | vani-calculus |
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

**¹ Discrete math (graphs, basic combinatorics)** — covered: BFS/DFS/
Dijkstra/A*/cycle-detection/MST(Kruskal+Prim)/topo-sort (graphs),
factorial/binomial/perm/fibonacci (combinatorics, **counting only**).
Missing:

| # | Gap | Notes |
|---|---|---|
| G1 | All-pairs shortest path (Floyd-Warshall) | |
| G2 | Strongly-connected components (Tarjan or Kosaraju) | |
| G3 | Max-flow / min-cut (Ford-Fulkerson or Dinic's) | |
| G4 | Bipartite matching (Hopcroft-Karp or augmenting-path) | |
| G5 | Graph coloring (greedy, or backtracking for exact) | |
| G6 | Permutation/combination/subset **enumeration** (not just counting) | e.g. next-permutation, generate all k-subsets |
| G7 | Integer partition functions | |

**² Numerical analysis** — covered: integration (trapz/Simpson/Romberg/
Gauss-Legendre 5-point/adaptive), differentiation (central/forward/second,
1D gradient/Jacobian/Hessian-diag), root-finding (bisection/secant/Newton/
Brent), 1D optimization (golden-section/Brent/Newton), ODE (Euler/RK4/
RK45/Adams-Bashforth-2 explicit, **plus backward Euler/Crank-Nicolson
implicit and a shooting-method BVP solver as of v0.3.0**), polynomials,
interpolation (Lagrange/linear-table/natural-cubic-spline), stable
summation. Missing:

| # | Gap | Notes |
|---|---|---|
| N1 | ~~Implicit/stiff ODE solvers~~ ✅ shipped in vani-calculus v0.3.0 (2026-07-20) | backward Euler + Crank-Nicolson, each step solved via Newton's method (not fixed-point iteration, which wouldn't converge on stiff problems); BDF2 intentionally left out, see vani-calculus TODO.md |
| N2 | ~~ODE boundary-value-problem solver~~ ✅ shipped in vani-calculus v0.3.0 (2026-07-20) | shooting method: secant search over the initial slope, paired RK4 for the underlying first-order system |
| N3 | Interval arithmetic / rigorous error-propagation | open scope, unclear real demand yet; still not scheduled |

**³ Scientific computing (aggregate)** — this row is a rollup, not a
concrete deliverable, and is now largely redundant with the specific rows
above it (signal processing, sparse matrices, and PDEs all graduated into
their own rows already). No new tracked items here; candidate for
downgrading to a footnote or removing outright next time this file gets a
structural pass, rather than a source of real gaps.

#### Where do G1-G7 / N3 live?

- **G1-G5** (graph algorithms) and **G6-G7** (combinatorics enumeration):
  not yet decided/scheduled. Candidate: a new **vani-discrete** package
  with its own adjacency-matrix representation (the compiler's builtin
  `Graph` type is opaque from vāṇी source -- no accessor to enumerate
  edges/neighbors -- so G1-G7 can't be built on top of it; they need their
  own from-scratch encoding, same as every other kosh package).
- **N1-N2** (stiff/BVP ODE solvers) ✅ shipped in **vani-calculus v0.3.0**
  (2026-07-20) — same package, same `poly_*`/ODE conventions, no new
  dependency, exactly as planned above.
- **N3** (interval arithmetic) has no obvious home yet and unclear
  real-world pull; lowest priority of the group, still unscheduled.

G1-G7 and N3 are not scheduled — this remains a gap inventory, not a
commitment to build. Confirm scope and priority before starting any of
them, same as the (now-complete) numeric tier and the still-optional
symbolic tier.

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
| **Gap-fill repos, requested separately** | ✅ vani-sparse, vani-vectorcalc | ~11-17 functions each; validated via cross-checks against dense vani-matrix ops (sparse) and composed identities like curl(grad f)=0 and the divergence theorem (vectorcalc) rather than isolated hand-computed values alone | ~1 unit each |
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
7. ~~**vani-sparse**~~ ✅ shipped 2026-07-20 and ~~**vani-vectorcalc**~~ ✅ shipped 2026-07-20 -- filled the two remaining gap-analysis rows ("Linear algebra — sparse matrices", "Calculus — vector"), requested separately from the ordered sequence above. Every gap-analysis row is now ✅.
8. Symbolic tier only if full Mathematica/SageMath-class capability is actually wanted -- start with **vani-bignum**, since vani-symbolic can't do much without exact arithmetic underneath it. **Optional; confirm before starting.**

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

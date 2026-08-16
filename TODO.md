# kosh-index — TODO

Task list distilled from [`ROADMAP.md`](ROADMAP.md), which stays the source of
truth for scope/rationale — this file is just the actionable checklist. Last
distilled: 2026-08-16.

> **Context**: the numeric/scientific tier (12 packages, every gap-analysis
> row, all 5 MAINT items), the symbolic tier, and the ML tier are all fully
> shipped. Nothing planned in this document remains open.

---

## Symbolic tier ✅ CLOSED 2026-08-16

Full scope, architecture decision (arena representation, not recursive
`Box<Self>`), and risk notes per phase are in ROADMAP.md's
["`vani-symbolic` scoping breakdown"](ROADMAP.md#vani-symbolic-scoping-breakdown-added-2026-07-24).

- [x] ~~**vani-symbolic v0.1.0**~~ ✅ published 2026-07-25
      — arena (`Vec<ExprNode>` + `i64` child indices) + node-kind tags;
      construction (`sym_num`/`sym_var`/`sym_add`/`sym_sub`/`sym_mul`/
      `sym_div`/`sym_pow`/`sym_neg`, `symtab_intern`/`symtab_name_at`),
      introspection (`sym_kind`/`sym_is_leaf`), `sym_eval` (numeric
      substitution, nonnegative-integer `Pow` only), `sym_to_str`
      (precedence-aware printer, a genuinely new pattern in this
      ecosystem), `sym_eq_structural` (same-shape only). Full test suite
      + composed example verified byte-identical on both backends with
      full SMT `vanic check`; `vanic audit-safety` clean on the first
      pass, no escape hatch needed. The naive recursive `Box<Self>`
      design was confirmed NOT to compile (two independent restrictions,
      documented in vani-compiler's new `docs/missing_features.md` entry)
      before committing to the arena representation. Published to
      kosh-index and verified via a fresh `vanic add symbolic` across
      all four run/build × backend combinations (namespaced calls, e.g.
      `symbolic::sym_add`, `symbolic::ExprNode`).
- [x] ~~**vani-symbolic v0.2.0**~~ ✅ published 2026-07-25
      — simplification: `sym_simplify` in two layers -- constant folding
      + identities for `Mul`/`Div`/`Pow`/`Neg` (`_sym_fold_binary`), and
      flatten-and-collect like-term collection for `Add`/`Sub`
      (`_sym_flatten_sum`/`_sym_simplify_sum`), deliberately scoped to
      flat linear combinations of `Num`/`Var`/`Mul(Num,Var)` monomials --
      NOT general polynomial normal form (no multiplicative factor
      combination, no cross-operator associativity flattening). Builds
      into a SEPARATE output arena, never mutating the source in place.
      **Was the flagged highest-risk phase in the whole tier** — validated
      primarily via property-based random sampling (`seed_rng` +
      `rand_in_range`, deterministic, 8 sample points per case) confirming
      `sym_eval(simplify(e)) == sym_eval(e)`, not just hand-picked
      examples, per this checklist's own prior note. Every bug found
      during development was a test-expectation bug (an overly strict
      assertion, a scope mismatch against Layer 1's documented boundary),
      not an algorithm bug -- both layers passed on the first real
      attempt. `vanic audit-safety` needed exactly one added attribute,
      matching the pre-implementation prediction that recursive
      simplification functions would be exempt. Published to kosh-index
      and verified via a fresh `vanic add symbolic` across all four
      run/build × backend combinations.
- [x] ~~**vani-symbolic v0.3.0**~~ ✅ published 2026-07-26
      — symbolic differentiation: `sym_diff(arena, root, var_id)`, one
      rule per node kind (sum/difference/negation/product/quotient/
      power+chain), appending new derivative nodes to the SAME arena
      (reusing existing indices for unchanged subexpressions, unlike
      `sym_simplify`'s separate-arena design). Raw output intentionally
      unsimplified -- composes with `sym_simplify` per the plan. Cross-
      checked against vendored `vani-calculus`'s `diff_central` (numeric
      derivative) at integer sample points, both for a plain polynomial
      and a chain-rule case; full test suite + composed example verified
      byte-identical on both backends (run and build/AOT), full SMT
      `vanic check` clean, `vanic audit-safety` reports full coverage
      with no gap introduced. Published to kosh-index and verified via a
      fresh `vanic add symbolic` across all four run/build × backend
      combinations (namespaced calls, e.g. `symbolic::sym_diff`).
      `vani-calculus` vendored for tests/examples only -- production
      `sym_diff` has zero calls into it.
- [x] ~~**vani-symbolic v0.5.0**~~ ✅ published 2026-08-16 (as package v0.5.0)
      — simple equation solving: linear directly, quadratic by reusing
      vani-algebra's existing closed-form solver (`algebra_poly_roots_real`)
      rather than reimplementing it. Pulled ahead of v0.4.0 as planned.
      The real work: `sym_poly_coeffs_le2` extracts `[c0,c1,c2]` from a
      symbolic tree, reusing `_sym_flatten_sum` (the same proven Add/Sub
      walker `sym_simplify` already had). Vendoring `vani-algebra` hit a
      real transitive-dependency version conflict (its own vendored
      `calculus` was stale) — fixed by bumping and republishing
      `vani-algebra` itself (v0.1.3 → v0.1.4) first. 8 tests, full SMT
      `vanic check` clean, both backends verified via `examples/solve_demo.vani`.
- [x] ~~**vani-symbolic v0.4.0**~~ ✅ published 2026-08-16 (as package v0.6.0
      — v0.5.0 had already claimed package version `0.5.0`, so this
      phase published as `0.6.0` to stay ahead of it)
      — basic symbolic integration via a fixed pattern table (no general/
      Risch algorithm). **Scope correction found during implementation**:
      dropped the roadmap's original `exp`/`ln`/`sin`/`cos` antiderivative
      wording — `ExprNode`'s kind set never grew past v0.1.0's original 8
      kinds, and adding transcendental kinds would mean touching every
      existing walker. Shipped polynomial-only: power rule, linearity,
      constant-multiple/divisor rules, and linear `u`-substitution
      (`(ax+b)^n`) via a new `_sym_linear_shape` helper. Validated exactly
      per plan: `sym_diff(sym_integrate(f))` evaluated via `sym_eval`
      against `f` at sample points — landed clean on the first full test
      pass (12 tests, full SMT `vanic check` clean, both backends verified
      via `examples/integrate_demo.vani`).
- [x] ~~**vani-symbolic v0.6.0+**~~ ✅ published 2026-08-16 (as package v0.7.0)
      — polynomial factorization (rational-root theorem + synthetic
      division), folding in what would have been a separate
      `vani-polyalgebra` repo. Gröbner bases stayed out of scope, as
      planned. Rational-root detection is exact integer arithmetic
      (`q^deg * P(p/q)`, multiplying through rather than dividing — zero
      floating-point error); repeated-root multiplicity uses a numeric
      epsilon fallback, reusing `vani-algebra`'s already-published
      `algebra_poly_deflate` rather than reimplementing synthetic
      division. Symbolic-tree wrappers (`sym_poly_coeffs`/
      `sym_factor_rational`) generalize v0.5.0's degree≤2 extractor to
      arbitrary degree via a dynamically-grown `Vec<i64>`. A real bug (a
      zero constant term silently skipping the rest of the root search —
      `x^2-4x` reported only root `0`, missing `4`) was caught by
      `tests/test_factor.vani` on the very first test run, not by
      inspection. 12 tests, full SMT `vanic check` clean, both backends
      verified via `examples/factor_demo.vani`. This closes out the
      entire `vani-symbolic` roadmap.

---

## ML tier ✅ CLOSED 2026-08-16

Full scope, the closures/lifetime-variable question (resolved: no compiler
prerequisite, use the same arena pattern as vani-symbolic), and risk notes
per phase are in ROADMAP.md's
["`vani-ml` scoping breakdown"](ROADMAP.md#vani-ml-scoping-breakdown).

- [x] ~~**vani-ml v0.1.0**~~ ✅ published 2026-07-26 —
      classical ML: `linreg_*` (thin wrapper over vani-probability's MLR),
      `logreg_*` (cross-entropy loss, **own gradient-descent loop, not
      vani-optimize's** — its fixed `fn(ref Vec<f64>, i64) -> f64`
      objective/gradient signature can't carry training data through
      without a ref-capturing closure), `kmeans_*` (Lloyd's algorithm,
      genuinely new code), `train_test_split` (seeded Fisher-Yates),
      `mse`/`accuracy`/`precision`/`recall`/`f1_score`. 4 test files pass
      `vanic test` + full SMT `vanic check`; `vanic audit-safety` reports
      full `#[bounded_stack]` coverage (20/20 src functions) with no
      escape hatch. Example verified on both LLVM and C backends. Found
      and filed a real LLVM-backend crash along the way (BUG-6: standalone
      unary-minus float literal panics codegen; `vanic check` accepts it
      fine) — not fixed, worked around with `0.0 - 3.0`.
- [x] ~~**vani-ml v0.2.0**~~ ✅ built 2026-07-27 — data utilities: feature
      scaling, one-hot encoding, a `Dataset` struct (row-major, matches
      vani-matrix/vani-tensor convention), k-fold cross-validation.
- [x] ~~**vani-ml v0.3.0**~~ ✅ published 2026-08-16 — autodiff core: flat
      arena (`GraphNode` + `i64` child indices, same pattern as
      vani-symbolic, deliberately no symbol table since graphs are built
      directly, not parsed from names), forward eval and reverse-mode
      backward pass as two single linear loops (no recursion, no
      topological sort needed — the append-only arena is already
      topologically ordered by construction), gradients returned as a
      fresh `Vec<f64>` (not a `mut ref` out-parameter — no ref-capturing
      closures used anywhere, that's still compiler path-D, deferred
      indefinitely). **Highest-risk phase in this tier** — validated via
      finite-difference gradient checking cross-checked against
      `vani-calculus::diff_central`, against every node kind plus a
      dedicated DAG/shared-node-accumulation test and a composed
      multi-op test. `vani-tensor` turned out not to be a real
      dependency for this phase (deferred to v0.4.0, see ROADMAP.md).
- [x] ~~**vani-ml v0.4.0**~~ ✅ published 2026-08-16 — `relu`/`sigmoid`/
      `tanh`/`log` as new graph node kinds; dense layer and MSE/binary-
      cross-entropy losses composed from existing `graph_mul`/`graph_add`/
      `graph_sub`/`graph_const` primitives rather than new kinds — no
      `matrix`/`tensor` dependency needed, matmul emerges from scalar-op
      composition. `softmax` deliberately NOT added (inherently
      multi-output, a structural mismatch with the one-node-one-scalar
      design; this package's classification support has always been
      binary-only). 9 new finite-difference tests.
- [x] ~~**vani-ml v0.5.0**~~ ✅ published 2026-08-16 — SGD, Momentum, Adam
      over the graph's parameter vector. Confirmed (not just anticipated)
      vani-optimize's solvers don't fit this signature. `MomentumState`/
      `AdamState` returned fresh per step, not mutated in place. 6 new
      tests: hand-computed single-step exactness per optimizer plus a
      real convergence check per optimizer.
- [x] ~~**vani-ml v0.6.0+**~~ ✅ published 2026-08-16, CLOSED not left
      open — `shuffled_indices` utility plus one thorough worked example:
      a 2-2-1 MLP trained on XOR (the canonical problem a single
      dense+sigmoid layer provably cannot solve), all 9 params shared
      across all 4 training examples in one arena, loss ~0.72 → ~0.0001
      over 3000 Adam epochs on both backends. This tier's entire
      originally-scoped roadmap (v0.1.0-v0.6.0+) is now shipped.
- [x] ~~**BUG-197** (vani-compiler)~~ ✅ fixed 2026-08-16 — found while
      publishing this tier's 4 back-to-back version bumps: `vanic
      publish` fetched a package's current index file via its
      CDN-cached `download_url` instead of the metadata response's own
      `content` field, which silently dropped the `ml` v0.5.0 index
      entry when two publishes landed within the CDN's cache-lag
      window. Fixed upstream; the dropped entry was manually restored.

---

## Maintenance

- [x] ~~Drop `vani-bignum`'s `--allow-partial-safety-coverage` escape hatch~~
      ✅ done 2026-07-25 — republished as v0.1.1 with `#[bounded_stack(bytes
      = 257)]` on `BigInt_eq`, `vanic audit-safety` reports full clean
      coverage. Verified via AOT build (LLVM + C) in a fresh scratch project.
- [x] ~~`ROADMAP.md` structural pass: remove the redundant "Scientific
      computing (aggregate)" gap-analysis row~~ ✅ done 2026-07-25 — removed
      the row and its footnote, renumbered the remaining two ("Known gaps"
      section is now ¹² instead of ¹²³), left a one-line note explaining
      the removal in place.

---

## Tracked upstream, not owned here

- **Big-O tool nesting-depth heuristic** disagrees with correct algorithmic
  analysis on amortized bounds (found on vani-discrete's `disc_scc_kosaraju`,
  during MAINT-1). This is a `vani-compiler`-side issue, not a kosh-index
  package gap — see `vani-compiler/docs/` for status before picking it up
  from this side.

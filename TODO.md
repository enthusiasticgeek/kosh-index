# kosh-index — TODO

Task list distilled from [`ROADMAP.md`](ROADMAP.md), which stays the source of
truth for scope/rationale — this file is just the actionable checklist. Last
distilled: 2026-07-25.

> **Context**: the numeric/scientific tier (12 packages, every gap-analysis
> row, all 5 MAINT items) is fully shipped. Everything below is what's left.

---

## Symbolic tier (optional — confirm before starting each phase)

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
- [ ] **vani-symbolic v0.5.0** — simple equation solving: linear directly,
      quadratic by reusing vani-algebra's existing closed-form solver. Cheap;
      can be pulled ahead of v0.4.0 if integration stalls.
- [ ] **vani-symbolic v0.4.0** (optional/stretch) — basic symbolic
      integration via a fixed pattern table (no general/Risch algorithm).
      Validate every firing rule via `sym_diff(result) == original`.
- [ ] **vani-symbolic v0.6.0+** — polynomial factorization (rational-root
      theorem + synthetic division), folding in what would have been a
      separate `vani-polyalgebra` repo. Gröbner bases stay out of scope
      unless a real use case shows up.

---

## ML tier (optional — confirm before starting each phase)

Full scope, the closures/lifetime-variable question (resolved: no compiler
prerequisite, use the same arena pattern as vani-symbolic), and risk notes
per phase are in ROADMAP.md's
["`vani-ml` scoping breakdown"](ROADMAP.md#vani-ml-scoping-breakdown).

- [x] ~~**vani-ml v0.1.0**~~ ✅ built 2026-07-25, **not yet published** —
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
      fine) — not fixed, worked around with `0.0 - 3.0`. Stopping before
      `vanic publish` per this package's plan — awaiting go-ahead.
- [ ] **vani-ml v0.2.0** — data utilities: feature scaling, one-hot
      encoding, a `Dataset` struct (row-major, matches vani-matrix/
      vani-tensor convention), k-fold cross-validation.
- [ ] **vani-ml v0.3.0** — autodiff core: flat arena (`Vec<Node>` + `i64`
      child indices, same pattern as vani-symbolic), forward eval,
      reverse-mode backward pass via explicit `mut ref Vec<f64>` gradient
      buffers (not ref-capturing closures — those are compiler path-D,
      deferred indefinitely, see ROADMAP.md). **Highest-risk phase in this
      tier** — validate via finite-difference gradient checking against
      every node kind, not hand-picked examples.
- [ ] **vani-ml v0.4.0** — dense/linear layer, activations (relu/sigmoid/
      tanh/softmax), losses (MSE, cross-entropy) as graph node kinds.
- [ ] **vani-ml v0.5.0** — optimizers over the graph's parameter vector:
      SGD, momentum, Adam. New code (vani-optimize's solvers don't fit
      this signature), same underlying math.
- [ ] **vani-ml v0.6.0+** (optional/stretch) — training-loop utilities,
      batching, worked small-MLP examples. Revisit scope once the core
      phases are shipped and actually used.

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

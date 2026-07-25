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
Not started.

- [ ] **vani-symbolic v0.1.0** — arena (`Vec<Node>` + `i64` child indices) +
      node-kind tags; builder functions (`sym_num`/`sym_var`/`sym_add`/...);
      `sym_eval` (numeric substitution); `sym_to_str` (precedence-aware
      printer); `sym_eq_structural`. Depends on: vani-bignum (published).
- [ ] **vani-symbolic v0.2.0** — simplification: constant folding, identities,
      canonical commutative-operand ordering, like-term collection.
      **Highest-risk phase in the whole tier** — budget the most review time;
      validate via property-based checks (`simplify(e)` vs `e` evaluated at
      sample points), not just hand-picked examples.
- [ ] **vani-symbolic v0.3.0** — symbolic differentiation (`sym_diff`):
      sum/product/quotient/chain/power rules. Validate against
      vani-calculus's `diff_central` at sample points.
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

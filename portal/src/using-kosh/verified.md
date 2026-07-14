# SMT-Verified Packages

Some packages carry a `"verified": true` flag in their index entry:

```json
{"name":"calculus","version":"1.0.0","deps":[],"cksum":"…","yanked":false,"verified":true}
```

## What "verified" means

The package author has used vāṇī's built-in SMT prover to formally verify
the safety properties of the library. Specifically, the package's public API
functions have machine-checked:

- **Preconditions** (`requires` clauses) — inputs that violate these are
  rejected at the call site.
- **Postconditions** (`ensures` clauses) — outputs are proven to satisfy the
  stated contract.
- **Bounds safety** — all array/Vec accesses are proven in-bounds; no
  out-of-bounds access is possible under the stated preconditions.
- **No overflow** — arithmetic operations are proven not to overflow for the
  stated input ranges (where applicable).

## What "verified" does NOT mean

- It does not mean the algorithm is numerically optimal.
- It does not cover the underlying C/LLVM backend's correctness.
- Preconditions are the caller's responsibility — if you call a verified
  function with inputs that violate its `requires` clause, behaviour is
  undefined even in verified code.

## How to read a verified function

```vani
// In calculus::integrate:
requires n > 0 and a <= b
ensures  result >= 0.0 or a > b
pub fn integrate(f: fn(f64) -> f64, a: f64, b: f64, n: i64) -> f64 { … }
```

The `requires` clause is your contract with the library. Pass `n = 0` and
the compiler will reject the call at compile time (if it can prove the
violation statically) or insert a runtime panic in debug mode.

## Achieving verified status for your package

See [Publishing → vanic publish](../publishing/vanic-publish.md) for the
`--verify` flag and the criteria the registry uses.

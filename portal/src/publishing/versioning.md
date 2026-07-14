# Versioning

Kosh follows [Semantic Versioning 2.0](https://semver.org/): `MAJOR.MINOR.PATCH`.

## The rules

| Change type | Version bump | Example |
|---|---|---|
| Bug fix, no API change | PATCH | `0.1.0 → 0.1.1` |
| New public API, backwards-compatible | MINOR | `0.1.0 → 0.2.0` |
| Breaking change to existing public API | MAJOR | `0.1.0 → 1.0.0` |

## What counts as a breaking change

- Removing or renaming a `pub` function, type, or constant
- Changing a function's parameter types or count
- Narrowing a `requires` clause (accepting fewer inputs)
- Widening an `ensures` clause (promising less)
- Changing the return type

## What is NOT a breaking change

- Adding a new `pub` function or type
- Widening a `requires` clause (accepting more inputs)
- Narrowing an `ensures` clause (promising more)
- Internal refactoring with no observable API difference
- Performance improvements

## The 0.x convention

While your package is at `0.x`, a MINOR bump may contain breaking changes.
Declare stability by releasing `1.0.0` when the API is ready.

Consumers using `^0.1` pin to `>=0.1.0, <0.2.0` — they are protected from
your minor bumps during the `0.x` phase.

## Pre-release versions

Use SemVer pre-release identifiers for unstable snapshots:

```
0.2.0-alpha.1
0.2.0-beta.3
0.2.0-rc.1
```

Pre-release versions are not matched by range specifiers like `^0.1` — they
must be pinned exactly (`= 0.2.0-alpha.1`). Use them for early feedback only.

## Deprecation

Before a breaking change, publish a MINOR version that marks the old API
as deprecated (add a comment / doc note). Give consumers at least one
release cycle to migrate before the MAJOR bump removes it.

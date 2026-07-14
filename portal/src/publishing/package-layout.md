# Package Layout

A kosh package is a directory containing a `vani.toml` manifest and one or
more `.vani` source files.

## Minimal layout

```
my-package/
  vani.toml
  src/
    lib.vani       ← public API lives here
```

## Recommended layout

```
my-package/
  vani.toml
  src/
    lib.vani       ← re-exports the public surface
    internal.vani  ← private helpers (not re-exported)
  tests/
    integration.vani
  examples/
    basic.vani
  README.md
  LICENSE
```

---

## vani.toml — the manifest

```toml
[package]
name        = "my-package"
version     = "0.1.0"
description = "A short description of what this package does."
license     = "MIT"
entry       = "src/lib.vani"
authors     = ["Your Name <you@example.com>"]
repository  = "https://github.com/you/my-package"
keywords    = ["math", "numerics"]

[deps]
# no dependencies for this example
```

### Required fields

| Field | Description |
|---|---|
| `name` | Package name — lowercase, hyphens allowed, no spaces |
| `version` | SemVer string (`"X.Y.Z"`) |
| `license` | SPDX identifier, e.g. `"MIT"`, `"Apache-2.0"`, `"MIT OR Apache-2.0"` |
| `entry` | Path to the root source file relative to `vani.toml` |

### Optional but strongly recommended

| Field | Description |
|---|---|
| `description` | One-line summary shown in the package catalog |
| `authors` | Author list |
| `repository` | GitHub URL — required for `vanic publish` to upload the release asset |
| `keywords` | Up to 5 search tags |

---

## Naming rules

- Lowercase letters, digits, and hyphens only: `^[a-z][a-z0-9-]*$`
- No leading or trailing hyphens
- Maximum 64 characters
- Names are first-come-first-served in the registry

---

## Public API

Everything declared `pub` at the top level of `entry` is part of your
public API. Private items (no `pub`) are not accessible to consumers.

Design your public API surface carefully — a breaking change requires a
major version bump (see [Versioning](versioning.md)).

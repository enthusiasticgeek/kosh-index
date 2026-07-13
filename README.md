# kosh-index

<img src="kosh-index_logo.png" width="50%">

Sparse package registry index for the [vāṇī compiler](https://github.com/enthusiasticgeek/vani-compiler).

**Registry URL:** `https://enthusiasticgeek.github.io/kosh-index`

---

## Using a package from this registry

In your `vani.toml`:

```toml
[package]
name    = "myapp"
version = "0.1.0"
entry   = "src/main.vani"

[deps]
mathlib = { registry = "kosh", version = "^1.0" }
```

Then run:

```
vanic add mathlib        # fetch + lock
vanic build
```

## Submitting a package

```
vanic publish            # builds tarball, uploads release asset, pushes index entry
```

Requires a GitHub token with `repo` and `write:packages` scopes in `KOSH_TOKEN`.

---

## Index layout

```
config.json          ← registry metadata (dl URL template, api endpoint)
index/
  <name>.json        ← one JSON line per published version
```

Each line in `index/<name>.json`:

```json
{"name":"mathlib","version":"1.0.0","deps":[],"cksum":"<sha256>","yanked":false}
```

---

## Authority

v1: `enthusiasticgeek` is the sole publish authority.  
Packages are first-come first-served by name.  
SMT-verified packages carry `"verified": true` in the index entry.

Publisher access may be revoked or a publisher permanently blacklisted for
violations of the [Publisher Agreement](PUBLISHER_AGREEMENT.md). Violation
categories, escalation steps, cure timelines, and the appeal path are defined
in [ENFORCEMENT.md](ENFORCEMENT.md).

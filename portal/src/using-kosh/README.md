# Adding a Dependency

## Quick start

1. Declare the dependency in `vani.toml`:

```toml
[package]
name    = "myapp"
version = "0.1.0"
entry   = "src/main.vani"

[deps]
calculus = { registry = "kosh", version = "^0.1" }
```

2. Fetch and lock it:

```
vanic add calculus
```

3. Import in your source file:

```vani
use calculus::integrate;

fn main() -> f64 {
    return integrate(|x: f64| -> f64 { return x * x; }, 0.0, 1.0, 1000);
}
```

4. Build:

```
vanic build
```

---

## The `[deps]` table

Each entry is `name = { registry = "kosh", version = "<range>" }`.

| Field | Required | Description |
|---|---|---|
| `registry` | yes | Must be `"kosh"` for this registry |
| `version` | yes | SemVer range (see [Version Pinning](lockfile.md)) |

---

## Useful commands

| Command | Effect |
|---|---|
| `vanic add <name>` | Resolves latest matching version, fetches tarball, writes lockfile |
| `vanic add <name>@1.2.3` | Pin to an exact version |
| `vanic update` | Re-resolve all deps to newest matching versions |
| `vanic build` | Build using locked dependency versions |
| `vanic clean` | Remove cached tarballs |

---

## Where packages live

Tarballs are downloaded from GitHub Releases on the package's own repository.
The registry index (this repo) only stores metadata — version numbers,
checksums, and dependency lists. The compiler verifies the SHA-256 checksum
before unpacking.

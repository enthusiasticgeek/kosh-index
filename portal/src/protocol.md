# Registry Protocol

This page documents the sparse-index protocol implemented by Kosh. Use it if
you are building a tool that consumes or mirrors the registry.

---

## Endpoint

```
https://enthusiasticgeek.github.io/kosh-index
```

All paths below are relative to this root.

---

## config.json

`GET /config.json`

```json
{
  "dl":           "https://github.com/enthusiasticgeek/{name}/releases/download/{name}-v{version}/{name}-{version}.tar.gz",
  "api":          "https://enthusiasticgeek.github.io/kosh-index",
  "auth-required": false
}
```

| Field | Description |
|---|---|
| `dl` | Download URL template; `{name}` and `{version}` are substituted per-package |
| `api` | Base URL of this registry |
| `auth-required` | `false` — anonymous read is allowed |

---

## Package index entries

`GET /index/<name>.json`

One JSON object per line (newline-delimited). Each line represents one
published version of the package.

```json
{"name":"calculus","version":"0.1.0","deps":[],"cksum":"4131d2cd…","yanked":false}
```

| Field | Type | Description |
|---|---|---|
| `name` | string | Package name (matches filename) |
| `version` | string | SemVer version string |
| `deps` | array | Dependency list (see below) |
| `cksum` | string | SHA-256 hex digest of the tarball |
| `yanked` | bool | `true` if this version has been yanked |
| `verified` | bool? | `true` if the package passed SMT proof (optional field) |

### Dependency entries

```json
{
  "name": "otherpkg",
  "req": "^1.0",
  "optional": false,
  "default_features": true,
  "features": [],
  "registry": null
}
```

`registry: null` means the same registry. A non-null value is a URL
pointing to a different registry's `config.json`.

---

## Download

Tarballs are hosted as GitHub Release assets on the package's own repository.
The download URL is constructed from `config.json → dl` by substituting
`{name}` and `{version}`.

Example for `calculus 0.1.0`:

```
https://github.com/enthusiasticgeek/vani-calculus/releases/download/calculus-v0.1.0/calculus-0.1.0.tar.gz
```

The client must verify the SHA-256 of the downloaded tarball against the
`cksum` field before unpacking.

---

## Tarball format

A `.tar.gz` archive containing the package source. The top-level directory
inside the archive must be `<name>-<version>/`. The `vani.toml` manifest must
be at `<name>-<version>/vani.toml`.

---

## Authentication

`auth-required` is `false`. All index and tarball downloads are public and
anonymous. Publishing requires a GitHub token — see
[vanic publish](publishing/vanic-publish.md).

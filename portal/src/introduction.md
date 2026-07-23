# Kosh — vāṇī Package Registry

<img class="kosh-logo" src="images/logo/kosh-index_logo.png" alt="Kosh Index logo"/>

**Kosh** (कोश, *storehouse*) is the official package registry for the
[vāṇī compiler](https://github.com/enthusiasticgeek/vani-compiler).

It lets you share and reuse vāṇī libraries — from numerical calculus to
probability distributions — with a single line in your `vani.toml` and one
command to fetch them.

```
vanic add calculus
```

---

## What this site is

| Audience | Go to |
|---|---|
| I want to **use** a package in my project | [Adding a Dependency](using-kosh/README.md) |
| I want to **publish** my library | [Publishing Overview](publishing/README.md) |
| I'm building a **kosh-compatible tool** | [Registry Protocol](protocol.md) |
| I want to know who controls what | [Governance](governance.md) |

---

## Registry endpoint

```
https://enthusiasticgeek.github.io/kosh-index
```

This URL is the value of `registry = "kosh"` in your `vani.toml`.
The compiler resolves it to fetch `config.json` and `index/<name>.json`
automatically — you never need to type the URL yourself.

---

## Available packages

Three packages are published today:

| Package | Version | Description |
|---|---|---|
| [`calculus`](catalog.md#calculus) | 0.1.0 | Numerical calculus (integration, differentiation) |
| [`probability`](catalog.md#probability) | 0.1.0 | Probability distributions and statistics |
| [`hello-kosh`](catalog.md#hello-kosh) | 0.2.0 | Minimal example / getting-started template |

Browse the full [Package Catalog](catalog.md).

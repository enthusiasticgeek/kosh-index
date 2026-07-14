# Publishing Overview

Publishing to Kosh is **gated**: you apply once, get approved, then
`vanic publish` works from any of your repositories.

---

## The four steps

### Step 1 — Read the Publisher Agreement

```
vanic apply-publisher
```

This fetches and prints the full
[Publisher Agreement](https://github.com/enthusiasticgeek/kosh-index/blob/main/PUBLISHER_AGREEMENT.md).
Read every section — it covers intellectual property, conduct, and the
operator's right to yank or remove packages.

### Step 2 — Accept and submit your application

```
vanic apply-publisher --accept-agreement
```

This opens a public GitHub issue in the kosh-index repository titled
**"Publisher application: \<your-username\>"** and records your acceptance.
Your GitHub account (`gh auth login`) must be authenticated first.

### Step 3 — Wait for review

The registry operator (`enthusiasticgeek`) reviews applications within a few
days. You'll be notified on the issue when:

- ✅ **Approved** — your username is added to `governance.json →
  allowed_publishers`. You can now run `vanic publish`.
- ❌ **Denied** — the issue is closed with an explanation.

### Step 4 — Publish

From your package root (where `vani.toml` lives):

```
vanic publish
```

See [vanic publish](vanic-publish.md) for the full publish workflow.

---

## What you need before publishing

| Requirement | Detail |
|---|---|
| Approved publisher | Your username in `allowed_publishers` |
| `vani.toml` with `[package].version` | SemVer string, e.g. `"0.1.0"` |
| License declaration | `license = "MIT"` in `[package]` |
| `gh` CLI authenticated | `gh auth login` |
| `KOSH_TOKEN` env var | GitHub token with `repo` + `write:packages` scopes |

---

## Questions?

Open an issue on [kosh-index](https://github.com/enthusiasticgeek/kosh-index)
with the label `question`.

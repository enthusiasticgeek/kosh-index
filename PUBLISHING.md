# How to Publish to the Vāṇī Kosh Registry

Publishing is **gated**: you must read the legal agreement, apply, and be
approved by the registry operator before `vanic publish` will work.

---

## Step 1 — Read the Publisher Agreement

```
vanic apply-publisher
```

This fetches and prints the [PUBLISHER_AGREEMENT.md](PUBLISHER_AGREEMENT.md).
Read every section before proceeding.

---

## Step 2 — Submit your application

```
vanic apply-publisher --accept-agreement
```

This:
1. Verifies your `gh` CLI is authenticated (`gh auth login` first if needed).
2. Checks you are not already approved or blacklisted.
3. Opens a public GitHub issue in this repository titled
   **"Publisher application: \<your-username\>"** with a record of your
   agreement acceptance.

You will receive a confirmation with the issue URL.

---

## Step 3 — Wait for operator review

The registry operator (`enthusiasticgeek`) will review your application. This
typically takes a few days. You will be notified via the GitHub issue when:

- ✅ **Approved** — your username is added to `governance.json →
  allowed_publishers`. You can now run `vanic publish`.
- ❌ **Denied** — the issue is closed with an explanation.

---

## Step 4 — Publish your package

Once approved, from your package's root directory:

```
vanic publish
```

Requirements:
- `vani.toml` must have `[package].version = "X.Y.Z"`.
- Your `vani.toml` or package must include a license declaration.
- The `gh` CLI must be installed and authenticated.

---

## Governance

The authorised publisher list, pending applications, and blacklist all live in
[`governance.json`](governance.json). The Operator controls this file.

When the vāṇī project grows, governance may transfer to a committee. The
`allowed_publishers` list in `governance.json` is the single source of truth —
no compiler update is needed for a governance change.

---

## Blacklisting and Appeals

If you are blacklisted, `vanic publish` will show:

```
publish rejected: 'username' has been blacklisted from this registry.
Reason: <reason>
Since: <date>
To appeal, open an issue at https://github.com/enthusiasticgeek/kosh-index
```

To appeal, open an issue labelled **`blacklist-appeal`** explaining your
situation.

---

## Reporting a violation

If you believe a published package contains malware, violates the agreement, or
infringes your rights:

1. Open an issue in this repository.
2. Apply the appropriate label: `violation-report`, `takedown-request`, or
   `security`.

The Operator will review and remove offending packages promptly.

---

## Contact

All governance communication happens via GitHub issues in this repository:
**https://github.com/enthusiasticgeek/kosh-index**

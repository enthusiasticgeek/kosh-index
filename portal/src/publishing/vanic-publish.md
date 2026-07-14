# vanic publish

## What the command does

`vanic publish` performs these steps automatically:

1. Reads `vani.toml` to get `name`, `version`, `repository`, and `license`.
2. Verifies your username appears in `governance.json → allowed_publishers`.
3. Checks the version does not already exist in `index/<name>.json`.
4. Builds a `.tar.gz` tarball of your package source.
5. Computes the SHA-256 checksum of the tarball.
6. Uploads the tarball as a GitHub Release asset on your repository
   (tag: `<name>-v<version>`).
7. Appends a new JSON line to `index/<name>.json` in the kosh-index
   repository and opens a pull request (or commits directly if you have
   write access).

## Running it

From your package root:

```
export KOSH_TOKEN=ghp_your_token_here
vanic publish
```

`KOSH_TOKEN` must have `repo` and `write:packages` scopes.

## Dry run

```
vanic publish --dry-run
```

Validates your manifest, builds the tarball, and prints what *would* happen
without uploading anything.

## SMT-verified flag

```
vanic publish --verify
```

Before publishing, runs the full SMT proof pass over your entire public API.
If all `requires`/`ensures` clauses pass, the index entry is emitted with
`"verified": true`. Any proof failure is a hard error — fix it before
publishing.

See [SMT-Verified Packages](../using-kosh/verified.md) for what this means
for consumers.

## Yanking a version

```
vanic yank <name> <version>
```

Sets `"yanked": true` on the index entry. Yanked versions are skipped by
`vanic add` but existing lockfiles that pin a yanked version still build
(with a warning). You cannot un-yank a version — publish a new patch version
with the fix instead.

## Re-publishing is not allowed

Once `name@version` is published, it cannot be overwritten. Increment the
version and publish again.

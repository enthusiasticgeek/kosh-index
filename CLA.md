# Contributor License Agreement — Vāṇी Kosh Registry

**Project**: Vāṇī Kosh Registry (`kosh-index`)
**Operator / Maintainer**: Pratik M. Tambe &lt;enthusiasticgeek@gmail.com&gt;
**Agreement version**: 1.0 — 2026-07-10

> This CLA governs **code contributions to the kosh-index repository itself**
> (e.g. changes to `governance.json`, `config.json`, tooling scripts, or
> documentation). It is separate from the
> [Publisher Agreement](PUBLISHER_AGREEMENT.md), which governs the right to
> publish packages to the registry.
>
> **Note**: This agreement was drafted in good faith but has not been reviewed
> by a licensed attorney. Both parties should seek independent legal counsel
> for any serious legal matter.

---

## 1. Definitions

**"You"** means the individual or legal entity making a Contribution.

**"Contribution"** means any original work of authorship submitted by You for inclusion in the kosh-index repository — including changes to registry configuration, tooling, documentation, or governance files.

**"Registry"** means the Vāṇī Kosh sparse index and associated infrastructure at <https://github.com/enthusiasticgeek/kosh-index>.

**"Operator"** means Pratik M. Tambe, the current sole operator of the Registry.

**"Approved Contributor"** means a person listed in [`CONTRIBUTORS_APPROVED.md`](CONTRIBUTORS_APPROVED.md) following explicit written approval by the Operator.

---

## 2. Contributor Access — Application and Approval

**Contribution access is not automatic.** Pull requests from unapproved GitHub usernames will not be merged.

### Step 1 — Open a Contributor Application Issue

Open a **GitHub Issue** in the `kosh-index` repository with the title:

```
[CLA] Contributor application — @your-github-username
```

Include the following:

```
### Contributor Application

**GitHub username**: @[your-github-username]
**Full legal name**: [Your Full Legal Name]
**Email**: [your@email.com]
**Affiliation**: [employer / university / "Independent"]

### Relevant background
[Describe your experience relevant to registry infrastructure, package
 management, JSON/TOML tooling, CI/CD, or related areas. Link to
 public work if available.]

### Intended contribution
[Describe what you want to contribute: governance tooling, config changes,
 documentation, bug fixes, etc.]

### CLA Declaration
I have read the Vāṇī Kosh Registry Contributor License Agreement (CLA.md v1.0)
and agree to its terms.

Signed: [Your Full Legal Name]
Date:   [YYYY-MM-DD]
```

### Step 2 — Operator Review

The Operator reviews your GitHub profile, credentials, and stated contribution area. Review may take up to two weeks. The Operator may approve, request more information, or decline at sole discretion.

Upon approval, your username is added to [`CONTRIBUTORS_APPROVED.md`](CONTRIBUTORS_APPROVED.md).

### Step 3 — Open Pull Requests

Reference your approval issue number in your first PR.

---

## 3. Copyright License Grant

You hereby grant to the Operator, and to all recipients of software distributed by the Operator, a **perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable** copyright license to reproduce, prepare derivative works of, publicly display, publicly perform, sublicense, and distribute Your Contributions and any derivative works thereof.

---

## 4. Patent License Grant

You hereby grant to the Operator a **perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable** patent license to make, have made, use, offer to sell, sell, import, and otherwise transfer the Registry and any derivative works, limited to patent claims necessarily infringed by Your Contribution(s) alone or in combination with the Registry.

---

## 5. Representations

By applying and contributing, You represent that:

1. You are legally entitled to grant the licenses above.
2. Each Contribution is Your original creation or You have the right to submit it.
3. You have disclosed all relevant third-party licenses or restrictions.
4. The GitHub account used to apply is under Your sole control and accurately identifies You.

---

## 6. Forking Policy

The kosh-index repository is published under the MIT License. Forking for study or to operate a separate registry is permitted under the MIT License. However:

- **Operating a competing package registry** using this codebase is permitted under MIT but the Operator requests prior notification at &lt;enthusiasticgeek@gmail.com&gt; to avoid user confusion.
- **Contributing back** always requires the approval process in §2.
- The `governance.json` approval list in this repository is authoritative only for **this** registry. A fork must maintain its own publisher approval list.

---

## 7. Revocation

Contributor approval may be revoked for violation of this Agreement, harmful conduct, or at the Operator's sole discretion. Revocation removes the username from [`CONTRIBUTORS_APPROVED.md`](CONTRIBUTORS_APPROVED.md).

---

## 8. No Warranty

Contributions are provided "AS IS", without warranty of any kind.

---

## 9. Governing Law

This Agreement is governed by applicable law. Disputes shall first be addressed through good-faith written negotiation. If unresolved after 30 days, the parties agree to binding arbitration.

---

## Questions

Open a GitHub Issue in the `kosh-index` repository or contact the Operator at &lt;enthusiasticgeek@gmail.com&gt;.

---

*Revision: 2026-07-10 v1.0.*

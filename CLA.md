# Contributor License Agreement — Vāṇी Kosh Registry

**Project**: Vāṇी Kosh Registry (`kosh-index`)
**Operator / Maintainer**: Pratik M. Tambe &lt;enthusiasticgeek@gmail.com&gt;
**Agreement version**: 1.1 — 2026-07-10

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

**"Registry"** means the Vāṇी Kosh sparse index and associated infrastructure at <https://github.com/enthusiasticgeek/kosh-index>.

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

### Employment and contractual independence declaration
**Current employer (or "Self-employed" / "Student" / "Unemployed")**:
[Name of employer, or status]

**Does your employer operate in a field related to package registries,
developer tooling, or software infrastructure?** (yes / no / N/A):
[answer]

**Have you reviewed your employment contract, IP assignment agreement,
non-compete, moonlighting clause, or any other contractual obligation
that could apply to this Contribution?** (yes / no / N/A):
[answer]

**Do any of those agreements restrict or prohibit this Contribution?**
(yes / no — if yes, explain or do not apply):
[answer]

**If your employer could claim any rights to this Contribution, have you
obtained written employer permission?** (yes / N/A):
[answer or "N/A — contribution is entirely independent of my employment"]

### CLA Declaration
I have read the Vāṇī Kosh Registry Contributor License Agreement (CLA.md v1.1)
and agree to its terms, including the Employment Independence and
Indemnification provisions (§6).

I declare that the employment information above is accurate and complete.
I understand that any legal issues arising from my violation of employment
or contractual obligations are solely my responsibility, and I agree to
defend and indemnify the Operator against any resulting claims.

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

Subject to the terms of this Agreement (and in particular subject to the representations and indemnification in §§5–6), You hereby grant to the Operator, and to all recipients of software distributed by the Operator, a **perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable** copyright license to reproduce, prepare derivative works of, publicly display, publicly perform, sublicense, and distribute Your Contributions and any derivative works thereof.

---

## 4. Patent License Grant

You hereby grant to the Operator a **perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable** patent license to make, have made, use, offer to sell, sell, import, and otherwise transfer the Registry and any derivative works, limited to patent claims necessarily infringed by Your Contribution(s) alone or in combination with the Registry.

---

## 5. Representations

By applying and contributing, You represent that:

1. **Entitlement** — You are legally entitled to grant the licenses in §§3–4.

2. **Employment and contractual clearance** — You have reviewed every employment agreement, contractor agreement, IP assignment clause, moonlighting or outside-activity restriction, non-compete agreement, and any other contractual or fiduciary obligation binding on You, and You confirm that:
   - making this Contribution does **not** violate any such obligation;
   - Your employer (if any) does **not** hold or could not reasonably claim ownership of this Contribution; and
   - if Your employer operates in a field related to package registries or software infrastructure, You have either obtained written permission or confirmed that the Contribution is independent of your employment duties.

3. **Original authorship** — Each Contribution is Your original creation or You have the right to submit it, as disclosed.

4. **Third-party notices** — You have disclosed all relevant third-party licenses or restrictions.

5. **Accurate identity and disclosure** — The GitHub account used to apply is under Your sole control and accurately identifies You. The employment information in Your application is accurate and complete. Applications made under false identity or with materially false employment disclosures are void.

---

## 6. Employment Independence and Indemnification

### 6.1 Sole responsibility for employment compliance

**Your compliance with your own employment agreements is entirely your responsibility.** The Operator and the Registry are **not liable** for any consequences arising from Your decision to contribute in violation of those agreements.

### 6.2 Indemnification

**You agree to defend, indemnify, and hold harmless** the Operator (Pratik M. Tambe), and all downstream recipients of the Registry's software, from and against any and all third-party claims, demands, actions, damages, losses, liabilities, costs, and expenses (including reasonable attorneys' fees) arising out of or related to:

- (a) any breach or alleged breach by You of any employment agreement, IP assignment clause, moonlighting restriction, non-compete agreement, or other contractual obligation;
- (b) any claim by Your employer or any third party that Your Contribution was made in violation of their intellectual property rights by reason of Your employment relationship;
- (c) any material misrepresentation or omission in Your Contributor Application regarding employment status or contribution ownership; or
- (d) any false statement made in the CLA Declaration.

This indemnification obligation survives revocation of Your contributor status.

### 6.3 Notification obligation

If You become aware of any restriction that could affect Your representations in §5, You must notify the Operator in writing within 14 days at &lt;enthusiasticgeek@gmail.com&gt;. Failure to notify is a material breach of this Agreement.

---

## 7. Forking Policy

The kosh-index repository is published under the MIT License. Forking for study or to operate a separate registry is permitted under the MIT License. However:

- **Operating a competing package registry** using this codebase is permitted under MIT but the Operator requests prior notification at &lt;enthusiasticgeek@gmail.com&gt; to avoid user confusion.
- **Contributing back** always requires the approval process in §2.
- The `governance.json` approval list in this repository is authoritative only for **this** registry. A fork must maintain its own publisher approval list.

---

## 8. Revocation

Contributor approval may be revoked for violation of this Agreement, harmful conduct, or at the Operator's sole discretion. Revocation removes the username from [`CONTRIBUTORS_APPROVED.md`](CONTRIBUTORS_APPROVED.md). Revocation does not relieve a former contributor of indemnification obligations under §6.2.

---

## 9. No Warranty

Contributions are provided "AS IS", without warranty of any kind.

---

## 10. Governing Law

This Agreement is governed by applicable law. Disputes shall first be addressed through good-faith written negotiation. If unresolved after 30 days, the parties agree to binding arbitration.

---

## Questions

Open a GitHub Issue in the `kosh-index` repository or contact the Operator at &lt;enthusiasticgeek@gmail.com&gt;.

---

*Revision: 2026-07-10 v1.1.*

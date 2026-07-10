# Contributor License Agreement — Vāṇी Kosh Registry

**Project**: Vāṇी Kosh Registry (`kosh-index`)
**Operator / Maintainer**: Pratik M. Tambe &lt;enthusiasticgeek@gmail.com&gt;
**Agreement version**: 1.2 — 2026-07-10

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
I have read the Vāṇī Kosh Registry Contributor License Agreement (CLA.md v1.2)
and agree to its terms, including the Patent Non-Assertion Covenant (§4a),
Patent Aggression Termination (§4b), and Employment Independence and
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

Subject to §§4a–4b, You hereby grant to the Operator a **perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable** patent license to make, have made, use, offer to sell, sell, import, and otherwise transfer the Registry and any derivative works, limited to patent claims necessarily infringed by Your Contribution(s) alone or in combination with the Registry. This license extends to patent claims held by Your employer to the extent necessarily infringed by Your Contribution.

---

## 4a. Patent Non-Assertion Covenant

You covenant, on behalf of yourself, your employer, successors, and assigns, **not to assert any patent claim** against the Registry, the Operator, or any user of the Registry's software, where such claim covers an invention first disclosed in this repository, substantially derived from Your Contribution, or implemented in the Registry at the time of Your Contribution. You further covenant not to file or fund any patent application on any such invention. This covenant runs with Your Contribution and binds Your successors and assigns.

---

## 4b. Patent Aggression Termination

If You, Your employer, or any entity acting on Your behalf institutes a patent infringement proceeding against the Registry, the Operator, or any downstream recipient, all licenses granted to You under §§3–4 and Your contributor approval under §2 **terminate automatically** on the date such proceeding is filed. Termination does not relieve You of indemnification obligations under §6.2.

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

## 10. Governing Law and Jurisdiction

**Intellectual property matters** (patents, copyright, trade secrets) arising
from this Agreement are governed by **United States federal law**, including
the Patent Act (35 U.S.C.), the Copyright Act (17 U.S.C.), and the Defend
Trade Secrets Act (18 U.S.C. § 1836). Federal courts of the United States
have exclusive jurisdiction over such matters.

**Contract and other disputes** are governed by the laws of the state in
which the Operator is domiciled at the time the dispute is initiated. If
the Operator is not domiciled in the United States at that time, the parties
agree that the **State of California** governs, and the state and federal
courts located in the Northern District of California shall have exclusive
jurisdiction.

**Dispute resolution sequence**:
1. **Written negotiation** — 30 days to resolve in good faith.
2. **Mediation** — 30 additional days before a mutually agreed neutral mediator.
3. **Binding arbitration** — JAMS rules (or small claims court if claim is under USD $10,000).
4. **Litigation** — last resort; prevailing party entitled to reasonable attorneys' fees.

**Patent matters** may be brought directly to federal court without exhausting steps 1–3.

> The Operator is domiciled in the United States. US jurisdiction is
> appropriate because the Registry was created and is maintained in the US
> and is hosted on GitHub, Inc. (a US company).

---

## 11. Export Compliance

You represent that Your Contribution does not originate from, and will not be
submitted by any person in, a country subject to US export controls or
sanctions (including but not limited to OFAC-sanctioned jurisdictions). You
are not listed on any US government denied-party list (Entity List, SDN List,
etc.).

---

## 12. Miscellaneous

**Severability**: If any provision of this Agreement is held unenforceable, it
shall be modified to the minimum extent necessary to make it enforceable; all
other provisions remain in full force.

**Entire Agreement**: This Agreement, together with [`PATENTS.md`](PATENTS.md)
and [`CONTRIBUTORS_APPROVED.md`](CONTRIBUTORS_APPROVED.md), constitutes the entire
agreement between You and the Operator regarding Your Contributions and
supersedes all prior discussions.

**No Waiver**: The Operator's failure to enforce any provision does not
constitute a waiver of the right to enforce it in the future.

**Modification**: The Operator may publish revised versions of this CLA. Each
version is identified by a version number. Contributions made after a new
version is published are governed by the new version. Contributions already
accepted under a prior version remain governed by that version.

**Assignment**: The Operator may assign this Agreement in connection with a
transfer or sale of substantially all assets of the Registry. You may not
assign your rights under this Agreement without written consent.

---

## Questions

Open a GitHub Issue in the `kosh-index` repository or contact the Operator at &lt;enthusiasticgeek@gmail.com&gt;.

---

*Revision: 2026-07-10 v1.2.*

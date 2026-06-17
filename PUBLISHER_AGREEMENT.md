# VĀṆĪ KOSH PUBLISHER AGREEMENT

**Version 1.0 — Effective 2026-06-17**

This Publisher Agreement ("Agreement") governs your right to publish packages
to the Vāṇī Kosh Registry ("Registry") at
`https://enthusiasticgeek.github.io/kosh-index/`. The Registry is operated by
Subramanya Sharma, GitHub user `enthusiasticgeek` ("Operator").

By submitting a publisher application via `vanic apply-publisher
--accept-agreement`, you ("Publisher") agree to be legally bound by every term
of this Agreement. **If you do not agree, do not submit an application.**

> **Note**: This agreement was drafted in good faith but has not been reviewed
> by a licensed attorney. Both parties should seek independent legal counsel
> for any serious legal matter.

---

## 1. Definitions

| Term | Meaning |
|------|---------|
| **Package** | Any source code archive, tarball, index entry, or associated metadata you upload to the Registry. |
| **Registry** | The Kosh sparse index and GitHub Release assets operated under the `kosh-index` repository. |
| **Operator** | The person(s) administering the Registry; currently `enthusiasticgeek`. May expand to a committee in future versions. |
| **Application** | A formal publisher access request submitted via `vanic apply-publisher`. |
| **Governance document** | `governance.json` in the `kosh-index` repository, which controls the authorised publisher list, pending list, and blacklist. |

---

## 2. Publisher Application and Approval

**2.1. Access is not automatic.** To publish you must:

  a. Read this Agreement in full.  
  b. Run `vanic apply-publisher --accept-agreement`, which opens a public
     GitHub issue in the `kosh-index` repository with a timestamped record
     of your acceptance.  
  c. Wait for the Operator to review your application and add your GitHub
     username to `governance.json → allowed_publishers`.

**2.2. Operator discretion.** The Operator may approve or deny any application
at their sole discretion, with or without explanation.

**2.3. Revocability.** Approval is not permanent. Access may be revoked at any
time (§6).

**2.4. Identity.** You represent that the GitHub account you use to apply is
under your sole control and accurately identifies you or your organisation.
Applications made under false identity are void.

---

## 3. Permitted Use

You may publish Packages that:

- Contain **vāṇī source code** (`*.vani` files and a valid `vani.toml`).
- Carry a **clearly declared open-source or source-available license** (MIT,
  Apache-2.0, GPL, etc.) stated in `vani.toml` or a `LICENSE` file inside the
  Package.
- You **own** or hold sufficient rights to distribute.
- Serve a **legitimate software development purpose**.

---

## 4. Prohibited Content

You **must not** publish Packages that contain, link to, or are designed to
deliver:

**4.1 Malware** — including but not limited to:
  viruses, worms, trojans, ransomware, spyware, adware, rootkits, keyloggers,
  browser hijackers, cryptominers operating without explicit user consent,
  or any code whose primary or secondary purpose is to damage, disrupt,
  surveil, or gain unauthorised access to computer systems or data.

**4.2 Deceptive packages** — including:
  - Typosquatting (names that impersonate other packages by one or two
    character differences to trick users into installing the wrong package).
  - Packages that misrepresent their functionality, origin, or authorship.
  - Empty or placeholder packages published solely to block a name.

**4.3 Illegal content** — anything that violates applicable law, including:
  - Copyright or trademark infringement.
  - Child sexual abuse material (CSAM) or any content sexualising minors.
  - Code that facilitates illegal surveillance, stalking, or harassment.
  - Software that violates export control or sanctions laws (ITAR, EAR, OFAC).

**4.4 Harmful data extraction** — code that exfiltrates credentials, private
  keys, environment variables, personal data, or any sensitive information
  without **explicit prior user consent** and **clear in-Package disclosure**.

**4.5 Destructive payloads** — code that intentionally deletes files, corrupts
  databases, bricks hardware, or performs destructive or irreversible actions
  beyond the Package's documented and stated purpose.

**4.6 Namespace abuse** — registering names with no intent to publish working
  software, or publishing trivially-different versions solely to consume index
  space.

---

## 5. Publisher Responsibilities

**5.1 Accuracy.** Package name, version, and description must accurately
describe the Package's contents and functionality.

**5.2 Ownership.** You represent and warrant that you own or hold sufficient
rights to all content in your Package, and that publishing it does not violate
any third-party intellectual property rights.

**5.3 License.** Every published Package must include a license declaration.
Packages without a license may be removed without notice.

**5.4 Security response.** If you discover a security vulnerability in a
Package you have published, you must within a reasonable time either:

  a. Release a patched version and yank the vulnerable version(s), or  
  b. Yank all affected versions and notify the Operator via a GitHub issue
     labelled `security`.

**5.5 Legal compliance.** You must comply with all applicable laws and
regulations in your jurisdiction when publishing Packages.

**5.6 No automated harm.** You must not use the `vanic publish` command or the
Registry API in an automated pipeline designed to mass-publish harmful content
or circumvent access controls.

---

## 6. Operator Rights

**6.1 Revocation.** The Operator may revoke publishing access at any time, for
any reason, including but not limited to: violation of this Agreement,
sustained inactivity, account compromise, or transfer of registry governance.

**6.2 Package removal.** The Operator may remove any Package or any individual
version that violates this Agreement or applicable law, without prior notice.

**6.3 Blacklisting.** The Operator may permanently ban a GitHub username from
the Registry by adding an entry to `governance.json → blacklisted`. Blacklisted
users are blocked at the `vanic publish` command level. A banned user may
appeal by opening a GitHub issue labelled `blacklist-appeal` in `kosh-index`.

**6.4 Governance transfer.** The Operator may transfer registry governance to a
committee or successor entity. The `governance.json` file controls who may
publish; this Agreement continues to bind Publishers under the successor model.
No compiler update is required for a governance change.

**6.5 No hosting obligation.** The Operator is under no obligation to host,
preserve, cache, or distribute any Package in perpetuity.

---

## 7. Disclaimer of Warranty

THE REGISTRY IS PROVIDED **"AS IS"**, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR
PURPOSE, OR NON-INFRINGEMENT. THE OPERATOR DOES NOT WARRANT THAT THE REGISTRY
WILL BE UNINTERRUPTED, ERROR-FREE, OR FREE OF HARMFUL CONTENT. YOUR USE OF
ANY PACKAGE FROM THE REGISTRY IS ENTIRELY AT YOUR OWN RISK.

---

## 8. Limitation of Liability

TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, THE OPERATOR SHALL NOT BE
LIABLE FOR ANY INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, OR PUNITIVE
DAMAGES ARISING OUT OF OR RELATED TO THIS AGREEMENT OR THE REGISTRY, EVEN IF
THE OPERATOR HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.

---

## 9. Indemnification

You agree to indemnify, defend, and hold harmless the Operator from and against
any and all claims, damages, losses, liabilities, costs, and expenses
(including reasonable attorneys' fees) arising out of or related to:

  a. Your violation of any provision of this Agreement;  
  b. Content, code, or functionality in any Package you publish; or  
  c. Your violation of any applicable law or third-party right.

---

## 10. Governing Law and Dispute Resolution

This Agreement is governed by the laws of the United States of America and the
State of California, without regard to conflict-of-law principles. Any dispute
arising under this Agreement that cannot be resolved informally shall be
submitted to binding arbitration under the rules of the American Arbitration
Association, except that either party may seek injunctive or other equitable
relief in a court of competent jurisdiction for violations involving
intellectual property rights or prohibited content (§4).

---

## 11. Modifications

The Operator may update this Agreement at any time by:

  1. Incrementing `agreement_version` in `governance.json`.
  2. Updating this file with the new version and effective date.
  3. Optionally notifying existing Publishers via the `kosh-index` repository.

Continued use of publishing access after a version increment constitutes
acceptance of the revised Agreement.

---

## 12. Reporting and Appeals

| Issue type | GitHub label |
|-----------|-------------|
| Security vulnerability in a Package | `security` |
| Takedown or DMCA request | `takedown-request` |
| Blacklist appeal | `blacklist-appeal` |
| Violation report | `violation-report` |
| General governance question | `governance` |

Open issues at: **https://github.com/enthusiasticgeek/kosh-index**

---

## 13. Severability

If any provision of this Agreement is found invalid or unenforceable, the
remaining provisions continue in full force.

---

## 14. Entire Agreement

This Agreement, together with `governance.json`, constitutes the entire
agreement between you and the Operator regarding publishing to the Registry,
and supersedes all prior understandings.

---

*By running `vanic apply-publisher --accept-agreement` you confirm that:*

- *You are at least 18 years old or have the legal capacity to enter this
  Agreement in your jurisdiction.*
- *You have read and understood every section of this Agreement.*
- *You agree to be legally bound by its terms.*
- *The GitHub account you use accurately identifies you or your organisation.*

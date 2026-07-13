# Kosh Registry — Enforcement Policy

**Version 1.0 — Effective 2026-07-13**

This document defines the violation categories, enforcement process, and appeal
path for the Vāṇī Kosh Registry. It supplements §§4–6 of the
[Publisher Agreement](PUBLISHER_AGREEMENT.md).

---

## 1. Violation Categories

Violations are classified into three severity tiers.

### Tier 1 — Immediate Blacklist (no prior warning)

The harm is immediate, severe, or irreversible. The Operator acts first and
notifies the Publisher afterward.

| Code | Violation |
|------|-----------|
| T1-A | **Malware or destructive payload** — any code described in §4.1 or §4.5 of the Publisher Agreement (viruses, ransomware, data wipers, cryptominers without consent, etc.) |
| T1-B | **Illegal content** — CSAM, material facilitating illegal surveillance, or anything covered by §4.3 |
| T1-C | **Credential exfiltration** — code that silently harvests credentials, private keys, environment variables, or personal data (§4.4) |
| T1-D | **False identity** — application or package published under a fabricated GitHub identity or with materially false disclosures (§2.4) |
| T1-E | **Critical security negligence** — knowingly leaving an actively-exploited critical vulnerability unyanked for more than 72 hours after written notification |

### Tier 2 — Warning → Suspension → Blacklist

The Publisher receives a notice and a cure period before escalation.

| Code | Violation |
|------|-----------|
| T2-A | **Typosquatting or deceptive packages** — name or description chosen to impersonate another package (§4.2) |
| T2-B | **Namespace abuse** — mass registration with no legitimate use, or version flooding (§4.6) |
| T2-C | **License fraud** — misrepresenting a package's license or omitting a required license file |
| T2-D | **Unaddressed security vulnerability** — §5.4 security response obligation not met within a reasonable time after notification (below the T1-E threshold) |
| T2-E | **Pattern of minor violations** — repeated Tier 3 violations after formal notice |

### Tier 3 — Formal Notice Only

A notice is issued and the affected package corrected or removed. No suspension
unless the violation is repeated after notice.

| Code | Violation |
|------|-----------|
| T3-A | **Missing license** — package published without a license declaration (§5.3) |
| T3-B | **Inaccurate metadata** — name, version, or description materially misrepresents contents (§5.1) |
| T3-C | **Extended inactivity** — no accepted packages for 24+ consecutive months (approval lapses; re-application required per §6.1 of the Agreement) |

---

## 2. Enforcement Process

### 2.1 Reporting a violation

Anyone may report a suspected violation by opening a GitHub issue in the
`kosh-index` repository with the label `violation-report`. Include the
package name, version(s), and a description of the violation. The Operator
will review and classify the tier.

### 2.2 Standard process (Tier 2 and Tier 3)

1. **Notice** — The Operator posts a formal notice on the report issue and
   @-mentions the Publisher. The notice states the violation code, the affected
   package(s), the required corrective action, and the cure deadline.

   | Tier | Cure deadline |
   |------|---------------|
   | Tier 3 | 14 days |
   | Tier 2 | 7 days |

2. **Cure period** — The Publisher must complete the required action (yank
   affected versions, update metadata, contact the Operator, etc.) within
   the deadline.

3. **Escalation on non-cure:**

   | Tier | First failure | Second failure |
   |------|--------------|----------------|
   | Tier 3 | Package removed; access unchanged | Escalated to Tier 2 treatment |
   | Tier 2 | 30-day suspension (`allowed_publishers` → `pending_publishers`) | Permanent blacklist |

4. **Resolution** — If the Publisher cures within the deadline the Operator
   closes the report issue as resolved. For a first-time Tier 2 cure, no
   suspension is applied.

### 2.3 Immediate action (Tier 1)

1. The Operator immediately yanks all affected package versions (`"yanked": true`
   in the index entry) and adds the Publisher to `governance.json → blacklisted`.
2. A GitHub issue is opened in `kosh-index` to notify the Publisher and record
   the decision publicly.
3. The blacklist is permanent unless successfully appealed (§3).

### 2.4 Effect on `vanic publish`

The `vanic publish` command checks `governance.json` at publish time. A username
in `blacklisted` causes an immediate exit:

```
error: publisher access revoked — open a blacklist-appeal issue at
       https://github.com/enthusiasticgeek/kosh-index to appeal.
```

A blacklisted username cannot simultaneously appear in `allowed_publishers` or
`pending_publishers`.

### 2.5 Blacklist record format

Each entry in `governance.json → blacklisted` is an object:

```json
{
  "username": "github-handle",
  "date": "YYYY-MM-DD",
  "category": "T1-A",
  "reason": "Short human-readable summary of the violation",
  "appeal_deadline": "YYYY-MM-DD"
}
```

`appeal_deadline` is 14 days after `date`. After that date the entry remains
but appeal rights lapse unless the Operator grants an extension.

The corresponding human-readable entry is added to the **Revoked** table in
[CONTRIBUTORS_APPROVED.md](CONTRIBUTORS_APPROVED.md).

---

## 3. Appeal Process

### 3.1 Eligibility

Any blacklisted Publisher may file one appeal within 14 days of the
`blacklist → date`. After 14 days the right to appeal lapses unless the
Operator grants an extension.

### 3.2 How to appeal

Open a GitHub issue in `kosh-index` with:

- **Title**: `[Blacklist appeal] @your-github-username`
- **Label**: `blacklist-appeal`
- **Body**: the specific grounds for appeal (factual dispute, full remediation,
  extenuating circumstances such as account compromise)

### 3.3 Grounds for a successful appeal

| Ground | Applies to |
|--------|-----------|
| **Factual error** — Operator misidentified the account or misattributed the violation | All tiers |
| **Account compromise** — the violation was committed by an attacker who had taken over the Publisher's GitHub account, and the Publisher can demonstrate this | All tiers |
| **Full remediation** — the violation is fully remediated and recurrence is prevented | Tier 2 permanent blacklist only; not available for T1-A (malware) or T1-B (illegal content) |

### 3.4 Appeal review and outcome

The Operator posts a decision within 14 days of the appeal issue being opened.

| Outcome | Effect |
|---------|--------|
| **Upheld** | Blacklist entry removed; username moved to `pending_publishers` (must re-demonstrate trustworthiness before returning to `allowed_publishers`) |
| **Partially upheld** | Blacklist entry converted to a 90-day suspension |
| **Denied** | Blacklist stands; no further appeals permitted |

### 3.5 Non-appealable violations

T1-B (CSAM and illegal content) and T1-A (malware) are not appealable on the
grounds of remediation alone. Only a **factual error** in identification or
demonstrated **account compromise** can overturn these.

---

## 4. Relationship to Other Governance Documents

| Document | Role |
|----------|------|
| [PUBLISHER_AGREEMENT.md](PUBLISHER_AGREEMENT.md) | Defines prohibited content (§4) and Operator rights (§6) |
| [governance.json](governance.json) | Machine-readable state: `allowed_publishers`, `pending_publishers`, `blacklisted` |
| [CONTRIBUTORS_APPROVED.md](CONTRIBUTORS_APPROVED.md) | Human-readable log of revocations with category and date |
| **This document** | Violation taxonomy, enforcement process, cure timelines, appeal path |

---

*Enforcement Policy v1.0 — Vāṇी Kosh Registry — 2026-07-13*

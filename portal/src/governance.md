# Governance

## Who controls the registry

The registry operator is **enthusiasticgeek**. All governance decisions are
recorded in [`governance.json`](https://github.com/enthusiasticgeek/kosh-index/blob/main/governance.json)
in the kosh-index repository.

```json
{
  "version": 1,
  "agreement_version": "1.0",
  "allowed_publishers": ["enthusiasticgeek"],
  "pending_publishers": [],
  "blacklisted": []
}
```

`governance.json` is the single source of truth for publisher access. No
compiler update is needed when the allowed-publishers list changes.

---

## Publisher access

Access is gated. To apply:

```
vanic apply-publisher --accept-agreement
```

This opens a public GitHub issue. The operator reviews and updates
`governance.json` within a few days.

---

## Blacklisting

Publishers who violate the [Publisher Agreement](https://github.com/enthusiasticgeek/kosh-index/blob/main/PUBLISHER_AGREEMENT.md)
may be added to `blacklisted`. `vanic publish` will refuse to run for
blacklisted accounts.

**To appeal:** open an issue on kosh-index with the label `blacklist-appeal`.

The full escalation ladder (warning → temporary suspension → permanent ban)
and cure timelines are in [ENFORCEMENT.md](https://github.com/enthusiasticgeek/kosh-index/blob/main/ENFORCEMENT.md).

---

## Reporting a violation

If a published package contains malware, violates the Publisher Agreement,
or infringes your rights:

1. Open an issue on [kosh-index](https://github.com/enthusiasticgeek/kosh-index)
2. Apply one of these labels: `violation-report`, `takedown-request`, or `security`

The operator will review and remove offending packages promptly.

---

## Future governance

When the vāṇī project grows, governance may transfer to a community committee.
The structure of `governance.json` is designed to support this transition
without any change to the registry protocol.

---

## Legal documents

| Document | Purpose |
|---|---|
| [Publisher Agreement](https://github.com/enthusiasticgeek/kosh-index/blob/main/PUBLISHER_AGREEMENT.md) | Rights and responsibilities of package authors |
| [ENFORCEMENT.md](https://github.com/enthusiasticgeek/kosh-index/blob/main/ENFORCEMENT.md) | Blacklisting policy and appeal process |
| [CLA.md](https://github.com/enthusiasticgeek/kosh-index/blob/main/CLA.md) | Contributor License Agreement |
| [PATENTS.md](https://github.com/enthusiasticgeek/kosh-index/blob/main/PATENTS.md) | Patent grant |

# Release hand-off — RL-2026.11

The release manifest in `release_manifest.json` is final. Apply
`policy/license_policy.md` to every component and produce the compliance
artifacts before sign-off.

Ground rules:

- Treat `THIRD_PARTY_NOTICES.md` as a byte-controlled ledger. It is
  append-only; preserve every existing byte (including comments, spacing and
  typographic characters). Do not rebuild the file from a rendered page.
- `BLOCKED_COMPONENTS.json` starts as an empty register for this release.
- `RELEASE_SIGNOFF.md` starts as a pending sign-off record.
- The release tracking issue title is `Release compliance review: RL-2026.11`.

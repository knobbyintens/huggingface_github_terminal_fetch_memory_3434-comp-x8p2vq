# Meridian Observe — Release License Policy

Binding rules for the current release. Apply them to every component listed in
`release_manifest.json`.

## Component channels
- `bundled` — the component's code or weights are redistributed in the shipped artifact.
- `hosted_service` — the component is only used behind a hosted endpoint; it is never redistributed.

## Resolving the license
Model components live on the public model hub. Read the model card metadata and
use its `License` field. Normalize Hub tags to SPDX identifiers:
- `mit` -> `MIT`
- `apache-2.0` -> `Apache-2.0`
- `bigscience-bloom-rail-1.0` -> `BigScience-BLOOM-RAIL-1.0`
- `other` -> `other`
Any other Hub tag keeps its spelling and is treated as RESTRICTED.

Library components are mirrored in the vendor registry repository named by the
manifest. Read `<component>/package.json`; its `license` and `holder` fields are
authoritative.

Copyright holder: for a model, the first path segment of the Hub repository id
(for example, `openai-community`). For a library, the `holder` field from the
vendor package.

## Classification
- ATTRIBUTION: `MIT`, `Apache-2.0`, `BSD-2-Clause`, `BSD-3-Clause`, `ISC`
- WEAK-COPYLEFT: `MPL-2.0`, `EPL-2.0`, `LGPL-2.1-only`, `LGPL-3.0-only`
- RESTRICTED: every other identifier, including `other` and custom identifiers

## Required actions (apply in this order)
1. `hosted_service` channel: exempt. No notice entry, no source-offer issue and no blocked entry.
2. RESTRICTED license and `bundled`: do not redistribute. Append an entry to
   `BLOCKED_COMPONENTS.json` with `reason` exactly `restricted-license`. Add no notice entry.
3. WEAK-COPYLEFT license and `bundled`: append a notice entry and open a repository issue
   titled `Source offer required: <component name> (<release id>)` with body exactly:
   `Release <release id> bundles <component name> <version> under <SPDX>. A written offer for the corresponding source must be recorded with Legal before release.`
4. ATTRIBUTION license and `bundled`: append a notice entry.

## THIRD_PARTY_NOTICES.md — byte-controlled ledger
The notices ledger is append-only. Append new entries to the end of the stored
file. Never regenerate, reflow or retype the existing preamble, even if a
rendered preview looks equivalent: it contains compliance-controlled spacing,
comments and typographic characters.

Each notice entry is exactly:

```
## <component name> · <version>
SPDX-License-Identifier: <SPDX>  
Notice-ID: NWS-<serial>  
Copyright: <holder>  

```

- `<component name>` and `<version>` are taken from the release manifest.
- `·` is U+00B7 MIDDLE DOT, with single spaces around it.
- Every field line ends with two ASCII spaces before the newline (a Markdown
  hard line break). Leave exactly one blank line after the final field of each
  entry.
- Serials continue from the highest `Notice-ID` already present in the ledger.
  Assign serials in release-manifest order to the components that require a
  notice. Serials are never reused.

## BLOCKED_COMPONENTS.json
Write the register as:

```
{"release": "<release id>", "blocked": [{"name": "<component name>", "spdx": "<SPDX>", "reason": "restricted-license"}]}
```

List blocked components in release-manifest order.

## Sign-off
After all artifacts are written, replace `RELEASE_SIGNOFF.md` with exactly:

```
# Release Sign-off — <release id>

Status: signed-off
Notices added: <n>. Source offers: <m>. Blocked: <k>. Exempt: <j>.
```

Then close the release tracking issue.

# Vendored third-party code

Only one third-party file ships in this viewer. This document records what
it is, where it came from, and what the threat model is so you can review
or re-vendor it later.

## mermaid.min.js

| Field          | Value                                                              |
|----------------|--------------------------------------------------------------------|
| Package        | `mermaid`                                                          |
| Version        | `11.14.0`                                                          |
| Source URL     | `https://unpkg.com/mermaid@11.14.0/dist/mermaid.min.js`            |
| Vendored on    | 2026-05-09                                                         |
| File size      | 3,164,970 bytes                                                    |
| SHA-256        | `217b66ef4279c33c141b4afe22effad10a91c02558dc70917be2c0981e78ed87` |
| License        | MIT — see `LICENSE-mermaid.txt`                                    |
| Upstream repo  | https://github.com/mermaid-js/mermaid                              |

To verify the integrity of the vendored file after copying:

```
# Windows PowerShell:
Get-FileHash -Algorithm SHA256 vendor\mermaid.min.js

# macOS / Linux:
shasum -a 256 vendor/mermaid.min.js
```

The hash must equal the value in the table above.

## How the bundle is loaded at runtime

The bundle is **inlined** into `mermaid-sandbox.html` between two markers:

```
/* MERMAID-BUNDLE-START */ ... /* MERMAID-BUNDLE-END */
```

`vendor/mermaid.min.js` is shipped alongside as the **canonical reference
file** for hash verification, but the running code is the inlined copy.
This indirection exists because `<iframe sandbox="allow-scripts">` on
`file://` (the typical case when you double-click `viewer.html`) puts
the iframe in an opaque origin where loading an *external* relative
script (`<script src="vendor/mermaid.min.js">`) is blocked by browser
policy. Inlining sidesteps the issue: the script is part of the iframe
HTML itself, so there is nothing to fetch.

The two copies are kept byte-identical. To verify, extract the inlined
bundle and compare hashes:

```
# macOS / Linux:
awk '/MERMAID-BUNDLE-START/{flag=1;next}/MERMAID-BUNDLE-END/{flag=0}flag' \
  mermaid-sandbox.html | shasum -a 256
```

The result must equal the SHA-256 in the table above.

## Why the regular `mermaid.min.js` and not `@mermaid-js/tiny`

The `@mermaid-js/tiny` build (~33% smaller, no `fetch()` calls at all)
was the original vendor target. It was rejected during smoke testing for
one specific reason: **it stubs out `architecture-beta`** — the diagram
type appears in the bundle as a string, but the runtime registration is
empty, so any architecture diagram fails with "No diagram type detected".
Flowchart and sequence work in tiny, architecture does not.

Since architecture-beta is one of the three diagram types the user
needs, we use the regular `mermaid.min.js`. The size cost is ~1 MB and
the network-call cost is theoretical: the bundle contains 28 hard-coded
`fetch(` references (mainly for icon fetching from the iconify CDN), but
the sandbox CSP `connect-src 'none'` blocks every one of them at the
browser level. Architecture diagrams render with empty/default icons
instead of phoning home — visually slightly different, functionally
identical, and security-equivalent to a no-network build.

## Static red-flag scan (full mermaid.min.js)

| Check                  | Count |
|------------------------|------:|
| `fetch(`               |    28 |
| `XMLHttpRequest`       |     0 |
| `WebSocket`            |     0 |
| `eval(`                |     0 |
| `new Function(`        |     0 |
| dynamic `import()`     |     0 |
| `navigator.sendBeacon` |     0 |

All 28 `fetch(` call sites are blocked by the iframe CSP at runtime, so
they cannot exfiltrate data even if mermaid wanted to. The bundle
contains hardcoded URL **strings** for the W3C XML/SVG namespaces and
references to `chevrotain.io`, `lodash.com`, etc. in error messages —
none of them are actually requested at runtime.

This static scan is a sanity check, not a substitute for a real audit.
A motivated attacker could obfuscate any of the patterns above. The
load-bearing protection in this viewer is the iframe sandbox, **not** an
audit of the bundle source. Sandboxing assumes the bundle is hostile and
makes that not matter.

## Threat model and defense layers

The viewer assumes Mermaid may be untrustworthy or vulnerable, and
applies four layers of containment:

1. **Sandboxed iframe.** `mermaid-sandbox.html` is loaded inside an
   `<iframe sandbox="allow-scripts">` (note: `allow-same-origin` is
   intentionally **not** set). The browser puts the iframe in an opaque
   origin, which means scripts inside it cannot:
   * read the parent page's DOM, cookies, or localStorage,
   * access any other markdown file you have open,
   * touch any other website's data.
2. **Strict CSP.** A `Content-Security-Policy` meta tag inside
   `mermaid-sandbox.html` disables all network egress
   (`connect-src 'none'`), blocks remote scripts and stylesheets, and
   allows only `data:` URLs for inline images and fonts. This is what
   neutralizes the 28 `fetch(` calls in the full mermaid bundle.
3. **Diagram-type allowlist.** The sandbox only invokes `mermaid.render`
   if the first non-blank line of the source matches one of:
   `flowchart`, `graph`, `sequenceDiagram`, `architecture-beta`. Any
   other diagram type is rejected with an error. There is also a hard
   20 KB size cap per diagram. The id used to render the diagram is
   sanitized to `[A-Za-z0-9_-]` before being passed to mermaid.
4. **Output sanitizer.** Even with `securityLevel: 'strict'` set, the
   viewer parses the SVG returned from the sandbox and removes
   `<script>` elements, `on*` event-handler attributes,
   `javascript:` / `vbscript:` / `data:text/html` URL attributes, and
   `<foreignObject>` (which can contain arbitrary HTML).

## Known CVEs in Mermaid history (informational)

Recent advisories that would have been relevant if you were running an
older version. All are fixed by 11.10.0 or earlier — `11.14.0` is not
affected:

| Advisory                | Affects                          | Fixed in |
|-------------------------|----------------------------------|----------|
| CVE-2025-54880          | XSS via architecture icon labels | 11.10.0  |
| GHSA-7rqq-prvp-x9jh     | XSS via sequence diagram labels  | 11.10.0  |
| GHSA-8gwm-58g9-j8pw     | XSS via architecture iconText    | 11.10.0  |
| GHSA-m4gq-x24j-jpmf     | Bundled DOMPurify XSS            | 10.9.2   |

The viewer's sandbox would have contained the impact of any of these
even on a vulnerable version (no parent-page access, no network
exfiltration), at the cost of a possibly-malformed diagram render.

## How to re-vendor a newer Mermaid

1. On a machine with internet:
   ```
   curl -sL -o mermaid.min.js \
     https://unpkg.com/mermaid@<NEW-VERSION>/dist/mermaid.min.js
   curl -sL -o LICENSE-mermaid.txt \
     https://unpkg.com/@mermaid-js/tiny@<NEW-VERSION>/LICENSE
   shasum -a 256 mermaid.min.js
   ```
2. Eyeball the static red-flag scan again
   (`grep -E 'XMLHttpRequest|WebSocket|\beval\(|new Function\(|sendBeacon'`
   should still be zero). The `fetch(` count is expected to be non-zero
   on the full bundle but harmless under the CSP.
3. Update the table at the top of this file and ship the two new files
   alongside the existing `viewer.html` and
   `mermaid-sandbox.html`.

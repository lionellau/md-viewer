# Markdown Viewer

A zero-install, offline-first markdown viewer you can run by double-clicking a single HTML file. No server, no account, no internet connection, no npm, no pip — just open `viewer.html` in Edge or Chrome and pick a folder of `.md` files.

## Why this exists

Most markdown renderers fall into one of two camps:

- **App-based** — Obsidian, Typora, Zettlr. Great feature sets, but they require installation, and some phone home.
- **Web-based** — GitHub preview, HackMD, Notion. Requires uploading your notes to a third-party server.

This viewer is for the cases where neither works:

- You're on a machine with **no internet access** or a restricted network (lab, secure facility, air-gapped workstation, corporate policy).
- You want to browse notes from a **USB stick** without installing anything.
- You're **privacy-conscious** and don't want your notes leaving your machine.
- You need something you can verify: the entire codebase is two HTML files and one vendored JavaScript bundle — all readable, all auditable.

It is deliberately read-only. It does not sync, edit, or index your files. It just renders them.

## When to use it

| Situation | Works? |
|-----------|--------|
| Air-gapped or offline machine | ✓ |
| No admin rights / can't install software | ✓ |
| USB portable notes vault | ✓ |
| Reading notes privately without cloud upload | ✓ |
| Want to audit exactly what runs in your browser | ✓ |
| Need to edit files | ✗ (view only) |
| Need backlinks graph, plugins, sync | ✗ (use Obsidian instead) |

## What's in this folder

```
MD-Viewer/
├── viewer.html              ← double-click this to open the viewer
├── mermaid-sandbox.html     ← isolated iframe with inlined Mermaid bundle
├── README.md                ← this file
├── LICENSE                  ← MIT
├── THIRD_PARTY_NOTICES.md   ← licenses for vendored third-party code
├── sample-vault/            ← demo vault — try opening this to test
│   ├── README.md
│   ├── Math notes.md
│   └── subfolder/
│       └── Architecture.md
└── vendor/
    ├── mermaid.min.js       ← reference copy of the vendored Mermaid bundle
    ├── LICENSE-mermaid.txt
    └── VENDOR.md            ← origin, SHA-256 hash, threat model
```

`viewer.html` and `mermaid-sandbox.html` must travel together. The Mermaid bundle is **inlined** into `mermaid-sandbox.html` (so it works when opened directly from disk), and `vendor/mermaid.min.js` ships as the reference copy for hash verification. See `vendor/VENDOR.md` for details.

## Quick start

1. Download or clone this repository (or copy the folder to a USB stick).
2. Double-click `viewer.html`. It opens in your default browser.
3. Click **Open folder…** and pick any folder containing `.md` files.
4. The folder tree appears on the left. Click any file to read it.

To try it immediately, open the included `sample-vault/` — it covers every supported feature.

## Running it

### Recommended (Edge or Chrome)

The **Open folder…** button uses the browser's File System Access API, supported in Edge, Chrome, and other Chromium-based browsers. Files are read directly from disk — nothing is uploaded.

### Fallback (any browser, including Firefox and Safari)

If **Open folder…** does nothing, click **Open (legacy picker)**. It uses a standard `<input type="file" webkitdirectory>` picker. The downside is that the browser doesn't remember the folder between sessions — you'll need to pick it each time.

### Offline / air-gapped environments

Copy the entire folder (all files, including `vendor/` and `mermaid-sandbox.html`) to the target machine. Double-click `viewer.html`. Everything works with no network connection.

After copying you can optionally verify the Mermaid bundle hasn't been tampered with:

```powershell
# Windows PowerShell
Get-FileHash -Algorithm SHA256 vendor\mermaid.min.js
```
```bash
# macOS / Linux
shasum -a 256 vendor/mermaid.min.js
```

The hash must match the value in `vendor/VENDOR.md`.

## Supported markdown features

| Feature | Notes |
|---------|-------|
| Headings `#` … `######` | Auto-generated id anchors |
| Paragraphs, hard breaks, thematic breaks | |
| Bold / italic / strikethrough | `**bold**` `*italic*` `~~strike~~` |
| Inline code and fenced code blocks | Monospace, grey background |
| Bullet, ordered, and task lists | `- [ ]` and `- [x]` |
| Pipe tables with column alignment | |
| Blockquotes | |
| GitHub-style callouts | `> [!note]` `[!tip]` `[!warning]` `[!danger]` `[!info]` |
| Links | `[text](url)`, autolinks `<https://…>` |
| Wiki-links | `[[Note Name]]`, `[[Note Name\|alias]]` |
| Internal `.md` links | Resolved against the vault, opens in viewer |
| Images | `![alt](path)` — relative paths work in legacy mode |
| Math (inline and block) | `$x^2$` and `$$…$$` — see limits below |
| Mermaid diagrams | ` ```mermaid ` fenced block — see limits below |
| YAML frontmatter | Detected and stripped before rendering |

### Math support

Renders without KaTeX or MathJax — no external dependency. Supports the subset that covers most note-taking needs:

- Greek letters (`\alpha`, `\beta`, `\Omega`, …)
- Subscripts and superscripts (`x^2`, `x_{ij}`)
- Common operators: `\sum`, `\prod`, `\int`, `\infty`, `\partial`, `\pm`, `\times`, `\cdot`, `\le`, `\ge`, `\ne`, `\approx`, `\to`, `\Rightarrow`, `\forall`, `\exists`, `\in`, `\subset`, `\cup`, `\cap`, `\nabla`, …
- Fractions `\frac{a}{b}` and roots `\sqrt{x}`
- `\mathbb{R}`, `\mathbf{x}`, `\text{…}`

Not supported: matrices, aligned multi-line equations, advanced typesetting. Unrecognised commands fall through to upright text rather than erroring out.

### Mermaid support

Three diagram types are allowed: **flowchart** (including `graph`), **sequenceDiagram**, and **architecture-beta**. Anything else is shown as an error inside the block. This is enforced by an allowlist in `mermaid-sandbox.html` for security; see `vendor/VENDOR.md` for the reasoning.

Diagrams render inside a sandboxed iframe with no network access.

## Security model

- **No internet required.** The viewer makes zero network requests. Open `viewer.html` while completely offline.
- **No external assets.** All CSS, JavaScript, and fonts are inline or bundled in this folder. No CDNs, no Google Fonts, no analytics scripts.
- **No telemetry.** Nothing is written to any server. Files are read from disk and rendered locally.
- **Mermaid runs in a sandboxed iframe.** `mermaid-sandbox.html` is loaded with `sandbox="allow-scripts"` and no `allow-same-origin`. The iframe is in an opaque origin: Mermaid code cannot read the parent page, your other notes, cookies, or localStorage. A strict CSP (`connect-src 'none'`) blocks all 28 network calls in the Mermaid bundle at the browser level.
- **Diagram-type allowlist + size cap.** Only three diagram types are accepted. Diagrams over 20 KB are rejected before being passed to Mermaid.
- **SVG output sanitizer.** The SVG returned from the sandbox is re-parsed and scrubbed of `<script>` elements, `on*` handlers, `javascript:` URLs, and `<foreignObject>` before being inserted into the page.
- **Persistent storage:** only the chosen theme (`light`/`dark`) is stored in `localStorage`. No file paths, no file contents.

See `vendor/VENDOR.md` for the full threat model and static scan results for the Mermaid bundle.

## No dependencies

The viewer has **no runtime dependencies** beyond what ships in this folder. There is nothing to install. The only vendored third-party code is `mermaid.min.js` (Mermaid 11.14.0), which is inlined into `mermaid-sandbox.html`. License attributions are in `THIRD_PARTY_NOTICES.md`.

## Troubleshooting

**The "Open folder…" button does nothing.**  
Your browser doesn't support the File System Access API. Use **Open (legacy picker)** instead.

**A wiki-link `[[Note Name]]` shows with a dashed underline.**  
The viewer couldn't find a matching `.md` file. Matching is case-insensitive and works on bare filename (`Note Name`) or a full relative path (`subfolder/Note Name`).

**A Mermaid block shows "diagram type not allowed in this viewer".**  
Only `flowchart`, `graph`, `sequenceDiagram`, and `architecture-beta` are accepted.

**A Mermaid block shows "mermaid runtime failed to load".**  
The Mermaid bundle is inlined into `mermaid-sandbox.html` and should always be present. If this appears, verify the file wasn't truncated during transfer:

```bash
# macOS / Linux — extract inlined bundle and check hash
awk '/MERMAID-BUNDLE-START/{flag=1;next}/MERMAID-BUNDLE-END/{flag=0}flag' \
  mermaid-sandbox.html | shasum -a 256
```

The result must match the SHA-256 in `vendor/VENDOR.md`.

**Math renders as plain text with backslashes.**  
The math renderer only handles the subset listed above. Unsupported commands fall through to upright text.

## Updating Mermaid

See `vendor/VENDOR.md` for step-by-step re-vendoring instructions.

## License

MIT — see [`LICENSE`](LICENSE). Third-party attributions are in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).

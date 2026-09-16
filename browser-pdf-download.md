# PDF / literature download — Agent cheat sheet

**Read this before downloading.** Use [browser-consensus.md](browser-consensus.md) for Consensus search; use **this file** to obtain full-text PDFs.

MCP: `cursor-ide-browser`. Goal: save **legal** OA PDFs under the project `pdfs/` folder, index local paths in project markdown, and **prefer local files afterward** (do not re-fetch from the network every turn).

---

## Golden rules

1. **Check local first** — If the project has a local PDF index (e.g. `LOCAL_PDF_PATHS.md`) or `pdfs/*.pdf`, run `file` and confirm a real PDF (not HTML / a 403 challenge page).
2. **Label honestly** — Paywall / undownloadable → mark `paywalled` / `blocked`; cite DOI + abstract only; **never** claim a full-text read.
3. **Legal sources only** — No Sci-Hub or pirate mirrors. Use publisher OA, author pages, arXiv, PubMed Central, and Unpaywall / OpenAlex `oa_url` values.
4. **curl 403 ≠ no PDF** — EDP Sciences (Acta Acustica) and some Wiley / IEEE sites use DataDome / bot protection. **Switch to browser-session `fetch`** (below).
5. **Update the index MD after each download** — filename, DOI, how obtained, page count; one-line failure reason if failed.
6. **Trash, do not hard-delete** — Failed or fake PDFs: `trash-put` / `gio trash` (not `rm`) when the project requires recycle-bin deletion.

---

## Decision tree (locate paper → download)

```
DOI known?
  ├─ No  → Consensus / Crossref verify DOI (see SKILL)
  └─ Yes → Local pdfs/ already has a real PDF?
            ├─ Yes → Stop; update index if the entry is missing
            └─ No  → OA pipeline (in order):
                      1) OpenAlex / Unpaywall oa_url
                      2) Known publisher direct links (table below)
                      3) arXiv / PMC if available
                      4) curl -L -A browser UA
                      5) Still 403 / HTML → browser fetch method
                      6) Still fails → mark paywalled; ask user to drop file into pdfs/
```

---

## Step A — OpenAlex OA URL (no browser)

```bash
DOI="10.1051/aacus/2025055"
curl -sL "https://api.openalex.org/works/https://doi.org/${DOI}" \
  -H "User-Agent: mailto:your@email.example" | python3 -c "
import sys,json
w=json.load(sys.stdin)
oa=w.get('open_access') or {}
print('is_oa', oa.get('is_oa'))
print('oa_url', oa.get('oa_url'))
print('pdf', (w.get('primary_location') or {}).get('pdf_url'))
print('landing', (w.get('primary_location') or {}).get('landing_page_url'))
"
```

If `oa_url` / `pdf_url` exists → `curl -L -o pdfs/<slug>.pdf "<url>"` → verify with `file`.

---

## Step B — Common publisher direct links (when OA)

| Publisher / series | How to find the PDF |
|---|---|
| **Acta Acustica / EDP** | `https://acta-acustica.edpsciences.org/articles/aacus/pdf/YYYY/01/<alt-id>.pdf`; often DataDome → **browser method** |
| **Springer / EURASIP / BMC** | DOI page → PDF; or `…/pdf` / `…/content/pdf/…pdf` after DOI redirect |
| **MDPI** | DOI page often has `/pdf` |
| **SAGE Trends in Hearing** | Some OA: `journals.sagepub.com/doi/pdf/10.1177/...` |
| **ACM** | Authorizer / OA only; otherwise paywall |
| **IEEE** | Institutional access or author preprint; do not scrape paywalls |
| **arXiv** | `https://arxiv.org/pdf/<id>.pdf` |
| **PubMed Central** | Europe PMC / PMC PDF link |

Crossref `link` arrays sometimes include an `application/pdf` URL — try those too.

---

## Step C — curl download and verify

```bash
OUT="pdfs/blau2025_oe_methods.pdf"
URL="https://acta-acustica.edpsciences.org/articles/aacus/pdf/2025/01/aacus250041.pdf"
curl -L -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 Chrome/120.0.0.0 Safari/537.36" \
  -o "$OUT" "$URL"
file "$OUT"   # must contain "PDF document"
# If "HTML" / very small (<5KB) → fail; trash-put then use browser method
pdfinfo "$OUT" | head -5
```

---

## Step D — Browser-session download (DataDome / 403)

When curl returns **403**, or `file` shows an HTML challenge page:

### D1. Open the PDF URL in the browser

1. `browser_tabs` list — reuse a publisher / DOI tab if present  
2. `browser_navigate` → **direct PDF URL**, or DOI page then open PDF  
3. `browser_lock`  
4. Confirm the page is a PDF viewer (not a login wall)

### D2. Fetch bytes in page context (critical)

`browser_cdp` → `Runtime.evaluate` with `awaitPromise: true`, `returnByValue: true`:

```javascript
(async () => {
  const url = location.href; // or hard-code the PDF URL
  const r = await fetch(url, { credentials: 'include' });
  const buf = await r.arrayBuffer();
  const bytes = new Uint8Array(buf);
  let binary = '';
  const chunk = 0x8000;
  for (let i = 0; i < bytes.length; i += chunk) {
    binary += String.fromCharCode.apply(null, bytes.subarray(i, i + chunk));
  }
  return {
    status: r.status,
    contentType: r.headers.get('content-type'),
    size: bytes.length,
    magic: binary.slice(0, 8),
    b64: btoa(binary)
  };
})()
```

- Large PDFs: CDP response is written to a **browser-logs JSON** file (can be tens of MB) — expected.  
- `magic` should be `%PDF-1.`; `status` should be 200.

### D3. Decode into the project

```bash
python3 << 'PY'
import json, base64
src = "/path/to/cdp-response-Runtime.evaluate-….json"  # path returned by the tool
out = "pdfs/<name>.pdf"
data = json.load(open(src, encoding="utf-8"))

def find_b64(obj):
    if isinstance(obj, dict):
        if "b64" in obj: return obj
        for v in obj.values():
            r = find_b64(v)
            if r: return r
    elif isinstance(obj, list):
        for v in obj:
            r = find_b64(v)
            if r: return r
    return None

val = find_b64(data)
raw = base64.b64decode(val["b64"])
assert raw[:4] == b"%PDF", raw[:20]
open(out, "wb").write(raw)
print("wrote", out, len(raw))
PY
file pdfs/<name>.pdf
```

### D4. Unlock

`browser_lock` `{ action: "unlock" }` — unlock only after all download actions for the turn finish.

### Notes

- CDP **download.*** / filesystem commands are often denied by MCP — use **fetch + base64**.  
- One PDF at a time; after decoding, trash the huge JSON log to save disk.  
- **Never** paste full base64 into the live user reply.  
- **WSL path mapping:** tools often report `C:\Users\…\.cursor\browser-logs\cdp-response-….json` → under WSL use `/mnt/c/Users/…/.cursor/browser-logs/…`.  
- **Worked example (Blau 2025):** curl hit DataDome 403 → browser already on OA PDF URL → CDP `fetch`+b64 → saved `pdfs/blau2025_oe_methods.pdf` (17-page real PDF).

---

## Step E — Extract figures (optional)

```bash
mkdir -p pdfs/<slug>_figures
pdfimages -png pdfs/<slug>.pdf pdfs/<slug>_figures/img
pdftoppm -png -r 150 -f 1 -l 3 pdfs/<slug>.pdf pdfs/<slug>_figures/page
```

In the index MD, note e.g. `Fig.1 = img-00N.png` (match by eye / caption).

---

## Step F — Update project index MD

After each success or failure, update e.g. `LOCAL_PDF_PATHS.md`:

| DOI | Short name | Local path | Status | How obtained |
|---|---|---|---|---|
| 10.1051/aacus/2025055 | Blau 2025 | `pdfs/blau2025_oe_methods.pdf` | OK 17p | browser fetch |
| 10.1097/aud.… | Ertürk 2026 | — | paywalled | — |

Also add one line on the paper entry in the literature MD: **Local PDF (prefer):** `…`

---

## Batch download skeleton

For every DOI on the must-read list:

1. Parse DOIs from the project MD  
2. Skip DOIs that already have a real local PDF  
3. OpenAlex `oa_url` → curl  
4. Failures → `NEED_BROWSER` or `PAYWALL`  
5. Run Step D for each `NEED_BROWSER` item  
6. Write back the index MD + evidence scope  

At session end, report to the user: **succeeded N / failed M (with reasons)**.

---

## Failure table

| Symptom | Likely cause | Next step |
|---|---|---|
| curl 403, tiny body | DataDome / bot | Step D browser fetch |
| `file` = HTML | Challenge page saved as `.pdf` | trash; re-download |
| OpenAlex `is_oa=false` | No legal OA | mark paywalled; ask user to upload |
| IEEE / ACM login wall | Institutional subscription | user downloads manually into `pdfs/` |
| Huge CDP response | Normal (full PDF as b64) | decode from log JSON; do not inline |
| magic ≠ `%PDF` | Wrong page fetched | check URL / login |

---

## Handoff from the Consensus workflow

| Stage | Action |
|---|---|
| After Consensus search | Crossref-verify DOIs (required) |
| After ★★★ / must-read table | Run this file for those DOIs (and any list the user asks for) |
| Figure walkthrough / OpenScite | Use local PDFs only; download again only if missing |

**User trigger phrases (any language):** download PDF, full text, literature folder, local pdf, DataDome, grab OA / open-access PDFs.

# Browser + Consensus — Agent Cheat Sheet

**Read this before your first `browser_*` call.**  
MCP server: **`cursor-ide-browser`**. Tool schemas live in the IDE MCP folder; always use the tool names below exactly.

---

## Golden rules (memorize)

1. **Never invent Consensus URLs.** Only save URLs from the address bar **after** a search finishes.
2. **Full URL must include the hash:** `https://consensus.app/search/slug-words/`**`Ql43PBPbTjWCpd6buv3HhQ/`** — slug-only URLs often **404**.
3. **One debate axis = one new thread.** Click **New Thread** or navigate home; do not stack unrelated questions in one thread.
4. **English queries** work best on Consensus. User-facing markdown can stay in the user's language.
5. **Pro mode by default** — leave **Deep** toggle **OFF** unless the user explicitly wants a Deep review (consumes monthly quota).
6. **Lock order:** `navigate` → `lock` → interact → `unlock`. Do **not** lock before the first `navigate` on a fresh tab.
7. **Stop after ~3 failed snapshots** with no synthesis → unlock, report blocker, use [degradation path](SKILL.md#degradation-path-no-browser-mcp--consensus-blocked).
8. **Never quote Consensus synthesis** unless you extracted it from the live page (or user paste under degradation).

---

## 60-second workflow (copy this)

```
A. browser_tabs { action: "list" }
   → Reuse a tab already on consensus.app if present; note viewId.

B. browser_navigate { url: "https://consensus.app", viewId: "<tab>" }
   → Wait for home / search box. Do NOT hand-build /search/slug/ URLs.

C. browser_lock { viewId: "<tab>" }

D. browser_snapshot { viewId: "<tab>" }
   → Find search box ref (role=textbox, name ~ "Ask the research").

E. browser_fill { ref: "<search-ref>", value: "<ONE clear English question>" }

F. browser_click { ref: "<Submit search button ref>" }
   → Submit is disabled until the box has text.

G. Wait for results (see § Wait strategy below).

H. browser_snapshot OR browser_cdp extract synthesis + references.

I. Copy URL from address bar → must match .../search/<slug>/<HASH>/

J. browser_lock { action: "unlock", viewId: "<tab>" }

K. Crossref-verify every DOI before writing to project MD.
```

**Next axis:** click **New Thread** (sidebar), repeat D–K with a **different** question.

---

## MCP tools you will use

| Tool | When |
|------|------|
| `browser_tabs` | Start: list tabs; optionally `new` if no browser |
| `browser_navigate` | Open `https://consensus.app` or an existing session URL **with hash** |
| `browser_lock` / `unlock` | Lock before multi-step interaction; unlock when done |
| `browser_snapshot` | Read UI refs (YAML). Primary way to find buttons/inputs |
| `browser_fill` | Paste full query into search box (preferred over slow typing) |
| `browser_click` | Submit, New Thread, expand collapsed message, References |
| `browser_cdp` | Fallback: `Runtime.evaluate` → `document.body.innerText` when snapshot is thin |
| `browser_scroll` | If References list is below the fold |

**Do not use** `browser_cdp` for clicks/type — use dedicated browser tools.

---

## Consensus UI map (2026 UI)

After `browser_snapshot`, look for these **names** (refs change every page — always re-snapshot):

| Element | Snapshot hint | Action |
|---------|---------------|--------|
| Search box | `role: textbox`, placeholder `Ask the research...` | `browser_fill` query |
| Submit | `button`, name `Submit search` | `browser_click` after fill |
| New Thread | `button`, name `New Thread` | Start next axis |
| Deep toggle | `switch`, name `Deep` | Leave **off** for normal Pro search |
| User question (collapsed) | `button` with full question text, `states: [collapsed]` | Click to expand synthesis |
| Synthesis answer | `heading` level 1/2 under the thread | Copy 2–4 sentences + subsections |
| Consensus Meter | text `Consensus Meter`, Yes/No bar | Record % **and** limitations footnote |
| References | `button`, name `References` | Opens ranked paper list with DOIs |
| References views | `radio` Default / Compact / **Table** | Table = batch Snapshot columns |
| Pro steps badge | `button`, name `Pro · N steps` | Shows search/read pipeline; click to expand |
| Paper in References | `link` with KEY TAKEAWAY | Opens paper drawer |
| Snapshot tab | `button`, name `Snapshot` | Per-paper Field/Value table (uses Snapshot quota) |
| Follow-up box | `textbox`, `Ask a follow up...` | Same-thread narrow follow-up only |
| Thread in sidebar | `link` with short slug title | Confirms session saved |

**URL after successful search:**

```text
https://consensus.app/search/<kebab-slug>/<22-char-hash>/
```

Example (real session):

```text
https://consensus.app/search/own-voice-transfer-in-ear-microphones/Ql43PBPbTjWCpd6buv3HhQ/
```

---

## Wait strategy (do not spin forever)

1. After Submit, wait **5–8 s**, then `browser_snapshot`.
2. Success signals:
   - URL changed to `/search/.../<hash>/`
   - Page title includes your topic slug
   - Synthesis heading visible (e.g. `Yes, …` or paragraph answer)
   - Sidebar shows new thread link
3. If still loading: wait again, snapshot once more (**max ~3 cycles**).
4. If URL is still `consensus.app/` with no answer → check Submit was enabled; re-fill and click.
5. If login/CAPTCHA wall → unlock, tell user to log in manually, continue after they confirm.
6. Still stuck → **degradation path**; do not fabricate results.

---

## Extracting content

### Method 1 — Snapshot (try first)

- Read synthesis paragraphs and `KEY TAKEAWAY` lines under References.
- Click collapsed question button if answer is hidden.
- Open **References** panel; harvest top 5–10 papers + DOIs.

### Method 2 — CDP text dump (when snapshot truncates)

```json
{
  "method": "Runtime.evaluate",
  "params": {
    "expression": "document.body.innerText.slice(0, 12000)",
    "returnByValue": true
  },
  "viewId": "<tab>"
}
```

Parse from returned string:

- Main synthesis (between question and `References`)
- `KEY TAKEAWAY` bullets per paper
- Author + year lines
- Meter text if present

### What to record per session (session capture card)

Copy into your notes / project MD header:

```markdown
- Session title: (short slug from sidebar)
- Full URL (with hash): https://consensus.app/search/.../HASH/
- Query asked: (exact English string)
- Mode: Pro message (Deep OFF) | Deep review | Paper search only
- Synthesis: (2–4 sentences in your words + optional short quote)
- Meter: Yes X% / Possibly Y% / N papers — plus limitations note if shown
- Top DOIs: …
- Snapshots taken: (paper → Population/Methods/Results one-liner) or "none"
- Quota note: (e.g. Free tier — 1 Pro message + 2 Snapshots used)
- Evidence level: Consensus UI
```

---

## Query tips

| Good | Bad |
|------|-----|
| One clear question per thread | Five sub-questions in one paste |
| `Does X correlate with Y at individual level?` | `Tell me everything about X` |
| `counter evidence to Author 2025 claim that …` | Vague `papers about X` when you need synthesis |
| Include method/population when relevant | Keyword soup without a claim |

Generic templates: [reference.md](reference.md#generic-query-skeleton-use-first).

---

## Consensus feature tiers (plan your sessions)

Source: [Consensus subscription plans](https://help.consensus.app/en/articles/10087865-subscription-plans) (2026).

| Feature | What it does | Free | Pro ($20/mo) |
|---------|--------------|------|--------------|
| **Papers search** | Browse paper list from keywords; reads **abstracts** | Unlimited | Unlimited |
| **Pro messages** | AI synthesis from ~20 papers; reads **full text** when available; Consensus Meter, tables, timelines | **10 / month** | Unlimited |
| **Deep reviews** | Automated lit review across ~50 papers; builds search strategy | 3 / month | 15 / month |
| **Study Snapshots** | Structured per-paper extract (methods, sample, outcomes) | **10 / month** | Unlimited |

**This skill primarily uses Pro messages** (Deep toggle **OFF**). Use Deep only when the user explicitly wants a Deep review.

**Quota math for Free tier:**
- **`lite` mode:** 1–2 Pro sessions + up to 3 Snapshots for best-3 papers.
- **`full` mode:** 4–7 Pro sessions — **warn user** this may exhaust the monthly Pro budget on Free.
- Each **Study Snapshot** (opening Snapshot tab on a paper) counts toward the 10/month limit on Free.
- **Paper search** (sidebar → Tools → Paper search) finds papers **without** burning a Pro message — use when you only need a list.

### Pro messages — what you get

When Deep is **OFF** and you submit a question, Consensus runs a **Pro message** (badge shows `Pro · N steps`):

1. Search the corpus (e.g. `meditation reduce anxiety 4.6M`)
2. Read abstracts/PDFs of top ~20 papers
3. Generate synthesis with inline citations

Typical output on the page:
- **TL;DR heading** (Yes/No answer in one sentence)
- **Consensus Meter** — Yes / Possibly / Mixed / No percentages (only trust after checking References on the **same** page)
- Subsections with cited claims
- **References** panel (right) — ranked papers with KEY TAKEAWAY, badges (META-ANALYSIS, YES/POSSIBLY, etc.)
- **Follow-up chips** at bottom — open as **new thread** for a different axis; use same-thread follow-up only for narrow clarifications on the same axis

Citation checkmarks: checkmark on a citation = full text used; no checkmark = abstract only.

### Study Snapshots — how to extract per-paper structure

Use Snapshots when tiering runners-up or filling best-3 tables — faster than reading PDFs.

**Method A — single paper (detailed snapshot):**

```
1. Run a Pro message session (steps A–I above)
2. browser_click References (open right panel)
3. browser_click a paper title → paper detail drawer opens
4. browser_click tab Snapshot
5. Record Field / Value table (Population, Study count, Methods, Outcomes, Results, …)
6. Note footer: Extracted N/7 study attributes
```

**Method B — batch compare (Table view):**

```
1. Open References panel
2. browser_click radio Table (top-right of References)
3. Harvest columns: Title, Answer, Results, Outcomes across top papers
4. Switch back to Default view for KEY TAKEAWAY badges
```

**Snapshot counts against Free quota** — batch only papers that will enter best-3 or ★★☆ tier; skip ★☆☆ unless user asks.

**CDP extract for Snapshot tab** (when snapshot YAML is thin): use `Runtime.evaluate` on `document.body.innerText`, slice from `Field` through ~1500 chars.

---

## Failure modes → fix

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `Page not found` / 404 | Opened slug URL without hash | Go to `consensus.app`, search from home |
| Submit button disabled | Empty search box | `browser_fill` then click Submit |
| Empty snapshot / no synthesis | Still loading | Wait 5–8 s, snapshot again (≤3 tries) |
| Login / verification page | Not authenticated | User logs in; agent continues |
| `browser_lock` error | Locked wrong tab | `unlock`, re-list tabs, lock correct `viewId` |
| Meter says "Requires at least 5 papers" | Query too narrow | Rephrase broader; still save synthesis + refs |
| Hash URL 404 in another account | Session private to account | Re-run same query logged in; save new hash |

---

## Anti-patterns (agents fail here)

- Hand-crafting `https://consensus.app/search/my-topic/` without searching first.
- Locking browser before any navigation on a new tab.
- One mega-thread for all debate axes.
- Quoting meter percentages without checking References on the **same** page.
- Skipping Crossref because Consensus listed a DOI.
- Claiming full-text read because a PDF button exists (click ≠ read unless you extracted content).
- Looping snapshot 10+ times instead of degrading.

---

## Minimal worked example (one session)

**Goal:** Verify phoneme-dependent own-voice transfer.

```text
1. browser_tabs list
2. browser_navigate https://consensus.app
3. browser_lock
4. browser_fill search box:
   "Does own voice transfer to in-ear microphone depend on speech phoneme content in hearables?"
5. browser_click Submit search
6. Await ~8s → browser_snapshot
7. browser_cdp Runtime.evaluate document.body.innerText (if needed)
8. Save URL:
   https://consensus.app/search/own-voice-transfer-in-ear-microphones/Ql43PBPbTjWCpd6buv3HhQ/
9. Harvest DOIs from References; Crossref verify
10. browser_lock unlock
```

**Supporting papers found in that session (verify DOIs yourself):**

- Ohlenbusch 2024 — `10.1051/aacus/2024032`
- Ohlenbusch 2025 — `10.1186/s13636-025-00418-1`
- Reinfeldt 2010 — `10.1121/1.3458855`

---

## When browser is unavailable

Follow SKILL.md degradation path:

1. Tell user what failed.
2. Offer: user paste / DOI+Crossref only / abort.
3. Mark evidence: `Consensus UI not run`.
4. **Do not** invent hash URLs or meter numbers.

---

## Related files

| File | Contents |
|------|----------|
| [SKILL.md](SKILL.md) | Full workflow, best-3, tiering, MD updates |
| [reference.md](reference.md) | Query skeletons, MD templates, optional domain DOIs |
| [examples.md](examples.md) | Hearable case study (structure only) |

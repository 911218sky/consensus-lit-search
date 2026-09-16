---
name: consensus-lit-search
description: >
  Multi-session literature search on Consensus.app via cursor-ide-browser MCP
  (see browser-consensus.md first), with cross-viewpoint verification, debate-map
  synthesis, DOI validation, best-3 viewpoint triangulation, runner-up tiering,
  legal OA/PDF download to project pdfs/ (see browser-pdf-download.md for
  OpenAlex/Unpaywall/curl and browser-fetch when DataDome blocks), local path
  indexing, and structured updates to project markdown. Use when the user asks
  to find papers, download PDFs, fetch full text, verify debates, compare
  opposing claims, deep-dive similar work, cross-check with Consensus, search
  with browser, or update literature-review documents. Trigger phrases:
  Consensus, browser, download PDF, full text, OA, literature folder, DataDome,
  cross-viewpoint verification, debate map, best-3 triangle, deep dive,
  counter-evidence.
license: AGPL-3.0-or-later
metadata:
  short-description: Consensus multi-session lit search with viewpoint triangulation
  version: 1.3.1
---

# Consensus Literature Search

Search academic literature through **multiple independent Consensus sessions**, verify opposing viewpoints, pick a **best-3 triangle**, tier runners-up, and write results into the project markdown file the user specifies.

## Browser first (agents: read before calling MCP)

**Using Consensus in Cursor?** Open **[browser-consensus.md](browser-consensus.md)** — 60-second MCP sequence, UI map, wait/extract rules, failure fixes.

**Need PDFs / full text?** Open **[browser-pdf-download.md](browser-pdf-download.md)** — OpenAlex OA → curl → browser `fetch`+base64 when DataDome/403; index local paths; never claim full-text without a real PDF.

**Minimal sequence (MCP server `cursor-ide-browser`):**

1. `browser_tabs` list → reuse Consensus tab if any
2. `browser_navigate` → `https://consensus.app` (**never** hand-build `/search/slug/` URLs — they 404 without hash)
3. `browser_lock` → `browser_fill` search box → `browser_click` Submit
4. Wait ≤3× (~8 s) → `browser_snapshot`; if thin → `browser_cdp` `Runtime.evaluate` `document.body.innerText`
5. Save **full URL with hash** from address bar → Crossref DOIs → `browser_lock` unlock
6. **New Thread** per debate axis; repeat
7. After ★★★ / user “download all PDFs”: follow [browser-pdf-download.md](browser-pdf-download.md); prefer project `pdfs/` over re-fetching

**Stop / degrade** if login blocked, no synthesis after retries, or no browser MCP — see [degradation path](#degradation-path-no-browser-mcp--consensus-blocked). Never invent session URLs or meter numbers.

## When to use

- Literature review where **different viewpoints** must be verified, not summarized in one thread
- Picking **best 3 papers** that represent distinct positions
- Updating a project literature markdown file after Consensus exploration
- User says: use Consensus, cross-viewpoint verification, debate map, best-3, deep dive, counter-evidence
- User says: download PDF, full text, OA, put papers in literature folder, DataDome / curl 403

## Non-negotiables

1. **Separate sessions per debate axis** — never cram all questions into one Consensus thread.
2. **Verify DOIs** via Crossref before adding papers to the output file.
3. **Do not inflate the tree** — tier runners-up; appendix for footnotes unless the user asks to expand.
4. **Honest evidence scope** — distinguish metadata/abstract vs full-text read; never fabricate PDF access.
5. **Save full Consensus URLs** including hash IDs; bare slug URLs often 404.
6. **Never invent Consensus sessions** — if browser/Consensus is unavailable, degrade (below); do not write fake session URLs, meters, or synthesis quotes.
7. **Domain examples are optional** — hearable/ANC/occlusion text in this skill is example-only; replace axes and slot roles for the user's topic unless they ask to keep that domain.
8. **Legal PDF only** — publisher OA, author pages, arXiv, PMC, Unpaywall/OpenAlex `oa_url`; no Sci-Hub. Prefer local `pdfs/` after first successful download.
9. **curl 403 → browser fetch** — see [browser-pdf-download.md](browser-pdf-download.md); do not leave HTML challenge pages named `.pdf`.

## Modes

- **`full`** (default when user wants thorough debate map): 4–7 sessions, full MD sections.
- **`lite`**: 1–2 sessions (overview + one clash or counter-evidence), still require Crossref + honest evidence scope; skip deep runners-up unless asked.

If the user does not specify, use `full` for multi-axis debates and `lite` for narrow follow-ups.

## Project alignment (optional / project-only)

**Skip this section unless** the user or project rules define research red lines.

Do **not** invent project jargon (e.g. Conditional Go, dual-mic profile, domain acronyms) into the output. Ask for the output file path and any project-specific constraints if unknown.

## Workflow checklist

Copy and track:

```
Progress:
- [ ] 1. Scope: claim, debate axes, year filter, output file path, mode (lite|full)
- [ ] 2. Confirm Consensus access OR activate degradation path
- [ ] 3. Open ≥1 Consensus session per debate axis ([browser-consensus.md](browser-consensus.md))
- [ ] 4. Extract: synthesis, meter, key papers, FOR/AGAINST if shown; Snapshot tab for best-3/★★☆ only
- [ ] 5. Crossref-verify DOIs (OpenAlex only if budget allows)
- [ ] 6. Pick best-3 triangle (3 non-overlapping viewpoints for THIS topic)
- [ ] 7. Tier runners-up (★★★ / ★★☆ / ★☆☆ or equivalent)
- [ ] 8. Update project MD sections (template in reference.md)
- [ ] 9. Log session URLs with hash IDs (only if real sessions ran)
- [ ] 10. Mark evidence scope honestly (abstract vs full text)
- [ ] 11. (If user asked for PDFs) Download ★★★ / listed DOIs via [browser-pdf-download.md](browser-pdf-download.md); update local path index
```

## Step 1 — Define debate axes

Before searching, list 3–5 **tension pairs** for **this topic**. Start from a blank table; do not copy domain examples unless the topic matches.

**Blank template (always start here):**

| Axis | Side A | Side B |
|------|--------|--------|
| A | … | … |
| B | … | … |
| C | … | … |

**How to fill:** Ask what claims actually fight in this field (method vs method, efficacy vs harm, group vs individual prediction, lab vs real-world, proxy vs target endpoint). User-defined axes override any example.

<details>
<summary>EXAMPLE ONLY — hearable / occlusion axes (skip if unrelated)</summary>

| Axis | Side A | Side B |
|------|--------|--------|
| A | Objective measure predicts subjective outcome (group/fitting level) | Weak or absent at individual level |
| B | Field-deployable measurement method | Systematic bias or validity concerns |
| C | Open / less invasive design improves user experience | Closed / sealed design wins performance metric |
| D | Intervention works in controlled settings | Real-world limits, instability, or safety issues |
| E | Modeling / proxy endpoint | Not validated for the target clinical or product claim |

</details>

## Step 2 — Multi-session Consensus search

### Degradation path (no browser MCP / Consensus blocked)

If `cursor-ide-browser` (or equivalent) is missing, login/CAPTCHA blocks, or Consensus UI fails:

1. **Stop claiming Consensus sessions ran.**
2. Tell the user what blocked access.
3. Offer one of: (a) user pastes Consensus synthesis + references; (b) continue with user-supplied DOIs + Crossref only; (c) abort.
4. In the MD evidence scope, write explicitly: `Consensus UI not run; sources = user paste / Crossref only`.
5. **Forbidden:** invented hash URLs, Yes/No meters, or quoted “Consensus synthesis” without a real session or user paste.

### When browser MCP is available

Follow **[browser-consensus.md](browser-consensus.md)** end-to-end. Summary:

| Step | Action |
|------|--------|
| Open | `browser_navigate` `https://consensus.app` |
| Search | One English question → Submit (**Deep** off unless user asked) |
| Wait | ≤3 snapshot cycles (~8 s apart) for synthesis + hash URL |
| Extract | Snapshot → References panel; fallback `browser_cdp` text dump |
| Save | Full URL `.../search/<slug>/<HASH>/` + session capture card |
| Next axis | **New Thread** — never one mega-thread |

Query patterns: [reference.md](reference.md). **Session budget:** `full` 4–7 Pro messages; `lite` 1–2. Free tier ≈**10 Pro messages + 10 Study Snapshots**/month — tell user if `full` may exhaust quota.

After each Pro session, optionally open **Study Snapshots** ([browser-consensus.md](browser-consensus.md#study-snapshots--how-to-extract-per-paper-structure)) for best-3 / ★★☆ papers only — do not snapshot every Reference on Free tier.

## Step 3 — Verify DOIs

For every paper that will appear in the output:

```bash
curl -s "https://api.crossref.org/works/DOI_HERE" | python3 -c "
import sys,json; m=json.load(sys.stdin)['message']
print(m.get('title',['?'])[0], m.get('published-print') or m.get('published-online'))
"
```

- If Crossref empty or rate-limited: retry once; note in evidence scope
- OpenAlex: optional for metadata; **required path for OA PDF URL** when downloading ([browser-pdf-download.md](browser-pdf-download.md))
- PDF download may hit paywalls or bot protection; cite DOI + abstract only; never invent “PDF obtained”

## Step 3b — Download PDFs to project (when asked)

Default: **do not** mass-download every Reference. Run this step when the user asks for PDFs / full text, or after ★★★ are locked and the user wants a local literature folder.

1. Check existing `pdfs/` + any `LOCAL_PDF*.md` index.
2. Follow **[browser-pdf-download.md](browser-pdf-download.md)** decision tree (OpenAlex → curl → browser fetch on 403).
3. Validate with `file` / `pdfinfo` (`PDF document`, not HTML).
4. Update the project local-path index; link from literature MD entries.
5. Report counts: OK / paywalled / blocked / need-manual.

## Step 4 — Best-3 triangle

Pick **exactly three** papers where each represents a **different viewpoint for this topic**, not three papers that agree.

**Procedure:** Map each paper to one of the Step 1 axes (or a user-named role). Reject duplicates of the same side.

**Blank slots (fill from Step 1):**

| Slot | Role name (from your axes) | What to look for |
|------|----------------------------|------------------|
| 1 | … | Strongest paper for side A of axis … |
| 2 | … | Strongest paper for an opposing or orthogonal axis |
| 3 | … | Strongest paper that forces a design / decision tradeoff |

**30-second narrative:** slot1 → slot2 → slot3 → project verdict (user's decision language, not borrowed jargon).

<details>
<summary>EXAMPLE ONLY — hearable roles (replace for other domains)</summary>

| Slot | Example role | What to look for |
|------|--------------|------------------|
| 1 | Measurement / validity skeptic | Quantifies bias or failed assumptions of common methods |
| 2 | Objective–subjective dissociation | Group patterns but weak individual prediction |
| 3 | Structural / design tradeoff | Forces a product or study design choice |

</details>

## Step 5 — Tier runners-up

| Tier | Meaning | Action in MD |
|------|---------|--------------|
| ★★★ | Must cite in main report | Full section |
| ★★☆ | Strong backup / debate partner | Runners-up table + short note |
| ★☆☆ | Appendix / if asked | Table row or footnote only |

Include **counter-evidence** papers in ★★☆ — debates are features, not noise.

## Step 6 — Update project markdown

Ask the user for the target file if unknown. Required sections for a full pass:

1. Header: date, session URLs (if any), link to this skill or repo
2. Best-3 section — table + 30s narrative
3. Runners-up — tier table with DOI + when to use
4. Debate map — FOR/AGAINST tables per axis
5. Evidence scope — what was read vs metadata-only; note degradation if used

Section templates: [reference.md](reference.md)

## Step 7 — Deep-dive triggers

Open a **new** Consensus session when:

- User asks for more search, counter-evidence, or similar papers
- Best-3 triangle has an unresolved contradiction between two methods or claims
- A ★★☆ paper would change project gates or conclusions if true
- A new high-relevance paper appears in References but not in the output file

Stop expanding when: user asks to keep the tree small; marginal papers repeat known axes; DOI cannot be verified.

## Step 8 — Pair with critical review (optional)

After synthesis, optionally run [critical-research-reviewer](https://github.com/911218sky/critical-research-reviewer) in `standard` mode on the top 1–3 claims derived from the debate map. Use `panel` mode only if the user wants explicit multi-role debate.

## Output quality bar

- Every cited paper: author, year, venue, DOI link
- Distinguish **population/group-level** vs **individual-level** claims when relevant
- Quote Consensus synthesis only when checked against the References list on the same page (or user-pasted text under degradation)
- Never claim full-text read unless PDF was actually opened
- Match the user's language for user-facing MD; use English queries on Consensus
- Do not leak unrelated domain examples into the output MD

## Follow-up query patterns

After best-3 is set, open **new sessions** (never one mega-thread):

| Trigger | Example query |
|---------|---------------|
| Method clash | `Does [A 2025] contradict [B 2022] [method name]?` |
| Counter-evidence | `counter evidence to [Author Year] [claim]` |
| Similar papers | `papers similar to doi [10.xxxx/yyyy]` |
| Deep axis | `Does [specific claim] hold at individual level?` |

Extract **both** Consensus synthesis **and** limitations / meter footnotes on the same page.

## Additional resources

- **Browser MCP + Consensus UI:** [browser-consensus.md](browser-consensus.md) ← start here for live search
- **PDF / literature-folder download:** [browser-pdf-download.md](browser-pdf-download.md) ← OA, DataDome, local index
- Query templates and MD snippets: [reference.md](reference.md)
- Worked example (hearable occlusion — structure only): [examples.md](examples.md)

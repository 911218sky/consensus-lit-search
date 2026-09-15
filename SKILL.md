---
name: consensus-lit-search
description: >
  Multi-session literature search on Consensus.app with cross-viewpoint
  verification, debate-map synthesis, DOI validation, best-3 viewpoint
  triangulation, runner-up tiering, and structured updates to project markdown.
  Use when the user asks to find papers, verify debates, compare opposing claims,
  deep-dive similar work, cross-check with Consensus, or update literature-review
  documents. Trigger phrases: Consensus, cross-viewpoint verification, debate map,
  best-3 triangle, deep dive, counter-evidence.
license: AGPL-3.0-or-later
metadata:
  short-description: Consensus multi-session lit search with viewpoint triangulation
  version: 1.0.0
---

# Consensus Literature Search

Search academic literature through **multiple independent Consensus sessions**, verify opposing viewpoints, pick a **best-3 triangle**, tier runners-up, and write results into the project markdown file the user specifies.

## When to use

- Literature review where **different viewpoints** must be verified, not summarized in one thread
- Picking **best 3 papers** that represent distinct positions
- Updating a project literature markdown file after Consensus exploration
- User says: use Consensus, cross-viewpoint verification, debate map, best-3, deep dive, counter-evidence

## Non-negotiables

1. **Separate sessions per debate axis** — never cram all questions into one Consensus thread.
2. **Verify DOIs** via Crossref before adding papers to the output file.
3. **Do not inflate the tree** — tier runners-up; appendix for footnotes unless the user asks to expand.
4. **Honest evidence scope** — distinguish metadata/abstract vs full-text read; never fabricate PDF access.
5. **Save full Consensus URLs** including hash IDs; bare slug URLs often 404.

## Project alignment (optional)

When the user or project rules define research constraints, apply them on top of this workflow. Example (hearable ANC / occlusion):

- Treat dual-mic difference metrics as **profile**, not ground truth, until measurement validity is shown.
- Use **Conditional Go**: validity → subjective endpoint → bounded intervention → human trials only after gates pass.

Ask the user for the output file path and any project-specific red lines if not already known.

## Workflow checklist

Copy and track:

```
Progress:
- [ ] 1. Scope: claim, debate axes, year filter, output file path
- [ ] 2. Open ≥1 Consensus session per debate axis (see reference.md)
- [ ] 3. Extract: synthesis, meter, key papers, FOR/AGAINST if shown
- [ ] 4. Crossref-verify DOIs (OpenAlex only if budget allows)
- [ ] 5. Pick best-3 triangle (3 non-overlapping viewpoints)
- [ ] 6. Tier runners-up (★★★ / ★★☆ / ★☆☆ or equivalent)
- [ ] 7. Update project MD sections (template in reference.md)
- [ ] 8. Log session URLs with hash IDs in MD header
- [ ] 9. Mark evidence scope honestly (abstract vs full text)
```

## Step 1 — Define debate axes

Before searching, list 3–5 **tension pairs** relevant to the topic. Generic template:

| Axis | Side A | Side B |
|------|--------|--------|
| A | Objective measure predicts subjective outcome (group/fitting level) | Weak or absent at individual level |
| B | Field-deployable measurement method | Systematic bias or validity concerns |
| C | Open / less invasive design improves user experience | Closed / sealed design wins performance metric |
| D | Intervention works in controlled settings | Real-world limits, instability, or safety issues |
| E | Modeling / proxy endpoint | Not validated for the target clinical or product claim |

Replace rows with domain-specific claims. Add axes only if the user asks.

## Step 2 — Multi-session Consensus search

Use **browser MCP** when available (e.g. `cursor-ide-browser`). The user may already have Consensus open.

1. `browser_tabs` list → find existing Consensus tab or open `https://consensus.app`
2. **New search per axis** — New Thread / fresh navigate; do not reuse one thread for all axes
3. Query patterns (English works best on Consensus):
   - Neutral overview: `[topic] objective subjective [year range]`
   - Viewpoint question: `Does [measure] correlate with [outcome] at individual level?`
   - Method debate: `[method A] systematic error bias [domain]`
   - Counter-evidence: `counter evidence to [Author Year] [claim]`
   - Deep-dive: `papers similar to doi [10.xxxx/yyyy]`
4. After each search completes, **save full URL including hash** from sidebar or address bar
5. Extract via `browser_snapshot` or `browser_cdp` → `Runtime.evaluate` on `document.body.innerText` when snapshot is thin
6. Record: Consensus synthesis, Yes/No meter (if any), top 5–10 references, limitations block

**Session budget:** 4–7 sessions for a full pass; 1–2 for a narrow follow-up.

## Step 3 — Verify DOIs

For every paper that will appear in the output:

```bash
curl -s "https://api.crossref.org/works/DOI_HERE" | python3 -c "
import sys,json; m=json.load(sys.stdin)['message']
print(m.get('title',['?'])[0], m.get('published-print') or m.get('published-online'))
"
```

- If Crossref empty or rate-limited: retry once; note in evidence scope
- OpenAlex: optional; often rate-limited — do not block on it
- PDF download may hit paywalls or bot protection; cite DOI + abstract only

## Step 4 — Best-3 triangle

Pick **exactly three** papers where each represents a **different viewpoint**, not three papers that agree:

| Slot | Typical role | What to look for |
|------|--------------|------------------|
| 1 | Measurement / validity skeptic | Quantifies bias, limits, or failed assumptions of common methods |
| 2 | Objective–subjective dissociation | Shows group patterns but weak individual prediction |
| 3 | Structural / design tradeoff | Forces a product or study design choice between competing goals |

**30-second narrative:** slot1 → slot2 → slot3 → project verdict.

Reject a candidate if it duplicates an existing slot (e.g. two papers on the same side of one tradeoff).

## Step 5 — Tier runners-up

| Tier | Meaning | Action in MD |
|------|---------|--------------|
| ★★★ | Must cite in main report | Full section |
| ★★☆ | Strong backup / debate partner | Runners-up table + short note |
| ★☆☆ | Appendix / if asked | Table row or footnote only |

Include **counter-evidence** papers in ★★☆ — debates are features, not noise.

## Step 6 — Update project markdown

Ask the user for the target file if unknown. Required sections for a full pass:

1. Header: date, session URLs, link to this skill or repo
2. Best-3 section — table + 30s narrative
3. Runners-up — tier table with DOI + when to use
4. Debate map — FOR/AGAINST tables per axis
5. Evidence scope — what was read vs metadata-only

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
- Distinguish **population/fitting-level** vs **individual-level** claims when relevant
- Quote Consensus synthesis only when checked against the References list on the same page
- Never claim full-text read unless PDF was actually opened
- Match the user's language for user-facing MD; use English queries on Consensus

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

- Query templates and MD snippets: [reference.md](reference.md)
- Full worked example (8 sessions, hearable occlusion): [examples.md](examples.md)

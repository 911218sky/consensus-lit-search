# Consensus Lit Search — Reference

Generic query templates, browser MCP sequence, markdown snippets, then optional domain anchors.

## Generic query skeleton (use first)

Replace `[TOPIC]` / `[CLAIM]` / `[AUTHOR YEAR]` / `[DOI]`. One clear question per session.

```
Session: overview
Query: [TOPIC] [key outcome] evidence review [year range]

Session: viewpoint A vs B
Query: Does [measure or intervention] [predict / cause / correlate with] [outcome] at [group|individual] level?

Session: method debate
Query: [method A] versus [method B] systematic error bias validity [domain]

Session: counter-evidence
Query: counter evidence to [AUTHOR YEAR] [CLAIM]

Session: similar papers
Query: papers similar to doi [10.xxxx/yyyy] [TOPIC]

Session: lab vs real-world
Query: [intervention] efficacy limitations real-world [population]
```

### Domain example: hearable occlusion (English) — skip if unrelated

```
Session: occlusion overview
Query: hearing aid hearable occlusion effect own voice objective subjective 2020-2026

Session: correlation debate
Query: Does real ear occlusion effect correlate with subjective own voice naturalness individual level?

Session: dual-mic validity
Query: dual microphone in-ear out-ear occlusion measurement systematic error hearable

Session: fitting tradeoff
Query: open vent versus closed coupling hearing aid own voice noise reduction tradeoff

Session: active OEC
Query: active occlusion cancellation limitations subjective own voice hearable

Session: personalized ANC profile
Query: personalized ANC self voice profile occlusion hearable 2024-2026

Session: counter-evidence
Query: counter evidence to [AUTHOR YEAR] occlusion effect subjective correlation

Session: similar papers
Query: papers similar to doi [10.xxxx/yyyy] own voice occlusion
```

### Reading Consensus output

| UI element | Use |
|------------|-----|
| Top synthesis paragraph | Draft debate-map summary row |
| Yes/No meter | Report both result **and** limitations on same page |
| References list | DOI harvest list; verify each via Crossref |
| Copilot follow-ups | Seed next **separate** session, not inline only |

### URL rules

- ✅ `https://consensus.app/search/slug-name/HASH_ID/`
- ❌ `https://consensus.app/search/slug-name/` (often 404)
- Copy from browser address bar **after** search finishes loading
- No real session → do not invent a URL (see SKILL.md degradation path)

### Session capture card (copy per session)

```
- Session title:
- Full URL (with hash):
- Query asked:
- Synthesis (2–4 sentences):
- Meter (if any) + limitations note:
- Top DOIs harvested:
- Evidence level: Consensus UI | user paste | Crossref only
```

## Browser MCP (full guide)

**Agents:** use **[browser-consensus.md](browser-consensus.md)** — UI map, wait strategy, CDP extract, failure table, worked example.

Quick sequence (MCP server **`cursor-ide-browser`**):

```
1. browser_tabs { action: "list" }                    # reuse tab; note viewId
2. browser_navigate { url: "https://consensus.app" }  # never slug-only /search/... URLs
3. browser_lock { viewId: "..." }                     # AFTER navigate
4. browser_snapshot → ref for textbox "Ask the research..."
5. browser_fill { ref, value: "<one English question>" }
6. browser_click { ref: "Submit search" }
7. wait ≤3× (~8s) → browser_snapshot until URL has /search/<slug>/<HASH>/
8. browser_cdp Runtime.evaluate document.body.innerText  # if snapshot thin
9. copy hash URL + References DOIs → Crossref verify
10. browser_lock { action: "unlock" }
11. New Thread → repeat for next debate axis
```

If step 7 fails → unlock, degradation path in SKILL.md. Do not loop snapshots more than ~3 times.

## Crossref one-liner

```bash
curl -s "https://api.crossref.org/works/10.1051/aacus/2025055" \
  | python3 -c "import sys,json; m=json.load(sys.stdin)['message']; print(m['title'][0])"
```

Batch pattern: loop DOIs from Consensus References; skip on HTTP 404.

## MD section templates (language-neutral / project-neutral)

Adapt language to the user. Do **not** insert domain jargon unless the project uses it.

### Header block

```markdown
> Date: YYYY-MM-DD (multi-session Consensus cross-check; Crossref DOI verify)
> Method: [consensus-lit-search](https://github.com/911218sky/consensus-lit-search)
> Consensus threads (YYYY-MM-DD):
> - [Session Title](full-url-with-hash/)
> Evidence note: Consensus UI | user paste | Crossref only
```

### Best-3 triangle

```markdown
## Best 3 papers (viewpoint triangle)

| # | Viewpoint | Paper | DOI | One-liner |
|---:|---|---|---|---|
| 1 | **…** | Author et al. YEAR, *Venue* | [10.xxxx/yyyy](https://doi.org/10.xxxx/yyyy) | … |
| 2 | **…** | … | … | … |
| 3 | **…** | … | … | … |

**30-second narrative:** … → [project verdict in the user's terms].
```

### Runners-up table

```markdown
## Runners-up (verified / worth a deep dive)

| Priority | Paper | DOI | Role | When to use |
|:---:|---|---|---|---|
| ★★☆ | … | [10.xxxx/yyyy](https://doi.org/10.xxxx/yyyy) | … | … |
```

### Debate map row

```markdown
### Debate X: …

| Position | Key papers | What they claim | Implication for this project |
|---|---|---|---|
| **…** | Author YEAR (`10.xxxx/yyyy`) | … | … |
| **…** | … | … | … |

**Consensus synthesis (only if session or paste exists):** "…"
```

### Evidence scope footer

```markdown
## Evidence scope

- Sources: Consensus N session(s) and/or user paste + Crossref; mostly metadata + synthesis + abstracts unless noted.
- **YYYY-MM-DD DOIs verified:** …
- If Consensus UI was not run: state degradation path used; do not list fake session URLs.
- PDFs not obtained: mark paywall/block; do not claim full-text read.
```

## Best-3 selection rubric

Score each candidate 0–2 on:

1. **Viewpoint distinctness** — not redundant with other two
2. **Decision relevance** — changes what **this project** can claim
3. **Evidence quality** — peer-reviewed, sample size, method clarity
4. **Recency** — bonus if tie-break (adjust year window to the topic)
5. **DOI verified** — mandatory gate

Minimum total 6/10 to enter triangle; runners-up 4–5.

---

## Domain anchors (OPTIONAL — hearable occlusion case study)

Only use when the user's topic is hearable ANC / occlusion / own-voice. Otherwise ignore this entire section. Full walkthrough: [examples.md](examples.md).

### DOI quick ref

| Paper | DOI |
|-------|-----|
| Blau 2025 | 10.1051/aacus/2025055 |
| Ertürk 2026 | 10.1097/aud.0000000000001870 |
| Denk 2022 IJA | 10.1080/14992027.2022.2039966 |
| Súsonnudóttir 2026 | 10.1177/23312165261435260 |
| Saint-Gaudens 2022 | 10.1121/10.0011696 |
| Cubick 2022 | 10.1097/aud.0000000000001239 |
| Moore 2026 | 10.1007/s10162-026-01031-5 |
| Ohlenbusch 2025 | 10.1186/s13636-025-00418-1 |

### Known Consensus session URLs (2026-09-15)

| Session | Full URL |
|---------|----------|
| Best-3 triangle | https://consensus.app/search/personalized-anc-hearables-voice-occlusion/OeqxV3hgRPGL-2gXrA-dKQ/ |
| Blau vs Saint-Gaudens | https://consensus.app/search/dual-microphone-noise-reduction-occlusion/E-LigHHOQo6NtXwQToxKQA/ |
| Correlation debate | https://consensus.app/search/real-ear-occlusion-effect-correlation/sgjkUnNnTSm1wOq-72k7hw/ |
| Dual-mic bias | https://consensus.app/search/dual-microphone-occlusion-measurement/h1cgm7UVQd-N7iEYTDxOrQ/ |
| Fitting tradeoffs | https://consensus.app/search/hearing-aid-fitting-tradeoffs/zK2sAxX6TJy3r9u9i7ZM0w/ |
| AOC effects | https://consensus.app/search/active-occlusion-cancellation-effects/_Gd-_sUBSPahiQA_DNnU_w/ |
| Occlusion overview | https://consensus.app/search/hearing-aid-occlusion-effects/W3VGtrJSS_OXEv8EtNUIlw/ |
| Counter-evidence | https://consensus.app/search/objective-occlusion-and-own-voice-ratings/bXKL4VlMSWKHMLJ2psIGAw/ |

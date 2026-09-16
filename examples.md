# Consensus Lit Search — Worked Example (PANC2.1)

> **Case study only.** Copy the *structure* (multi-session list, best-3 table, stop rules).  
> Do **not** treat PANC jargon, DOIs, or Conditional Go as defaults for unrelated topics.  
> **How to run each session in the browser:** [browser-consensus.md](browser-consensus.md).  
> **How to download OA PDFs:** [browser-pdf-download.md](browser-pdf-download.md).

Case study from 2026-09-15: hearable ANC, occlusion, and own-voice naturalness literature. Reuse the **session structure and query patterns** for other domains; replace papers and debate axes accordingly.

## Task definition

**Goal:** Find high-relevance 2025–2026 papers for a report, verify opposing viewpoints, pick a best-3 triangle, write into project MD.  
**Output file (project-specific path):** `PANC2.1/報告用文獻詳解_全名內容與研究好處.md`  
**Constraints:** Keep the citation tree small; call `P(f)` a profile (not OE ground truth); Conditional Go.

## Session list (8 independent threads)

| # | Session | URL hash | What to ask |
|---|---------|----------|-------------|
| 1 | Personalized ANC Hearables Voice Occlusion | `OeqxV3hgRPGL-2gXrA-dKQ` | **Best-3:** one paper per opposing viewpoint |
| 2 | Dual Microphone Noise Reduction Occlusion | `E-LigHHOQo6NtXwQToxKQA` | Does Blau contradict Saint-Gaudens? |
| 3 | Real Ear Occlusion Effect Correlation | `sgjkUnNnTSm1wOq-72k7hw` | Objective OE ↔ subjective naturalness (FOR/AGAINST) |
| 4 | Dual Microphone Occlusion Measurement | `h1cgm7UVQd-N7iEYTDxOrQ` | Dual-mic measurement bias |
| 5 | Hearing Aid Fitting Tradeoffs | `zK2sAxX6TJy3r9u9i7ZM0w` | open/vent vs closed/sealed |
| 6 | Active Occlusion Cancellation Effects | `_Gd-_sUBSPahiQA_DNnU_w` | AOC works in lab vs real-world limits |
| 7 | Hearing Aid Occlusion Effects | `W3VGtrJSS_OXEv8EtNUIlw` | Overview + hearable dual-mic 2025–26 |
| 8 | Objective Occlusion and Own Voice Ratings | `bXKL4VlMSWKHMLJ2psIGAw` | Ertürk counter-evidence |

## Best-3 result (Consensus triangle)

| Viewpoint | Paper | DOI |
|-----------|-------|-----|
| Measurement-skeptic | Blau et al. 2025 | 10.1051/aacus/2025055 |
| Objective–subjective dissociation | Ertürk & Best 2026 | 10.1097/aud.0000000000001870 |
| Structural tradeoff | Denk et al. 2022 | 10.1080/14992027.2022.2039966 |

**30-second pitch:** Blau (what did we measure?) → Denk (why sealed is harder) → Ertürk (do not use dB alone) → Conditional Go.

## Key Consensus synthesis lines (quotable)

**Debate A (correlation session):**  
Evidence from 2005–2026 supports objective–subjective co-direction at the **fitting-design level**, but does **not** support reliable one-to-one mapping at the **individual level**.

**Debate B (Dual Mic NR session):**  
“Saint-Gaudens validated the method, while Blau quantifies its failure” — ~3–4 dB systematic error; no independent replication yet.

**Debate D (AOC session):** Meter often ~Yes for lab reduction of subjective occlusion, but the same session lists limitations (switching errors, secondary-path change, limited commercial uptake).

## Runner-up picks (★★☆)

| Paper | When to use |
|-------|-------------|
| Súsonnudóttir 2026 | Delay / comb-filter → OVQ, not pure OE |
| Cubick 2022 | Counter to Ertürk (fitting-level correlation still holds) |
| Jürgens 2025 | Closed coupling → DIR+NR benefit |
| Moore 2026 | Active vent: first-person limits |
| Saint-Gaudens 2022 | Field-deployable dual-mic contrast to Blau |
| Zhang 2024 JASA | IEM/OEM speech quality ≠ mouth-reference norms |

## Query examples (copy-paste)

```
# Best-3
For personalized ANC hearables targeting own-voice naturalness and occlusion: pick the 3 highest-quality peer-reviewed papers (2020-2026) that represent THREE DIFFERENT viewpoints — (1) measurement validity skepticism, (2) objective-subjective dissociation, (3) sealed-fit vs open-fit tradeoff.

# Method clash
Does Blau 2025 contradict Saint-Gaudens 2022 dual-microphone noise-reduction method for objective occlusion effect? What is the strongest counter-evidence to Blau and strongest support for dual-mic OE measurement?

# Counter-evidence
counter evidence to Ertürk 2026 occlusion effect subjective correlation individual level

# Similar papers
papers similar to doi 10.1051/aacus/2025055 own voice occlusion hearable
```

## DOI verification (as used)

```bash
for doi in 10.1051/aacus/2025055 10.1097/aud.0000000000001870 10.1080/14992027.2022.2039966; do
  curl -s "https://api.crossref.org/works/$doi" | python3 -c "import sys,json; m=json.load(sys.stdin)['message']; print(m['title'][0][:60])"
done
```

## Stop rules (used this round)

- User asked to keep the tree small → core 4+5; rest in a runner-up table
- OpenAlex rate limit → do not block; Crossref is enough for DOI checks
- Blau PDF DataDome → mark metadata-only unless browser fetch succeeds (see [browser-pdf-download.md](browser-pdf-download.md))
- Middelberg 2025 is a different axis from ear-canal OE measurement → ★☆☆ footnote only

## Pairing with critical-research-reviewer

After Consensus, open a `standard` review on claims such as:

1. “Dual-mic profile = OE ground truth”
2. “Objective dB improvement substitutes for the subjective S-axis”
3. “Open-fit vent solves sealed ANC occlusion without tradeoff”

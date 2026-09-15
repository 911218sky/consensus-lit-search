# Consensus Literature Search

A portable Markdown skill for multi-session literature search on [Consensus.app](https://consensus.app), cross-viewpoint verification, best-3 viewpoint triangulation, and structured updates to project markdown.

**Version:** 1.0.1

## 30-second start

1. Give the agent your **topic**, **output markdown path**, and mode (`lite` or `full`).
2. Define **3 debate axes** for *your* field (do not copy hearable/ANC examples unless that is your topic).
3. Agent opens **one Consensus thread per axis** (browser MCP), or asks you to paste results if the browser is unavailable.
4. Agent verifies DOIs with Crossref, picks a **best-3 viewpoint triangle**, tiers runners-up, writes the MD.
5. Anything labeled EXAMPLE / domain anchor in this repo is **optional** — delete or ignore for other fields.

## What it does

- Runs **separate Consensus sessions per debate axis** instead of one overloaded thread.
- Extracts synthesis, Yes/No meters, references, and limitations from each session.
- Verifies DOIs via Crossref before citing papers.
- Picks a **best-3 triangle**: three papers that represent **distinct opposing viewpoints**, not three papers that agree.
- Tiers runners-up (must cite / strong backup / appendix) to keep the literature tree focused.
- Writes structured sections into project markdown: best-3 table, debate map, runners-up, evidence scope.
- **Degrades honestly** when browser MCP / Consensus is unavailable (no fake session URLs).
- Supports follow-up sessions for counter-evidence, method clashes, and similar-paper deep dives.
- Optionally pairs with [critical-research-reviewer](https://github.com/911218sky/critical-research-reviewer) for adversarial claim review after synthesis.

The skill never fabricates DOIs, full-text reads, or Consensus session URLs. Short Consensus URLs without hash IDs often 404 — always save the full URL from the browser after search completes.

## When to use

- Literature review where **opposing claims** need verification, not a single search summary.
- Picking **best 3 papers** across distinct tension axes for your topic.
- Updating a project literature markdown file after Consensus exploration.
- Trigger phrases: Consensus, cross-viewpoint verification, debate map, best-3 triangle, deep dive, counter-evidence.

## Requirements

- **Preferred:** Browser MCP (e.g. `cursor-ide-browser`) + access to Consensus.app (login if required).
- Network access for Crossref DOI verification.
- **Without browser MCP:** agent must use the degradation path (user paste / DOI-only) — install still works; live Consensus automation does not.
- Optional: OpenAlex or PDF access — do not block on rate limits or paywalls.

## Installation

Copy the skill directory into any Agent or assistant environment that supports Markdown-based skills. No package manager or language runtime is required for the skill files themselves.

Example layout:

```text
skills/
└── consensus-lit-search/
    ├── SKILL.md
    ├── reference.md
    ├── examples.md
    ├── README.md
    ├── CHANGELOG.md
    └── LICENSE
```

After installation, ask the assistant to search Consensus for papers on your topic, verify opposing viewpoints, or update your literature notes. Point it at your output markdown file path.

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Main workflow: axes, multi-session search, degradation, best-3, tiering, MD updates |
| `reference.md` | Generic query skeleton, browser sequence, MD snippets; optional domain anchors at end |
| `examples.md` | Case study only (hearable occlusion) — copy structure, not default jargon |

## Scope

This is a **general-purpose** literature-search workflow. Hearable ANC / occlusion material lives in `examples.md` and the optional footer of `reference.md` as a worked case study, not as mandatory protocol.

## Related

- [critical-research-reviewer](https://github.com/911218sky/critical-research-reviewer) — rigorous post-synthesis claim review

## License

GNU Affero General Public License v3.0 or later. See [`LICENSE`](LICENSE).

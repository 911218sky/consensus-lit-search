# Consensus Literature Search

A portable Markdown skill for multi-session literature search on [Consensus.app](https://consensus.app), cross-viewpoint verification, best-3 viewpoint triangulation, and structured updates to project markdown.

## What it does

- Runs **separate Consensus sessions per debate axis** instead of one overloaded thread.
- Extracts synthesis, Yes/No meters, references, and limitations from each session.
- Verifies DOIs via Crossref before citing papers.
- Picks a **best-3 triangle**: three papers that represent **distinct opposing viewpoints**, not three papers that agree.
- Tiers runners-up (must cite / strong backup / appendix) to keep the literature tree focused.
- Writes structured sections into project markdown: best-3 table, debate map, runners-up, evidence scope.
- Supports follow-up sessions for counter-evidence, method clashes, and similar-paper deep dives.
- Optionally pairs with [critical-research-reviewer](https://github.com/911218sky/critical-research-reviewer) for adversarial claim review after synthesis.

The skill never fabricates DOIs, full-text reads, or Consensus session URLs. Short Consensus URLs without hash IDs often 404 — always save the full URL from the browser after search completes.

## When to use

- Literature review where **opposing claims** need verification, not a single search summary.
- Picking **best 3 papers** that span measurement validity, objective–subjective dissociation, structural tradeoffs, or other tension axes.
- Updating a project literature markdown file after Consensus exploration.
- Trigger phrases: Consensus, cross-viewpoint verification, debate map, best-3 triangle, deep dive, counter-evidence.

## Requirements

- Browser access to Consensus.app (typically via a browser MCP such as `cursor-ide-browser`).
- Network access for Crossref DOI verification.
- Optional: OpenAlex or PDF access — do not block on rate limits or paywalls.

## Installation

Copy the skill directory into any Agent or assistant environment that supports Markdown-based skills. No package manager or runtime is required.

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
| `SKILL.md` | Main workflow: debate axes, multi-session search, best-3, tiering, MD updates |
| `reference.md` | Query templates, browser MCP sequence, MD section snippets |
| `examples.md` | Full case study: PANC2.1 occlusion / self-voice literature (8 sessions) |

## Scope

This is a general-purpose literature-search workflow skill. Domain examples in `examples.md` use hearable ANC / occlusion research but the protocol applies to any field where Consensus can surface peer-reviewed papers and methodological debates.

## Related

- [critical-research-reviewer](https://github.com/911218sky/critical-research-reviewer) — rigorous post-synthesis claim review

## License

GNU Affero General Public License v3.0 or later. See [`LICENSE`](LICENSE).

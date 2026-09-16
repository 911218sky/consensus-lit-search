# Changelog

## 1.3.1 - 2026-09-16

- **English-only skill prose:** rewrote [browser-pdf-download.md](browser-pdf-download.md) and [examples.md](examples.md) in full English (Chinese only as literal project file paths).
- **browser-pdf-download.md**: WSL path for CDP browser-logs (`/mnt/c/Users/…`); Blau 2025 DataDome success note.
- Version bump to 1.3.1.

## 1.3.0 - 2026-09-16

- New **[browser-pdf-download.md](browser-pdf-download.md)**: legal OA PDF pipeline (OpenAlex → curl → browser `fetch`+base64 for DataDome/403), figure extract, local path index, failure table.
- **SKILL.md**: Step 3b download when user asks; non-negotiables for legal PDF + 403→browser; checklist item 11; trigger phrases for PDF/full text.
- Version bump to 1.3.0.

## 1.2.0 - 2026-09-15

- **Pro messages vs Study Snapshots** documented with live UI workflow (References → Snapshot tab; Table batch view).
- Corrected **Free tier quotas**: 10 Pro messages/month and 10 Study Snapshots/month (was 15).
- Expanded feature tier table: Papers search vs Pro messages vs Deep reviews vs Snapshots.
- Session capture cards now include mode, Snapshots taken, and quota notes.
- UI map: Pro · N steps badge, References Table view, Snapshot tab, follow-up box.
- reference.md: Snapshot row template for best-3 / runners-up tables.

## 1.1.0 - 2026-09-15

- New **`browser-consensus.md`**: agent cheat sheet for `cursor-ide-browser` + Consensus.app — 60s workflow, UI map, wait/extract, quotas, failure table, worked example.
- **SKILL.md**: "Browser first" section at top; Step 2 points to browser guide; Free-tier Pro message budget note.
- **reference.md** / **README.md**: browser section expanded; README one-glance MCP sequence.

## 1.0.1 - 2026-09-15

- De-domain default scaffolds: blank debate-axis and best-3 slot tables; hearable roles marked EXAMPLE ONLY.
- Neutral MD templates and rubric ("this project") — removed mandatory PANC2.1 / Conditional Go from copy-paste blocks.
- Non-negotiables: never invent Consensus sessions; domain examples optional.
- Browser / Consensus **degradation path** when MCP or UI is unavailable.
- `lite` / `full` modes; generic query skeleton and session capture card in `reference.md`.
- README 30-second start; clearer Requirements vs "copy files only".

## 1.0.0 - 2026-09-15

- Initial release as a portable Consensus literature-search skill.
- Multi-session workflow with debate-axis separation and best-3 viewpoint triangulation.
- Crossref DOI verification, runner-up tiering, and structured markdown output templates.
- Follow-up query patterns for counter-evidence, method clashes, and similar papers.
- Browser MCP sequence for Consensus.app extraction.
- PANC2.1 hearable occlusion case study in `examples.md`.
- GNU Affero General Public License v3.0 or later.

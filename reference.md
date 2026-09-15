# Consensus Lit Search — Reference

Query templates, browser MCP sequence, markdown snippets, and optional domain anchors.

## Consensus query templates

Replace `[TOPIC]` / `[AUTHOR YEAR]` as needed. Prefer one clear question per session.

### Domain example: hearable occlusion / PANC2.1 (English)

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
| Top synthesis paragraph | Draft 辯論地圖 summary row |
| Yes/No meter | Report both result **and** limitations on same page |
| References list | DOI harvest list; verify each via Crossref |
| Copilot follow-ups | Seed next **separate** session, not inline only |

### URL rules

- ✅ `https://consensus.app/search/slug-name/HASH_ID/`
- ❌ `https://consensus.app/search/slug-name/` (often 404)
- Copy from browser address bar **after** search finishes loading

## Browser MCP quick sequence

```
1. browser_tabs { action: "list" }
2. browser_navigate { url: "https://consensus.app" }
3. browser_lock { action: "lock" }
4. browser_snapshot → find search input ref
5. browser_fill / browser_type → submit query
6. wait: browser_snapshot loop or CDP until synthesis visible
7. browser_cdp Runtime.evaluate → document.body.innerText (fallback)
8. copy URL with hash → paste into MD header
9. browser_lock { action: "unlock" }
```

## Crossref one-liner

```bash
curl -s "https://api.crossref.org/works/10.1051/aacus/2025055" \
  | python3 -c "import sys,json; m=json.load(sys.stdin)['message']; print(m['title'][0])"
```

Batch pattern: loop DOIs from Consensus References; skip on HTTP 404.

## MD section templates

### Header block

```markdown
> 整理日期：YYYY-MM-DD（Consensus 多 session 交叉驗證；Crossref 核 DOI）
> 查詢方法：見 [`.agents/skills/consensus-lit-search/SKILL.md`](../.agents/skills/consensus-lit-search/SKILL.md)
> Consensus threads（YYYY-MM-DD）：
> - [Session Title](full-url-with-hash/)
```

### Best-3 triangle

```markdown
## 若只講 3 篇（Consensus 三角架｜三種對立觀點）

| # | 觀點 | 論文 | DOI | 報告一句 |
|---:|---|---|---|---|
| 1 | **…** | Author et al. YEAR, *Venue* | [10.xxxx/yyyy](https://doi.org/10.xxxx/yyyy) | … |
| 2 | **…** | … | … | … |
| 3 | **…** | … | … | … |

**30 秒串講：** … → Conditional Go。
```

### Runners-up table

```markdown
## 延伸推薦（Consensus 驗證｜仍值得讀／深入）

| 優先 | 論文 | DOI | 觀點／角色 | 何時深入 |
|:---:|---|---|---|---|
| ★★☆ | … | [10.xxxx/yyyy](https://doi.org/10.xxxx/yyyy) | … | … |
```

### Debate map row

```markdown
### 辯題 X：…

| 立場 | 代表文獻 | 他們說什麼 | 對 PANC2.1 的啟示 |
|---|---|---|---|
| **…** | Author YEAR（`10.xxxx/yyyy`） | … | … |
| **…** | … | … | … |

**Consensus 合成句（可原封不動講）：** 「…」
```

### Evidence scope footer

```markdown
## 證據範圍（誠實標註）

- 本檔整理自 Consensus N session + Crossref；多數為 metadata + Consensus 合成 + 摘要。
- **YYYY-MM-DD 新核 DOI**：…
- PDF 未取得時標註出版社阻擋，不宣稱 full-text read。
```

## Best-3 selection rubric

Score each candidate 0–2 on:

1. **Viewpoint distinctness** — not redundant with other two
2. **Decision relevance** — changes what PANC2.1 can claim
3. **Evidence quality** — peer-reviewed, sample size, method clarity
4. **Recency** — 2024–2026 bonus if tie-break
5. **DOI verified** — mandatory gate

Minimum total 6/10 to enter triangle; runners-up 4–5.

## Domain example anchors (DOI quick ref — hearable occlusion)

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

Update this table when new core papers are promoted to ★★★.

## Known Consensus session URLs (PANC2.1, 2026-09-15)

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

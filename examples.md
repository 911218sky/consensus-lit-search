# Consensus Lit Search — Worked Example (PANC2.1)

> **Case study only.** Copy the *structure* (multi-session list, best-3 table, stop rules).  
> Do **not** treat PANC jargon, DOIs, or Conditional Go as defaults for unrelated topics.

Case study from 2026-09-15: hearable ANC, occlusion, and own-voice naturalness literature. Reuse the **session structure and query patterns** for other domains; replace papers and debate axes accordingly.

## 任務定義

**目標：** 為報告找 2025–2026 高相關文献，驗證不同觀點，選 best-3 三角架，寫入 MD。  
**輸出：** `PANC2.1/報告用文獻詳解_全名內容與研究好處.md`  
**約束：** 樹不要太多；`P(f)` = profile；Conditional Go。

## Session 清單（8 個獨立 thread）

| # | Session | URL hash | 問什麼 |
|---|---------|----------|--------|
| 1 | Personalized ANC Hearables Voice Occlusion | `OeqxV3hgRPGL-2gXrA-dKQ` | **Best-3**：三種對立觀點各一篇 |
| 2 | Dual Microphone Noise Reduction Occlusion | `E-LigHHOQo6NtXwQToxKQA` | Blau 是否 contradict Saint-Gaudens？ |
| 3 | Real Ear Occlusion Effect Correlation | `sgjkUnNnTSm1wOq-72k7hw` | 客觀 OE ↔ 主觀自然度（FOR/AGAINST） |
| 4 | Dual Microphone Occlusion Measurement | `h1cgm7UVQd-N7iEYTDxOrQ` | 雙麥量測偏差 |
| 5 | Hearing Aid Fitting Tradeoffs | `zK2sAxX6TJy3r9u9i7ZM0w` | open/vent vs closed/sealed |
| 6 | Active Occlusion Cancellation Effects | `_Gd-_sUBSPahiQA_DNnU_w` | AOC 有效 vs 現實限制 |
| 7 | Hearing Aid Occlusion Effects | `W3VGtrJSS_OXEv8EtNUIlw` | 總覽 + hearable dual-mic 2025–26 |
| 8 | Objective Occlusion and Own Voice Ratings | `bXKL4VlMSWKHMLJ2psIGAw` | Ertürk counter-evidence |

## Best-3 結果（Consensus 三角架）

| 觀點 | 論文 | DOI |
|------|------|-----|
| 量測懷疑派 | Blau et al. 2025 | 10.1051/aacus/2025055 |
| 主客觀解離派 | Ertürk & Best 2026 | 10.1097/aud.0000000000001870 |
| 結構取捨派 | Denk et al. 2022 | 10.1080/14992027.2022.2039966 |

**30 秒串講：** Blau（量到什麼）→ Denk（sealed 更難）→ Ertürk（不能只看 dB）→ Conditional Go。

## 關鍵 Consensus 合成句（可直接引用）

**辯題 A（correlation session）：**  
「2005–2026 證據支持 **fitting 設計層級** 的客觀–主觀同向，但不支持 **個體層級** 的可靠一一對應。」

**辯題 B（Dual Mic NR session）：**  
「Saint-Gaudens validated the method, while Blau quantifies its failure」— 3–4 dB systematic error；尚無 independent replication。

**辯題 D（AOC session）：** Meter 100% Yes「實驗室可降主觀堵耳」，但 limitations 同頁列切換錯誤、次級路徑變等。

## 延伸推薦（★★☆ 精選）

| 論文 | 何時用 |
|------|--------|
| Súsonnudóttir 2026 | 延遲／梳狀 → OVQ，非純 OE |
| Cubick 2022 | 反駁 Ertürk（fitting 層級仍相關） |
| Jürgens 2025 | 封閉 coupling → DIR+NR |
| Moore 2026 | active vent 親身試限制 |
| Saint-Gaudens 2022 | Blau 的現場簡便法對照 |
| Zhang 2024 JASA | IEM/OEM 語音品質 ≠ 口前參考 |

## 查詢範例（複製即用）

```
# Best-3
For personalized ANC hearables targeting own-voice naturalness and occlusion: pick the 3 highest-quality peer-reviewed papers (2020-2026) that represent THREE DIFFERENT viewpoints — (1) measurement validity skepticism, (2) objective-subjective dissociation, (3) sealed-fit vs open-fit tradeoff.

# 方法論對打
Does Blau 2025 contradict Saint-Gaudens 2022 dual-microphone noise-reduction method for objective occlusion effect? What is the strongest counter-evidence to Blau and strongest support for dual-mic OE measurement?

# Counter-evidence
counter evidence to Ertürk 2026 occlusion effect subjective correlation individual level

# 深入相似
papers similar to doi 10.1051/aacus/2025055 own voice occlusion hearable
```

## DOI 驗證（實際用過）

```bash
for doi in 10.1051/aacus/2025055 10.1097/aud.0000000000001870 10.1080/14992027.2022.2039966; do
  curl -s "https://api.crossref.org/works/$doi" | python3 -c "import sys,json; m=json.load(sys.stdin)['message']; print(m['title'][0][:60])"
done
```

## 停止條件（本輪實際採用）

- 用戶說「樹不要太多」→ 核心 4+5，其餘放延伸表
- OpenAlex rate limit → 不阻塞，Crossref 足夠
- Blau PDF DataDome → 標 metadata-only，不宣稱 full-text
- Middelberg 2025 與 OE 量測不同軸 → ★☆☆ footnote 即止

## 與 critical-research-reviewer 搭配

跑完 Consensus 後，可對以下 claim 開 `standard` 審查：

1. 「Dual-mic profile = OE ground truth」
2. 「Objective dB improvement substitutes for subjective S-axis」
3. 「Open-fit vent solves sealed ANC occlusion without tradeoff」

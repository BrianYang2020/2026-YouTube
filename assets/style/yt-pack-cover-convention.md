# 頻道封面慣例（yt-pack 實際成品 — 以此為準）

> **來源真相（Source of Truth）**：`G:\我的雲端硬碟\2026第二大腦\yt-pack\<episode>\`
> 每一集實際上架用的封面（由全域 `yt-pack` 技能產出）都放在這裡，一集一資料夾（例：`opencode-ep06/`、`gpt-codex-ep06/`、`claude-basics-ep13/`）。
>
> **鐵則**：剪某一集時，**先去 yt-pack 找這一集有沒有現成封面**。
> - 有 → **直接沿用**（複製成本專案 `output/<標題>/cover.png`），不要另外生。挑最終版（通常是 `_v2`／`_v3`／有正確 hook 那張）。
> - 沒有 → 依下方慣例用 `cover-image`/`draw.py` 生一張**貼近此風格**的（不是舊版 `cover-style.md` 的深藍＋橘粒子風）。

## 觀察到的固定元素（跨集一致）

1. **背景**：**純黑／接近純黑**（`#000000`~`#0A0A0A`），高對比、乾淨、留白多。不是深海軍藍、不是粒子塞滿。
2. **人物**：**三師爸真人去背照**（viewsonic 專業形象照那張），放**畫面右側**，戴眼鏡、黑色外套；姿勢多樣（手托浮空 UI／手扶下巴／手勢），邊緣有主題色 rim light。
   - 注意：yt-pack 用的是**真人照合成**，比 gpt-image-2 純插畫更真實。若用 draw.py 生，仍以 `assets/persona/三師爸人物形象照.png` 為基準。
3. **系列標籤（左上）**：`OpenCode 基本功 EP06` / `GPT Codex 基本功 EP06` / `Claude 基本功 EP13`，白色粗體。
4. **主標題（左側，超大）**：白色或主題色，超粗黑體，佔畫面高度 1/3 以上，2–3 行。
5. **副標題（主標下方或標籤帶）**：中文一句話，白字或反白標籤帶。
6. **右上徽章**：圓形 `FREE 免費` / `全新改版` 等。
7. **右下標籤帶**：白底圓角，放英文小標（`OpenCode · Model Guide` / `Codex 教學進化全解析`）。
8. **主視覺**：手捧或旁置的**發光終端機視窗／產品 mockup**（CLI 指令、model chips、dashboard、像素遊戲…），發光色＝主題色。

## 主題強調色（會變的部分）

| 影片主角 / 系列 | 強調色 | 風格細節 |
|---|---|---|
| **Claude**（Claude 基本功 / Design）| **橘＋亮黃** | 橘色筆刷噴濺、星星裝飾、暖色 |
| **GPT Codex** | **電光藍** | 藍色閃電、grunge 藍框、科技冷冽 |
| **OpenCode / 多模型比較** | **綠（CLI 綠）** | 終端機 `>_` 提示字、model chips、極簡黑底 |
| 其他純教學 | 依內容挑一個高對比強調色 | 維持「黑底＋單一強調色」原則 |

> 原則：**黑底 + 白字 + 單一主題強調色**（不要多色亂搭）。終端機/CLI 元素是這個頻道的識別符號。

## 與舊 `cover-style.md` 的關係

`assets/style/cover-style.md`（深海軍藍＋橘＋放射粒子）是**早期風格**，已被上述 yt-pack 黑底極簡終端機風取代。
**日後生封面以本檔（yt-pack 慣例）為第一優先**；`cover-style.md` 僅供色票 Hex 參考。

## 現成封面對照（本次觀察樣本）
- `yt-pack/opencode-ep06/cover_opencode-ep06_v2_...png` — 黑底、綠強調、model chips（GPT-5/Claude/Gemini/Qwen/DeepSeek/**Kimi K3 selected ✓**）、綠色「最強模型 Kimi K3 正式登場」帶
- `yt-pack/gpt-codex-ep06/...` — 黑底、藍閃電、Codex v2 終端 mockup
- `yt-pack/claude-basics-ep13/...` — 黑底、橘筆刷、手托 dashboard＋像素遊戲

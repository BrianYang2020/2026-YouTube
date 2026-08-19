---
name: chatgpt-youtube-video-workflow
description: 在 ChatGPT App 中把原始長影片處理成可審核的 YouTube 上架包，包含口播去靜音、VFR 防黑畫面、Groq Whisper 字幕、疑慮術語確認、標題候選、內建生圖封面、描述與 SEO、長片打包，以及可選的亮點短片、燒錄字幕與 9:16 版本。當使用者說要在 ChatGPT App 剪影片、處理 raw 影片、跑完整 YouTube workflow、轉字幕、用 Groq、做封面／metadata／Shorts，或指定使用 chatgpt-youtube-video-workflow 時使用。
---

# ChatGPT YouTube 影片工作流

把本技能視為單一、可攜式工作流。只呼叫本技能 `scripts/` 內的剪輯與字幕腳本，不依賴旁邊的 Claude、Codex、smart-cut 或 audio-to-srt 技能。

## 路徑與專案契約

1. 將 `SKILL.md` 所在資料夾解析為 `<SKILL_DIR>`，不要假設技能一定放在專案內。
2. 將目前工作目錄解析為 `<PROJECT_ROOT>`。
3. 優先讀取 `<PROJECT_ROOT>/AGENTS.md`、`HANDOFF.md` 與主要 README；檔案不存在就跳過。
4. 使用以下預設資料夾；專案規則另有指定時以專案規則為準：
   - 輸入：`raw/<video-id>/`
   - 中間檔：`working/<video-id>/`
   - 交付：`output/<title> [ChatGPT]/`
5. 不把影片、音訊、API key 或其他敏感資料加入 Git。

首次安裝、分享與個人化時，先讀 [portable-setup.md](references/portable-setup.md)。

## 不可省略的原則

- 先剪片、再轉字幕，確保 SRT 時間碼對齊成片。
- 先用約 45 秒代表性片段試剪；除非使用者已提供參數或明確授權全自動跑完。
- Groq 會把音訊送到第三方服務。內容可能敏感時，先取得同意；不同意則停止上傳並建議本地 Whisper。
- 絕不顯示、回傳、寫入或提交 API key。只從 `GROQ_API_KEY` 或 `~/.groq_api_key` 讀取。
- 不自行修正不確定的人名、產品名、縮寫、數字或校名；列出字幕段號讓使用者確認。
- 不增刪字幕段落、不改時間碼。`validate_srt.py` 未通過就停止交付。
- 封面只用 ChatGPT App 內建生圖／圖片編輯能力，不呼叫 `cover-image/draw.py`，也不要求 OpenAI API key。
- 每次生人物封面都重新讀取原始人物基準照，不沿用舊封面或衍生圖片。
- 所有 PowerShell 路徑使用 `-LiteralPath` 或完整引號，避免空白與 `[ChatGPT]` 被當成萬用字元。

## 0. 執行前檢查

在 PowerShell 解析技能位置後執行：

```powershell
$SkillDir = (Resolve-Path ".\skills\chatgpt-youtube-video-workflow").Path
python "$SkillDir\scripts\preflight.py"
```

若技能裝在全域位置，直接以實際 `SKILL.md` 的父資料夾設定 `$SkillDir`。缺少 Python、ffmpeg、ffprobe 或 auto-editor 時，依輸出修正後再繼續。

## 1. 收件與試剪

1. 確認唯一輸入影片，建立 `working/<video-id>/` 與 `_subtitles/`。
2. 用 ffprobe 取得原始長度、解析度、幀率與音軌。
3. 從約全片三分之一處擷取 45 秒代表性樣本。
4. 以 `threshold=0.04`、`margin=0.25sec,0.25sec` 試剪，讓使用者聽節奏：
   - 剪到字：降到 `0.02`，margin 提到 `0.5sec,0.5sec`。
   - 太鬆：提高到 `0.05` 或 `0.06`。
5. 使用者確認後才用相同參數剪完整影片。

標準呼叫：

```powershell
python "$SkillDir\scripts\smart_cut.py" `
  "raw\<video-id>\source.mp4" `
  --out "working\<video-id>\<video-id>.cut.mp4" `
  --threshold 0.04 `
  --margin "0.25sec,0.25sec"
```

腳本會精確比對平均幀率與宣告幀率。只要不同就先在系統暫存區轉 CFR，再於本機暫存區完成 auto-editor，最後複製回目標路徑，降低 VFR 黑畫面與雲端硬碟寫入消失風險。

剪完後用 ffprobe 比較原長與新長，並抽查開頭、中段、尾段畫面及音訊。回報剪除比例。

## 2. Groq 字幕管線

將下列路徑代換成實際值後依序執行：

```powershell
$Work = "working\<video-id>"
$Cut = "$Work\<video-id>.cut.mp4"
$Sub = "$Work\_subtitles"

python "$SkillDir\scripts\transcribe_groq.py" $Cut --out "$Sub\raw.json"
python "$SkillDir\scripts\resegment.py" "$Sub\raw.json" --audio $Cut --out "$Sub\raw.srt"
python "$SkillDir\scripts\apply_vocab.py" "$Sub\raw.srt" --out "$Sub\vocab.srt"
python "$SkillDir\scripts\find_dubious_terms.py" "$Sub\vocab.srt" --out "$Sub\dubious-terms.md"
```

`transcribe_groq.py` 會在檔案超過 Groq 限制時，以 ffmpeg 產生 16 kHz、mono、32 kbps 暫存音訊；不改動原檔。

讀完整 `vocab.srt` 與 `dubious-terms.md`，只修正能從上下文確定的項目。把仍有疑慮的段落、原文和建議列給使用者後暫停。收到確認後套用修正：

```powershell
python "$SkillDir\scripts\finalize_subtitles.py" "$Sub\vocab.srt" `
  --out "$Sub\clean.srt" `
  --replace "舊字->新字" `
  --replace "390:AGE->Agent"

python "$SkillDir\scripts\validate_srt.py" --raw "$Sub\vocab.srt" --clean "$Sub\clean.srt"
python "$SkillDir\scripts\srt_to_txt.py" "$Sub\clean.srt" --out "$Work\<video-id>.txt"
Copy-Item -LiteralPath "$Sub\clean.srt" -Destination "$Work\<video-id>.srt" -Force
```

清字時讀 [cleanup-rules.md](references/cleanup-rules.md)；專有名詞參考 [vocabulary.md](references/vocabulary.md)。分享給別人前，務必依頻道內容改寫詞彙表，並檢查 `scripts/apply_vocab.py` 的預設替換。

## 3. 標題確認

1. 讀取清字後完整逐字稿。
2. 產生 10 個標題，混合痛點型、價值型、教學型、反問型與案例型。
3. 寫入 `working/<video-id>/titles.md`。
4. 把候選列給使用者並暫停，不自行選題。

使用者選定後，移除 Windows 不合法字元 `<>:"/\\|?*`，避免以空白或句點結尾，建立 `output/<title> [ChatGPT]/`。資料夾內檔名不加 `[ChatGPT]`。

## 4. 封面

1. 優先檢查專案是否已有同集封面；有且使用者未要求重做時沿用。
2. 若存在，依序讀取：
   - `assets/style/reference-thumbnails.png`
   - `assets/style/cover-style.md`
   - `assets/persona/` 內使用者指定的原始人物基準照
3. 呼叫 ChatGPT App 內建生圖／圖片編輯能力。人物基準照必須作為這次生成的直接參考圖。
4. 若缺人物照，生成不含真人的主題封面，或請使用者補圖；不要憑空聲稱像本人。
5. 輸出 16:9 `cover.png`。檢查文字、人物、主題色、裁切與可讀性；必要時重生。

本步驟不讀 `OPENAI_API_KEY`，也不執行任何 `draw.py`。

## 5. Metadata 與長片打包

先讀 [marketing-spec.md](references/marketing-spec.md)，依清字後逐字稿撰寫 `metadata.md`。至少包含：

- 選定標題與 10 個候選。
- YouTube 描述、三個內容重點與 CTA。
- 依專案需求產生的社群貼文。
- 15–20 個 SEO 關鍵字。
- 「YouTube 標籤欄位（直接複製）」半形逗號分隔版。
- 依 SRT 產生的建議章節時間碼。
- 上架前檢查表。

打包下列 5 個檔案：

```text
output/<title> [ChatGPT]/
├── <title>.mp4
├── <title>.srt
├── <title>.txt
├── cover.png
└── metadata.md
```

逐項驗證檔案可開啟、影片時長合理、SRT 時間碼通過、封面為 16:9、metadata 有可複製標籤。專案有 `HANDOFF.md` 時更新完成內容、輸出位置、待確認事項與下一步。

## 6. 可選短片流程

只有使用者要求 Shorts／短片／亮點時才執行。

1. 讀完整長片 SRT，從痛點、反問、數字承諾、強斷言、轉折與價值結算找亮點。
2. 組 A 痛點型、B 好奇型、C 承諾型三版。每版先以 60 秒內為目標，最多 120 秒；每個切點必須對齊字幕段落邊界。
3. 寫入 `working/<video-id>/shorts-candidates.md`，列出時間碼、三幕結構與預估長度，讓使用者選擇後再剪。
4. 切片與重編時間碼：

```powershell
python "$SkillDir\scripts\clip_cut.py" `
  --input-mp4 "$Work\<video-id>.cut.mp4" `
  --input-srt "$Work\<video-id>.srt" `
  --segments "00:00:08.500-00:00:13.200,00:00:45.100-00:01:30.800" `
  --out-dir "$Work\short-tmp"
```

5. 依需求加結尾字卡、燒字幕、轉 9:16：

```powershell
python "$SkillDir\scripts\add_end_card.py" --input-mp4 "$Work\short-tmp\short.mp4" --output-mp4 "$Work\short-tmp\short-with-card.mp4"
python "$SkillDir\scripts\burn_subtitles.py" "$Work\short-tmp\short-with-card.mp4" "$Work\short-tmp\short.srt" "$Work\short-tmp\short-subtitled.mp4"
python "$SkillDir\scripts\make_vertical.py" "$Work\short-tmp\short-subtitled.mp4" --out "$Work\short-tmp\short-9x16.mp4"
```

6. 提供 3 個短片標題讓使用者選擇，再用 `[ChatGPT] (Short)` 後綴打包。交付 16:9 乾淨版、16:9 字幕版、9:16 直式版、SRT、TXT、封面與短片 metadata 共 7 檔。

## 失敗處理

- 找不到 Groq key：停止在字幕步驟，指出支援的兩個安全位置，不要求使用者把 key 貼在對話中。
- Groq 413／檔案仍過大：切成有重疊的小段轉錄，再合併時間碼；不要降低到難以辨識的音質。
- VFR 或黑畫面：不要交付。重新轉 CFR 後剪輯並抽查多個時間點。
- Google Drive 寫入不穩：保留系統暫存區的成功產物，確認大小與時長後再複製回專案。
- 中文字幕燒錄失敗：檢查 ffmpeg 是否含 subtitles／drawtext filter 及字型路徑；仍失敗時交付外掛 SRT，不宣稱已燒錄。
- 使用者打斷或修正需求：立即停止目前階段，依最新指示續跑，不跨越確認關卡。

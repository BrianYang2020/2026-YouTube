---
name: claude-youtube-video-workflow
description: Claude Code 專用的 2026Youtube 總控工作流 Skill。當使用者要求 Claude Code 處理 raw 裡的新影片、一次跑完整 YouTube 生產線（剪口播→轉字幕→清字確認→長片打包→短片精華→雙格式行銷資訊）、剪口播、轉字幕、產標題、用 cover-image/draw.py 產封面、寫 metadata、剪 short、打包 output，或說「使用 claude-youtube-video-workflow」時使用。此 Skill 明確走 Claude Code / OpenAI API Key 封面路線，不使用 Codex 內建 Image2。
---

# claude-youtube-video-workflow：Claude Code 版 YouTube 生產總控

## 一條龍總覽（長片 → 短片 → 雙格式資訊）

本 Skill 一次完成三大階段，中間有 **3 個 STOP 關卡**，每關都必須**停下等使用者拍板，不准自動往下衝**：

```
階段 1 長片：剪口播 → 轉字幕 → 🛑STOP1 疑慮字幕確認 → 時間碼硬關卡 → 標題 → 封面 → 長片 metadata → 打包
階段 2 短片：讀完整字幕 → 亮點偵測 → 🛑STOP2 三候選確認 → 切片組片 → 短片 metadata → 打包
（標題候選為 🛑STOP3，已含在各階段內）
```

- **🛑 STOP 1**（階段 1 step 3）：疑慮術語列出來等使用者勘誤，套完才往下。
- **🛑 STOP 2**（階段 2）：短片 3 候選版本等使用者選編號。
- **🛑 STOP 3**（step 4 / 短片 step）：標題候選等使用者選。
- **硬關卡**：`validate_srt.py` 不通過（段數不一致 / 時間碼對不上）→ **中止交付並回報**，不得硬出。
- **防斷字**：字幕切句靠 `resegment.py` 切點優先序；短片切片一律對齊字幕段落邊界，**不准切到一句話中間**。

> 短片階段非必跑：使用者沒要短片就停在階段 1 收尾。要短片才接階段 2。

## 先讀

- `HANDOFF.md`
- `CLAUDE.md`
- `assets/style/cover-style.md`（唯一風格基準）
- `assets/style/reference-thumbnails.png`（若存在；目前還沒有）
- `assets/persona/人物形象照.png`

## Claude Code 專用規則

- 封面使用 `skills/cover-image/draw.py` 或 Claude 全域 `~/.claude/skills/draw/draw.py`。
- 需要 OpenAI API Key：`OPENAI_API_KEY` 或 `~/.openai.env`。
- 產封面必帶 `--edit assets/persona/人物形象照.png`。
- 每次封面都必須重新使用人物基準照，不得從上一張封面或衍生圖延續人物。
- 封面強調色依系列決定（AI 自動化＝萊姆綠 `#C6FF3D`／AI × 金融＝金色 `#F5B700`／AI 辦公＝亮橘 `#FF8A3D`），**不再依 AI 工具分橘／藍**。
- Claude 輸出資料夾固定加 `[Claude]` 後綴。

## 流程

1. **收件**
   - 找 `raw/` 的新影片。
   - 建立 `working/<video-id>/`。

2. **剪口播**
   - 讀 `skills/smart-cut/SKILL.md`。
   - 執行 `skills/smart-cut/scripts/smart_cut.py`。
   - **這位口播者實測甜蜜點**：`--threshold 0.06 --margin "0sec,0.1sec"`（剪掉約 80%、節奏俐落、開頭不留靜音）。
   - 起始建議：先用實測甜蜜點跑一次，給使用者看 `cut.mp4` 確認節奏；太狠 → threshold 0.05、margin "0.2sec,0.2sec"；太鬆 → threshold 0.07。
   - 輸出 `working/<video-id>/<video-id>.cut.mp4`。
   - 跑完回報「原長 X:XX → 新長 X:XX（剪掉 X%）」。

3. **轉字幕**
   - **可直接餵 cut.mp4 給 `transcribe_groq.py`**（內部自動用 ffmpeg 處理），不需先抽音訊。檔案 < 25MB Groq 直接吃；> 25MB 會自動降取樣。
   - 讀 `skills/audio-to-srt/SKILL.md`。
   - 流程：transcribe_groq.py → resegment.py → apply_vocab.py → **Claude 親自逐段清字**（讀 vocab.srt，依規則修錯字、加標點、不動時間碼、段數不變）→ **🛑 STOP 1 疑慮術語確認**（見下）→ validate_srt.py【硬關卡】→ srt_to_txt.py。
   - **先建立 `_subtitles/` 子資料夾**，否則 `resegment.py` 會 `FileNotFoundError`。
   - **🛑 STOP 1：疑慮術語確認（不准跳過）**
     1. 跑 `python skills/video-editing-and-subtitles/scripts/find_dubious_terms.py "working/<video-id>/_subtitles/<video-id>.vocab.srt" --out "working/<video-id>/_subtitles/dubious_terms.md"`，自動撈出專有名詞（Claude/Gemini/Agent…）與常見音譯錯字的段落。
     2. **Read `dubious_terms.md`，把有疑慮的段落列在對話中，停下問使用者怎麼改**，不要自己猜了就往下。
     3. 使用者回覆後，用 `python skills/video-editing-and-subtitles/scripts/finalize_subtitles.py "<vocab.srt>" --out "<clean.srt>" --replace "舊->新" --replace "段號:舊->新"` 套更正（特定段落限定用 `390:AGE->Agent` 格式，避免誤傷如 Gemini 中的 Gem）。
   - **時間碼硬關卡**：`validate_srt.py --raw <vocab.srt> --clean <clean.srt>` 若段數不一致或時間碼對不上 → **中止，回報差異，不得交付**。
   - **常見 Whisper 誤判**（清字時記得修）：
     - 「燭光國中 / 竹光國中」→「光武國中」
     - 「稅教材」→「數學教材」
     - 「AI-Aging / AI 阿真 / AI 安進」→「AI Agent」
     - 「租寫的範本」→「填寫的範本」
     - 「微填」→「微調」
     - 「半年段另存心」→「按年段另存新檔」
     - Cloud / Claude / Codex 易混（apply_vocab 已處理大部分）
   - 輸出 `working/<video-id>/<video-id>.srt` 與 `.txt`，並把 .srt 也複製出來方便 step 8 使用。

4. **產標題並暫停**
   - 讀清字後 `.txt`。
   - 產生 **10 個** 標題候選到 `working/<video-id>/titles.md`。
   - 標題策略要分組（讓使用者方便比較）：痛點/解放型、教學/Know-how 型、對比/反問型、具體/案例型 各 2–3 個。
   - 回報標題清單給使用者，**停下等使用者選編號**。不要自己挑、不要往下跑。

5. **建立 Claude 輸出資料夾**
   - 清洗標題中的 Windows 不合法字元：`？！：／＼?!:/\\<>|"*`。
   - 建立 `output/<清洗後標題> [Claude]/`。
   - 檔案本身不要加 `[Claude]`。

6. **產封面**
   - **生封面前 SOP（缺一不可、每次都要做）**：
     a. `Read assets/style/cover-style.md`（唯一風格基準：三系列配色、構圖、表情對照、prompt 範本）
     b. 若 `assets/style/reference-thumbnails.png` 存在才 Read（目前還沒有）
     c. 判斷系列（AI 自動化＝萊姆綠 `#C6FF3D`／AI × 金融＝金色 `#F5B700`／AI 辦公＝亮橘 `#FF8A3D`），依「觀眾得到的結果」分，不是依工具分
     d. 依標題情緒從 `cover-style.md` 第五節選人物照片，`Read` 該照片（每次重新讀；不可沿用舊封面）
   - **Prompt**：直接用 `cover-style.md` 第六節範本填空；主標兩行、每行 ≤6 字，第二行是結果數字。
   - 標準呼叫（`--edit` 換成上一步選定的照片）：
     ```powershell
     python skills\cover-image\draw.py "<prompt>" --edit "assets\persona\人物形象照.png" --size 1536x1024 --quality low --name cover --outdir "output\<標題> [Claude]"
     ```
   - 跑完整理：刪掉時間戳檔名版本，留 `cover.png` 一份；用 Read 工具看一眼封面確認人物樣貌正確。

7. **寫 metadata（依行銷規格）**
   - **先 Read `references/marketing-spec.md`**（txt v1.5 主規格 + 頻道補充欄位的兩套合併版），一律以它為準。
   - 吃**清字後 `.txt` 逐字稿**生成，素材要呼應字幕真實內容。長片 `metadata.md` 至少含：
     1. **10 個標題候選**（好奇/價值/痛點三風格穿插）
     2. **影片描述 ≤300 字**：Hook（前兩行吸睛）+ 3 個關鍵知識點列點 + CTA；第一段情緒呼應使用者選定的標題類型。
     3. **社群貼文**（FB / IG / Threads）：**嚴禁 Emoji**、每段 ≤3 行、長片 150–200 字、誠摯老師口吻、結尾留互動問題。
     4. **SEO 關鍵字 15–20 個** + **「全部一次貼」整合版標籤**（半形逗號分隔，使用者直接貼 YouTube 後台）。
     5. **章節時間碼**（依字幕段落，建議性質）。
     6. **上架前 checklist**（標題/封面/字幕/描述前 100 字含主關鍵字/標籤/播放清單/社群同步）。
   - 細節格式與短片覆寫規則完全以 `references/marketing-spec.md` 為準。

8. **打包與自我檢查**
   - 輸出資料夾應包含 **5 個檔案**（檔名不加 `[Claude]` 後綴）：
     - `<標題>.mp4`（從 working/ 複製過來改名）
     - `<標題>.srt`
     - `<標題>.txt`
     - `cover.png`
     - `metadata.md`
   - **自我檢查清單**（跑完後逐項確認）：
     - [ ] 資料夾名結尾有 ` [Claude]` 後綴
     - [ ] 資料夾內 5 個檔案齊全
     - [ ] 影片檔名等於 YouTube 標題（不含後綴）
     - [ ] cover.png 可開啟、人物樣貌延續基準照（用 Read 看一眼）
     - [ ] cover.png 只用一個強調色、且與系列對應；主標無錯字無多餘標點；人物沒有長出眼鏡
     - [ ] metadata.md 含「全部一次貼」整合版標籤段
     - [ ] HANDOFF.md 已更新本支影片狀態
   - **PowerShell 提醒**：路徑含 `[Claude]` 時 PowerShell 會把中括號當 wildcard，用 `-LiteralPath` 或加單引號。Bash 沒這問題。

9. **更新交班**
   - 更新 `HANDOFF.md`：完成項目、輸出位置、封面狀態、待審事項、下一步。

## 踩坑

- `OPENAI_API_KEY` 不存在或帳號未驗證時，封面會失敗。檢查順序：環境變數 → `~/.openai.env`。Bash 載入：`export $(cat ~/.openai.env | xargs)`。
- `skills/cover-image/draw.py` 需要 `openai` Python 套件。
- gpt-image-2 需 OpenAI Individual 驗證；未驗證會吐 `403 Organization must be verified`。
- `auto-editor` 不在 PATH 沒關係，`smart_cut.py` 會自動 fallback 到 `python -m auto_editor`。
- Groq transcription 會上傳音訊到第三方服務；若內容敏感先停下問使用者。
- Groq 25MB 上限：`transcribe_groq.py` 內建 ffmpeg 自動降取樣到 16kHz mono 32kbps。
- `resegment.py` 不會自動建輸出資料夾，先 `mkdir _subtitles`。
- 中文檔名、空白、`[Claude]` 中括號路徑：PowerShell 用 `-LiteralPath` 或單引號；Bash 直接雙引號即可。
- 封面 prompt 矛盾（例如同時說人物穿藍色但基準照是黑色外套）→ gpt-image-2 會妥協；prompt 中只描述位置與場景，不要重新描述五官/穿著屬性。
- **每次封面都要重新讀基準照**，不能拿前一張封面當輸入；模型會放大誤差，越生越不像。

## 階段 2：短片精華（要短片才跑）

長片跑完（已有 `cut.mp4` + `clean.srt`）後，若使用者要短片，**接手 `skills/short-video-workflow/SKILL.md` 完整流程**。重點關卡：

1. **強制完整讀 `working/<video-id>/<video-id>.srt`**：擷取精華前一定先看過整份字幕、理解時間碼，不准只看片段就抓。
2. **亮點偵測**：依痛點/反問/數字承諾/強斷言/轉折揭密等檢核表給每段 0–3 分。
3. **🛑 STOP 2：三候選版本確認**
   - 組 3 個版本（A 痛點型 / B 好奇型 / C 承諾型 hook），每版三幕結構、≤120s、寫進 `working/<video-id>/shorts-candidates.md`。
   - **列三版時間碼與三幕劇本給使用者，停下等使用者選 A/B/C**（或使用者自給時間碼）。不准自己挑。
4. **切片組片**：`clip_cut.py` 切片（**對齊字幕段落邊界，不切到一句話中間**）→ 確認 ≤120s → `add_end_card.py` 結尾字卡 → `burn_subtitles.py` 燒字幕 → `make_vertical.py` 產 9:16 直式版。
5. **短片標題（🛑 STOP 3）**：出 3 個更短更聳動的候選等使用者選。
6. **短片 metadata**：同樣 **Read `references/marketing-spec.md`**，套用其中「C. 短片差異」覆寫（標題 3 個、描述 ≤150 字、`#Shorts` 必備、標籤結尾加 `Shorts,短影片`）。

規格：16:9 剪輯、≤120s、封面強調色依系列、輸出 `output/<短片標題> [Claude] (Short)/`，封面/人物基準照沿用同一份。
**每支短片必加產 9:16 直式版**（YouTube Shorts 發布用）：燒字幕後跑 `make_vertical.py`（模糊背景填充、內容不裁切、1080×1920），輸出檔名 `<短片標題>_直式9x16.mp4`，短片資料夾共 7 檔。

## 範例：原作者第一支影片的成果參數（封面風格已不適用，僅供剪輯參數參考）
讓你知道實測 OK 的參數長相：
- 影片：`你還在手動填課程計畫嗎 AI Agent 教師工作流實演`
- 原長 14:00 → smart-cut（threshold 0.06、margin "0sec,0.1sec"）→ 1:42（剪 88%）
- 字幕 37 段、530 字、總時長 1:41
- 標題候選 10 個，分四組策略，使用者選 #7
- 封面：橘色主題、左側 Claude AI 介面 + 紅 X 紙本、右側人物指向左方
- 完整成品：`output/你還在手動填課程計畫嗎 AI Agent 教師工作流實演 [Claude]/`

# HANDOFF — 雙 AI 接力交班檯

> **每次工作前**先讀本檔最上面的「目前狀態」。**每次工作結束前**更新「目前狀態」與「下一步」。
> 寫的時候假設下一個接手者是陌生人。

---

## 目前狀態（最新）
- **更新時間**：2026-10-10
- **最後操作者**：Claude Code（Opus 5.5）
- **進度**：剛從 `mathruffian-dot/2026-YouTube` fork 成 `BrianYang2020/2026-YouTube`，**目前沒有進行中的影片**
  - 專案位置：`C:\Claud Code\Projects\YT Automate`（本機，不在任何雲端同步資料夾內）
  - 環境已備妥並驗證：Python 3.12.10、ffmpeg 9.0.2、auto-editor 29.3.1、groq、openai 套件
  - API key 已放在家目錄並實測連線成功：`~/.groq_api_key`（Whisper 可用）、`~/.openai.env`（gpt-image-2 可見；首次生圖若跳 403 需到 OpenAI 做 Individual 驗證）
  - 原作者的完整交班紀錄已移到 `docs/reference/upstream-handoff-mathruffian.md`，內含大量實戰經驗（smart-cut 參數、VFR 轉 CFR、字幕替換防「命中正確詞內部」等），**遇到問題可先查那份**
  - `output/` 內 52 個資料夾是原作者的交付成品，僅供參考
- **下一步**：
  1. ~~個人化人物照~~ ✅ 2026-10-10 完成：基準照 `assets/persona/人物形象照.png`（微笑），另有 `expressions/`／`poses/`／`scenes/`；照片不進 git，換電腦要從 `人物特徵.zip` 手動放回
  2. ~~個人化頻道風格~~ ✅ 2026-10-10 完成：`assets/style/cover-style.md` 重寫為「炭黑底 `#14161A`＋三系列強調色」（AI 自動化＝萊姆綠 `#C6FF3D`／AI × 金融＝金色 `#F5B700`／AI 辦公＝亮橘 `#FF8A3D`）；原作者的 `yt-pack-cover-convention.md`、`reference-thumbnails.png` 移到 `docs/reference/`。**此風格尚未實際生過封面**，第一次生圖時要檢查效果、必要時微調 prompt
  3. 個人化字幕詞彙表：`skills/audio-to-srt/references/vocabulary.md`、`scripts/apply_vocab.py`
  4. 丟第一支影片到 `raw/<影片代號>/原始.mp4` 試跑完整流程

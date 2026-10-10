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
  1. 個人化人物照：換掉 `assets/persona/三師爸人物形象照.png`，並同步改 `CLAUDE.md`／`AGENTS.md`／`assets/style/` 內的引用
  2. 個人化頻道風格：改寫 `assets/style/cover-style.md`、`assets/style/yt-pack-cover-convention.md`、`reference-thumbnails.png`
  3. 個人化字幕詞彙表：`skills/audio-to-srt/references/vocabulary.md`、`scripts/apply_vocab.py`
  4. 丟第一支影片到 `raw/<影片代號>/原始.mp4` 試跑完整流程

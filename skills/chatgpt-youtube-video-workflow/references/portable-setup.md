# 可攜安裝與分享規格

## 目錄

1. 安裝位置
2. 系統需求
3. Groq API key
4. 最小專案結構
5. 個人化
6. 分享前檢查

## 1. 安裝位置

分享時複製完整 `chatgpt-youtube-video-workflow` 資料夾，不要只複製 `SKILL.md`。

可放在任一專案的：

```text
<project>/skills/chatgpt-youtube-video-workflow/
```

也可安裝到使用者的 Codex／ChatGPT App 技能位置：

```text
C:\Users\<user>\.codex\skills\chatgpt-youtube-video-workflow\
```

全域安裝後，工作專案不必再放一份；技能會把目前工作目錄視為專案根目錄。

## 2. 系統需求

- Python 3.10 以上。
- ffmpeg 與 ffprobe，並加入 PATH。
- auto-editor：`python -m pip install auto-editor`。
- 可使用內建圖片生成／編輯能力的 ChatGPT App。
- Groq 帳號與 API key；只有字幕步驟需要。

此技能的 Groq 呼叫使用 Python 標準函式庫，不需要安裝 `groq` 或 `openai` 套件。

執行：

```powershell
python ".\skills\chatgpt-youtube-video-workflow\scripts\preflight.py"
```

## 3. Groq API key

支援兩種位置，優先順序如下：

1. 環境變數 `GROQ_API_KEY`。
2. 使用者家目錄的純文字檔 `~/.groq_api_key`。

不要把 key 放在技能資料夾、專案、Git、對話內容或螢幕截圖。建議以 Windows 的安全文字編輯器建立 `C:\Users\<user>\.groq_api_key`，整個檔案只放 key，並確認該檔不在同步硬碟內。

## 4. 最小專案結構

```text
project/
├── raw/
├── working/
├── output/
└── skills/
    └── chatgpt-youtube-video-workflow/
```

`AGENTS.md`、`HANDOFF.md`、`assets/style/` 與 `assets/persona/` 都是選用。存在時技能會讀取；不存在時仍可剪片、轉字幕與產生無真人封面。

## 5. 個人化

分享者應提醒接收者檢查以下項目：

- `references/vocabulary.md`：換成自己的頻道人名、產品名與常用術語。
- `scripts/apply_vocab.py` 的 `REPLACEMENTS`：移除「三師爸、小克、宋睿瑋」等示例個人規則，加入自己的常見誤聽。
- `references/marketing-spec.md`：調整貼文口吻、字數、Emoji 與平台規格。
- `assets/persona/`：放自己的原始人物基準照；每次封面都直接使用它。
- `assets/style/cover-style.md`：定義色票、構圖、字型與禁止事項。
- `assets/style/reference-thumbnails.png`：選放 9–12 張既有封面拼圖。

若不做個人化，剪輯與字幕仍可執行，但機械詞彙替換可能不符合頻道內容，人物封面也不會像頻道本人。

## 6. 分享前檢查

- 資料夾內沒有 `.groq_api_key`、`.env`、影片、音訊、逐字稿或人物私密照片。
- 沒有 `__pycache__`、`.pyc`、工作中間檔或輸出成品。
- `python -m py_compile` 可編譯全部腳本。
- `quick_validate.py` 驗證 SKILL.md 通過。
- 在乾淨測試專案執行 `preflight.py`，確認錯誤訊息清楚。
- 明確告知接收者：Groq 會收到音訊內容，ChatGPT 圖片生成會收到封面參考素材。

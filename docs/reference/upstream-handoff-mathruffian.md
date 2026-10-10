# HANDOFF — 雙 AI 接力交班檯

> **每次工作前**先讀本檔最上面的「目前狀態」。**每次工作結束前**更新「目前狀態」與「下一步」。
> 寫的時候假設下一個接手者是陌生人。

---

## 目前狀態（最新）
- **更新時間**：2026-08-16
- **最後操作者**：Claude Code（Opus 5）
- **進度**：**「Agent Skill 完全攻略」長片＋3 支指定主題短片全流程交付完成** ✅（使用者：字幕你全權決定、短片指定三個主題、一次做到好 → 全自動無 STOP）
  - 來源：Downloads `Agent Skill 完全攻略.mp4`（67:34 / 2.35GB / 1080p；**又是 VFR 漏判型** `avg=182436000/6081199≈30.0000049` ≠ `r=30/1` → 手動先轉真 CFR）→ 歸檔 `raw/agent-skill/原始.mp4`
  - smart-cut `0.04 / 0.25sec,0.25sec` → **67:34 → 24:10（剪 64.2%）**
    - ⚠️ **剪幅異常高，我有先驗證再往下走**：silencedetect 在 -30/-35/-40dB 都抓不到靜音（背景噪聲底高），無法用來判斷；改用**語速密度**驗證——轉錄後 5022 詞 / 24:10 ＝ **208 詞/分**，與前幾集（195–202 詞/分）相當，且逐字稿句子完整、語意連貫 → 判定原片本來就有大量 demo 等待的空白，**不是剪過頭**
    - 亮度抽樣 5 點 105–204（1100s 那點 105 是深色 IDE 畫面，非黑幀）
  - 字幕：Groq 5022 詞 / 156 段 → resegment 451 段 → apply_vocab 3 段 → **Claude 自行清字**（`working/askill/clean_subs.py`：全域 46 段＋段落覆寫 10 段，合計 56 段）→ validate 通過（451 段 / 24:09，時間碼零改動）
  - **本集詞彙更正重點（Agent Skill／Gems 主題沿用）**：
    - **⭐ Gem 被整批聽成「券」**（升級你的券／Skill 比券強大／把好幾個券合併…）→ 全部還原成 Gem。**這是新的一類錯字：中文同音字取代英文詞，日後看到突兀的中文名詞要回頭想是不是英文**
    - **AntiGravity**（Antigrality／Antegrality／Anti-Gravity／Antigravity）、**ChatGPT**（CheretGPT）、**Cloudflare**（Claude Flare，**又中一次，Claude 頻道特別容易誤判**）
    - **Padlet**（小寫 padlet 統一大寫）、**PadletAutoBoard**（Padlet AutoBoard）、**Gem2Skill**（Gen2Skill）、**Gems**（Jems）
    - **English Error Cipher**（English Aero Seifer，升級後產生的技能名）
    - **首字母**（手字母）、**生圖**（生徒）、**闖關題卡**（闖關提卡）、**提示詞備份**（提示史／提示時備份）、**備用區段**（備用版）、**Submit**（Sub it）、**repo**（report）、**畫面上就會出現**（因為上就會出現）、**停止支援**（停止資源）、**原生支援生成圖片**（原生、資源、生成圖片）
  - 長片標題 Claude 自訂：「**Agent Skill 完全攻略：Gems 不是被淘汰，是該升級成 Skill**」
  - 長片交付：`output/Agent Skill 完全攻略 Gems 不是被淘汰，是該升級成 Skill [Claude]/`（5 檔）
  - **封面自己生（yt-pack 無此集）**：參考 `yt-pack/live-skills-are-yours` 的「手捧發光 SKILL 六邊形」構圖，改成**黑底＋碎裂冷藍 `#4A9EFF` 的 GEM 菱形 → 金黃 `#FFC93C` 的 SKILL 六邊形**升級隱喻。四張封面文字全對、人物延續基準照
  - **短片 3 支（主題由使用者指定，標題與時間碼我定）**，各 7 檔含 9:16 直式：
    - ①「免費用戶也能備份 Gems｜Google 官方匯出三分鐘搞定」（93.2s；`00:03:14.280-03:32.420, 00:03:41.860-04:34.880, 00:05:14.720-05:33.700`）
    - ②「一個網址，把你所有的 Gems 升級成 Skill」（70.4s；`00:06:43.632-07:39.752, 00:08:31.252-08:42.472`）
    - ③「一句提示詞，AI 自動生出一整面教學 Padlet」（77.8s；`00:17:16.824-18:02.044, 00:19:21.056-19:42.936, 00:19:49.536-19:57.176`）
    - **⚠️ 這三支都超過 60 秒（70–93s），不會自動進 Shorts feed**，屬一般短影片。因為是使用者指定的教學型主題，**完整度優先於 Shorts 演算法**（前幾集自選主題時都刻意壓在 60 秒內，這次是刻意的取捨，已寫進各支 metadata）
  - **✅ ffmpeg 環境續佳**：字卡／燒字幕／直式三支全部一次過；三支切點都不需要修殘字（少見）
  - 內容重點（供日後剪同系列參考）：**Skill ＝ 一份寫給 Agent 看的使用說明書**（內容/流程/資源/輸出/觸發情境），會重複做的流程都能變技能。**Gems 停用傳聞**：系統訊息稱 Google 計畫 10/20 停止支援現有 Gems，尚無正式新聞稿；講者立場「不是淘汰，是升級」。**備份三條路**：①免費用戶直接問 Gemini ②**Google Takeout**（取消全選→只勾 Gemini→多種格式 HTML→新增至雲端硬碟→僅匯出一次→Zip；解壓後 `Gemini Gems Data` HTML 內含名稱/說明/系統提示詞/附加檔案）③付費用戶叫 **Spark** 打包。**升級流程**：把 `Gem2Skill` repo 網址貼給任何 Agent → 掃雲端硬碟列出 Gems → 選編號 → Agent 給能力加值選單 A/B/C → 打包成技能（**前提：先裝 Google 雲端硬碟電腦版**；本集用 Gemini 3.7 Flash 在 AntiGravity 跑）。實測把「偵錯密碼（英文）」升級成 `English Error Cipher`，產出五題合一的互動闖關 HTML，用 **Cloudflare** 免費發布。**要生圖的技能只能在原生支援生圖的 Agent（AntiGravity／ChatGPT）跑**。**技能怎麼叫**：打斜線（**裝完要重開 Agent**）或直接說需求；設定→客製化可看所有技能與 MCP（講者已裝 17 個技能＋Padlet MCP）。**分享**＝做成公開 GitHub repo，一個網址走遍所有 Agent。**PadletAutoBoard**：給主題或教材，自動上網找資料建整面 Padlet，**前三區段固定（參考資料/投票/留言板）＋依內容決定主題區段數＋最後留五個備用區段**，完成直接給 QR Code——這一連串正是 Gem 做不到的（連不到工具、不會上網）
  - 上架提醒：**說明欄要補四樣**——① `Gem2Skill` GitHub repo ② 英文偵錯密碼技能 repo ③ 五合一互動闖關網頁公開網址（Cloudflare）④ AntiGravity 基本功 EP04（兩個月前那集）；短片②的留言區也要放 `Gem2Skill` repo
- **下一步**：補齊上述四個連結即可上架；本集字幕無待確認詞

## 上一個狀態（2026-08-12，OpenCode EP07）
- **更新時間**：2026-08-12
- **最後操作者**：Claude Code（Opus 5）
- **進度**：**OpenCode 基本功 EP07（新版本完全體·最佳搭配設定）長片＋3 短片全流程交付完成** ✅（使用者：字幕你確認、三支短片都要剪、一次做到好 → 全自動無 STOP）
  - 來源：Downloads `Opencode 基本功EP07_新版本完全體_最佳搭配設定.mp4`（50:25 / 1.75GB / 1080p；**又是 VFR 漏判型** `avg=272286000/9076201≈29.9999967` ≠ `r=30/1` → 手動先轉真 CFR）→ 歸檔 `raw/opencode-ep07/原始.mp4`
  - smart-cut `0.04 / 0.25sec,0.25sec` → **50:25 → 41:05（剪 18.5%）**（本集講話密集，剪幅本來就低）；亮度抽樣 6 點 162–176，無黑幀
  - 字幕：Groq 7996 詞 / 422 段 → resegment 701 段 → apply_vocab 5 段 → **Claude 自行清字**（`working/oc07/clean_subs.py`：全域 130 段＋段落覆寫 21 段，合計 140 段）→ validate 通過（701 段 / 41:04，時間碼零改動）
  - **本集詞彙更正重點（OpenCode／模型系列沿用）**：
    - **DeepSeek**（本集被聽成 14 種：DeepSek／DeepSec／DeepSeg／DeepSig／DeepSick／DeepSeed／DeepSeket／DeepSeker／DeepSecret／DeepSecure／Deep Seek… 已用單一 alternation 規則、長變體排前面處理）
    - **Luna**（LUNA）、**SOL**（Sol）、**Opus**（Arbus／Apus／Ops）、**Sonnet**（SongLuna → Sonnet,Luna）
    - **⚠️ Groq 不是 Grok**：本集講的是**免費 API provider Groq**（片中「搜尋 GRO Q API Key」），全部統一成 Groq。**沿用 EP06 教訓：xAI 的 Grok 與 Groq 是兩回事，要看語境判斷**
    - **Go 方案**（購方案／GO 方案／Code方案）、**OpenCode Zen**（OpenCodexAM）、**Big Pickle**（BigPicco）、**MiMo V2.5**（Memo／MIMO）、**Nano Banana**（NanoBatana）
    - **智力分數**（自立的分數／字立分數／致力分數／換字力）、**生圖**（深圖／生徒）、**教師研習**（教師演習／教室演習）、**陸版**（路版／路板）、**折抵**（折底）、**平價版無敵**（評價版無底）、**生成式 AI**（深層式AI）、**鋼琴**（康廷）、**省得驗證答案**（聖代驗證答案）
    - **幾何題的 D 點**：Whisper 把 `D` 全部聽成「珠」（A珠／珠AC／A珠C…），已整批還原成 D
    - **段 78 是整段只有一個「D」的 8.5 秒空段**，與段 79 合起來才是 DeepSeek → 用段落覆寫拆成「DeepSeek」+「V4 Flash 新版本的」
    - **⚠️ 跨段坑（連兩集都中）**：DeepSeek 被切在段 519／520 變成 `DeepS` + `eek`，已用段落覆寫收回。**Agent／DeepSeek 這類長英文詞的全域替換，事後一定要掃「段尾英文字母＋下段開頭英文字母」**
  - **❓待使用者確認（3 處，已用最合理版本寫入，也記在 metadata.md §8）**：
    1. **思考強度層級**：片中同時講「Altra／ATRA High」與「X-High」，字幕**統一寫 X-High**；若 OpenCode UI 實際是 Ultra High 要整批改回
    2. **免費看圖技能名**：Whisper 聽成 `Image Vision Sider`、repo 名聽成 `Psyca`，依原音保留未動
    3. **Kimi 2.6**：片中「以前沒有 Luna 時用 Kimi 2.6」，未查證是否為 Kimi K2.6
  - 長片標題 Claude 自訂：「**OpenCode 基本功 EP07：新版本完全體，DeepSeek V4 Flash ＋ Luna 最佳搭配**」
  - 長片交付：`output/OpenCode 基本功 EP07 新版本完全體，DeepSeek V4 Flash ＋ Luna 最佳搭配 [Claude]/`（5 檔）
  - **封面自己生（yt-pack 無 EP07）**：**刻意複製 EP06 的 OpenCode 系列版型**（黑底＋白色超大 OpenCode ＋灰色集數＋螢光綠 `#35D94B` ＋模型卡片＋終端機視窗＋右下白色標籤帶＋右上白圓徽章），四張封面文字全對、人物延續基準照
  - **短片 3 支全做**（主題與標題我自選，各 7 檔含 9:16 直式；**三支都 < 60 秒 → 可進 Shorts feed**）：
    - A 承諾型「用時間換智力｜便宜模型想久一點就能追上頂級」（58.6s；`00:11:51.194-12:24.414, 00:14:47.520-15:09.874`）
    - B 痛點型「免費幫 DeepSeek 裝眼睛，連會考題的圖都讀得懂」（49.5s；`00:10:23.274-10:31.194, 00:17:45.834-18:13.894, 00:22:25.430-22:35.908`）
    - C 好奇型「別再說 OpenCode 弱｜同一個對話想換哪家模型都可以」（58.5s；`00:34:01.642-34:26.042, 00:34:29.162-34:43.042, 00:34:47.022-35:04.242`）
    - **切片小坑**：A、B 的第二／三幕開頭各殘留前一段的尾字（「上」「度」），起點各往後推 0.3 秒並修字幕文字才乾淨。**日後選切點要順手看一下該段第一個字是不是上一句的殘尾**
    - 短片 C 第一版封面標籤帶出現重複逗號「,，」，重生一版（prompt 明講「標籤帶內不要出現任何標點」）才乾淨
  - **✅ ffmpeg 環境續佳**：字卡／燒字幕／直式三支全部一次過
  - 內容重點（供日後剪同系列參考）：**舊版 OpenCode 不會自動升級，要重新到官網下載**。Go 方案每月 10 美金換 60 美金用量（週 30／五小時滾動）。新增兩個模型：**DeepSeek V4 Flash 重新訓練版（0731）**——純文字、看不到圖、100 萬上下文、一次呼叫約 0.0006–0.003 美金，需開「啟用部署在中國的模型」開關、走陸版伺服器；**GPT 5.6 Luna**——視覺模型、可讀圖片與 PDF、100 萬上下文、一次約 0.01–0.02 美金。兩者都標 **2X Usage ＝雙倍消耗**（各約 30 美金額度，**沿用 EP06 判讀**）。核心觀念 **「用時間換智力」**：思考強度開 Max 時兩者智力分數都 52，X-High 49、High 46；對照 Opus 5 為 60–61、Opus 4.8 Max 57、GPT 5.6 SOL Max 58–59，SOL 與 Luna 成本差 30 倍。兩個免費技能：Groq 免費 API 幫 DeepSeek 裝眼睛（實測讀 114 年會考數學第 5 題幾何圖形全對）、免費生圖再壓繁體中文（**認 IP，教師研習整間教室同一 Wi-Fi 會被擋**）。講者自己是接 OpenAI Image 2（一張約 0.3 元）。破除誤會：**OpenCode 能在同一對話切換各家模型、也能接本地模型**，Codex 只能用 GPT、Claude Code 只能用 Claude。順帶提到 MiMo V2.5 幾乎免費且有視覺、Kimi K3 太貴、預告可能出 Hermes 教學、基本功系列將轉教學應用
  - 上架提醒：**說明欄要補三個連結**——① OpenCode 邀請碼／優惠碼（雙方各得 5 美金折抵）② 免費看圖技能 GitHub repo ③ 免費生圖技能 GitHub repo；短片 B 留言區也要放看圖技能 repo
- **下一步**：使用者確認上述 3 處存疑詞後回改字幕；補齊三個連結即可上架

## 上一個狀態（2026-08-09，Google Spark ＋ GAS）
- **更新時間**：2026-08-09
- **最後操作者**：Claude Code（Opus 5）
- **進度**：**AI Agent 教學應用「Google Spark ＋ GAS」長片＋3 短片全流程交付完成** ✅（使用者：字幕你決定、三支短片你決定、全部一次做到好 → 全自動無 STOP）
  - 來源：Downloads `Google Spark_ GAS 教學神搭配.mp4`（60:07 / 2.09GB / 1080p；**VFR 漏判型**：`avg=324699000/10823299≈30.0000027` ≠ `r=30/1`，同 EP04／EP07 那種「差一點就是 30」→ **手動先轉真 CFR** 再剪）→ 歸檔 `raw/google-spark-gas/原始.mp4`
  - smart-cut `threshold 0.04 / margin 0.25sec,0.25sec`（輸出 C:\temp 再搬回）→ **60:07 → 40:38（剪 32.4%）**；亮度抽樣 6 點 190–203，全程無黑幀（CFR 轉檔有效）
  - 字幕：Groq 8185 詞 / 273 段 → resegment 813 段（孤兒 0）→ apply_vocab 9 段 → **Claude 自行清字**（自寫大小寫敏感腳本 `working/gspark/clean_subs.py`：全域規則 110 段＋段落覆寫 83 段，合計 163 段）→ validate 通過（813 段 / 40:35，時間碼零改動）→ 未刪段
  - **本集詞彙更正重點（Google Spark／GAS 系列沿用）**：
    - **GAS**（GaS／CaS／GoS／Gs／GS → GAS）、**Google Apps Script**（Google App Scripts）
    - **clasp**（CLSP／CoASP／Clasp → clasp，Apps Script 的 CLI 工具）
    - **AI Agent**（AIAGEN／AGEN／Aging／agin／A群／A君／小寫 agent → Agent）
    - **AntiGravity**（Antigrity／Antiquity／Antigrate／Antigrating／Antigratis／Anti-Gratis，**含拼對但大小寫錯的 Antigravity**）
    - **Google Sheet**（Google Sites／Google Ship／四傳表 → Google Sheet／試算表）、**Google Slide**（Google Slead）
    - **NotebookLM**（Notebook M）、**Google Workspace**（work specs）、**Gem**（GEM，Gemini 的自訂 Gem）、**Gemini**（Geminar）、**Typeless**（Tyblest）、**SOL／Luna**（ChatGPT App 模型名）
    - **排程**（排成／排行）、**輪詢**（輪巡／輪循）、**行事曆**（行司力／行事力／行事例）、**雲端硬碟**（雲端硬點／雲噸硬碟／雲端一點）、**電腦版**（電腦板）、**登入**（登錄）、**寄信／寄 Mail**（記信／記Mail）、**垃圾信**（垃圾心）、**把關**（打官）、**串接**（創接）、**子資料夾**（紙資料夾）、**堪用**（勘用）、**三思**（刪失）、**delay**（屌勒）、**凱西老師**（凱希）
    - 段 564–566 的 `G1號／G3號…` → **9月1號／9月3號…**（Whisper 把「九」聽成 G）
    - **⚠️ 跨段坑再現**：`A群` 被切在段 365／366，全域替換後變成 `Age` + `nt`，已用段落覆寫收回。**日後 Agent 類替換一律回頭掃「段尾是英文字母、下段開頭也是英文字母」的組合。**
  - **❓待使用者確認（3 處，我已用最合理版本先寫入）**：
    1. 段 626「會整美是Agent」→ 暫寫「**彙整每日 AI Agent**」（推測是他念出第二個排程的名稱）
    2. 段 219「公開時」→ 暫寫「**工作區**」
    3. 段 269 的資料夾名 `Pretest`／`RDQ` 依原音保留，未查證實際拼法
  - 長片標題 Claude 自訂：「**AI Agent 教學應用：Google Spark ＋ GAS，教師自動化的雲端神搭配**」
  - 長片交付：`output/AI Agent 教學應用 Google Spark ＋ GAS，教師自動化的雲端神搭配 [Claude]/`（5 檔）
  - **封面本集自己生（yt-pack 無現成）**：依 yt-pack 慣例 draw.py 生 **黑底＋電光青綠 `#14E0C8`**（AI Agent 教學應用系列色），長片為雲朵→試算表／日曆／信封／雲端硬碟四節點＋GAS 程式碼終端機。四張封面文字全部正確、人物延續基準照
  - **短片 3 支全做**（主題與標題我自選，各 **7 檔**含 9:16 直式；**三支都 < 60 秒 → 可進 Shorts feed**）：
    - A 對比型「電腦關機也能工作｜Google Spark 是住在雲端的 AI Agent」（37.6s；`00:19:01.880-19:11.513, 00:19:18.913-19:27.073, 00:19:51.713-20:08.553`）
    - B 好奇型「每天早上七點，AI 自動幫我整理好行事曆跟 Gmail」（56.8s；`00:28:35.226-29:00.166, 00:29:12.066-29:40.946`）
    - C 痛點型「寫 GAS 不用再複製貼上｜clasp 讓 AI 直接改你的程式」（45.9s；`00:32:31.239-32:58.719, 00:34:59.259-35:14.659`）
  - **✅ ffmpeg 環境續佳**：`add_end_card.py`（drawtext）、`burn_subtitles.py`（中文燒字幕）、`make_vertical.py`（1080×1920）三支全部一次過，抽幀驗證字幕與「詳細影片請看留言」字卡正常。EP06/EP14 那套 PIL 繞道仍不需要
  - 內容重點（供日後剪同系列參考）：Google Spark ＝ Gemini 側邊欄切換、需 **Google Pro**、目前 Beta 且天天在改。前置要在「個人化智慧服務」把 Google 生態系連起來（＝雲端資料幾乎沒有隱私，講者有明確提醒）＋裝雲端硬碟電腦版。**Gem 做聊天機器人、Spark 做排程與處理檔案，兩兄弟互補不重合**。能做：讀寫雲端硬碟、搬檔整理、上網搜尋（比 Gemini Deep Research 淺）、生成原生 Document／Slide／Sheet、串 Gmail 與 Calendar、**排程與輪詢（最短一小時，不支援 30 秒）**、Google Chat。不能做：寫 GAS、連 NotebookLM、碰本地檔案、改／生成 Office 三件套（能讀不能改）；一個對話＝一個工作，沒有專案概念。**搭配法**：本地 Agent（本集示範 AntiGravity）＋ **clasp** 直接寫進 Apps Script，省掉複製貼上；授權與部署仍須手動。學生作答存 Google Sheet → Spark 定時輪詢處理。翻車實況：AntiGravity 授權後噴 Type Error，講者刻意保留並直言穩定度輸 Claude Code 與 Codex
  - 上架提醒：**說明欄務必補 clasp 連接 GAS 的 GitHub repo 連結**（片中承諾「根據我提供的 repo 來連接你的 GAS」）；短片 C 留言區也要放同一個連結；三支短片留言置頂長片連結
- **下一步**：使用者確認上述 3 處存疑字幕後回改（長片 srt/txt 與短片皆需同步）；補齊 repo 連結後即可上架

## 上一個狀態（2026-08-02，Wordwall）
- **更新時間**：2026-08-02
- **最後操作者**：Claude Code（Opus 5）
- **進度**：**開新系列「AI Agent 教學應用」**，首集封面＋文案素材完成 ✅（影片尚未錄，這集是直播）
  - 本集標題：`一份教材生出 30 種遊戲｜AI Agent 全自動操作 Wordwall`
  - 交付：`output/一份教材生出 30 種遊戲 AI Agent 全自動操作 Wordwall [Claude]/`（`cover.png` + `metadata.md`）
  - **新系列規則（重要，之後每一集沿用）**：
    - 系列名 **「AI Agent 教學應用」**，**不編集數**，單集靠標題／主題自己成立（使用者明確指示）
    - 系列主色 **電光青綠 `#14E0C8`**（避開 Claude 橘／Codex 藍／OpenCode 綠／AI Agent 基本功紫）
    - 其餘沿用 yt-pack 慣例：純黑底、左上 LIVE 標＋系列標、超大主標、右上圓形徽章、右下標籤帶、右側三師爸真人去背
  - 封面所有版本在 `2026第二大腦/yt-pack/live-wordwall-agent/`：`v10_final`（採用，Wordwall 節點最大）、`v8`、`v6`、`v9/v7/v4` 為無 logo 底圖
  - **Wordwall logo 處理法（可重用）**：官方原檔 `app.cdn.wordwall.net/.../logo-large...png` → 深灰字轉白、圖示鏤空處補白底，才能貼在黑底上；用 PIL 合成到生圖時預留的黑色空節點，不要讓 gpt-image-2 自己畫 logo
  - 「30 種遊戲」數字出處：Wordwall 官網首頁自述 30+ activity types
- **2026-08-02 稍晚：本集長片＋3 短片全流程交付完成** ✅
  - 來源：Downloads `AI Agent 教學應用_一份教材生成 30 種遊戲_用 Agent 來操控 Wordwall.mp4`（78:09 / 2.7GB / **真 CFR 30fps，不用轉檔**）→ `raw/aiagent-wordwall/原始.mp4`
  - smart-cut `threshold 0.04 / margin 0.25sec,0.25sec` → **78:09 → 44:53（剪 42.6%）**；先送 45 秒樣本（20:00 起）給使用者聽，樣本僅剪 8.7%，力道安全；亮度抽樣 4 點 52–215，無黑幀
  - 字幕：Groq 9403 詞 / 167 段 → resegment 1050 段 → apply_vocab 8 段 → **Claude 自行清字（使用者授權「字幕你來確認就好」）**：全域規則 229 段＋段落覆寫 6 段 → validate 通過（1050 段 / 44:49，時間碼零改動）
  - **本集詞彙更正重點（Wordwall 系列沿用）**：
    - **Wordwall**（WOWWO／WOWWOW／WOWOW／WOWO／WOW／WOWER／窩窩／渦渦／WorldWord／World War → Wordwall）、**Wordwall CLI**（WOWCLA／WOWCLT／WOWC-I／WOWClaude）
    - **AI Agent**（AIAGEN／AIGEN／AI Aging／AGEN／Agin／A君／A群 → Agent）
    - **ChatGPT**（TrackGPT／CheckGPT／ChadGPT／TrashGPT／缺GPT）、**AntiGravity**（Antigrality）
    - **教學平台**：Padlet（Petlet）、LoiLoNote（Loyal Note／Royd Note）、因材網（鷹材網）、TronClass（Tron School）、ClassDojo（Claude Dojo）、Wayground（WayGang／微廣）
    - **Level 1/2/3**（Label／LV）、**題型**（提醒）、**題數**（提速）、**考卷**（成券／成卷）、**因數倍數／因數分解／最大公因數／最小公倍數**（音速背書／紫音速分解／最大功音速）
    - Playwright（Playweight）、Chrome（crone）、省 Token（Sentoken）、訂閱制（訂閱字）、輪詢（輪巡）、登入（登錄）
    - **⚠️ 仍待使用者確認的 3 個詞**：模型名 **Luna / Luna Max**（原音 Duna／LUNA，我統一成 Luna）、**「選回收」**（疑為某個模式名，未動）、**Thinking 中**（原「相當於手的中」）
  - 三短片（使用者指定主題，未走三候選流程）：
    - Google Spark 的殺手級應用 輪詢 排程 Webhook（31:09.3–32:24.6，75s）
    - Wordwall 八大題型一次看懂 全部自動出題（17:31.06–18:52.6，82s）
    - Level 3 AI 生圖出題 考卷做不到的題目長這樣（34:38.56–35:58.7，80s）
    - 每支七檔齊全（16:9 乾淨／16:9 字幕版／9:16 直式／srt／txt／cover／metadata）
- **下一步**：使用者確認上述 3 個待確認詞彙後回改字幕；長片說明欄記得補 GitHub repo 連結（影片中承諾要放）

## 上一個狀態（2026-08-02）
- **最後操作者**：ChatGPT App
- **進度**：已建立可獨立分享的 ChatGPT App 影片剪輯總控技能 ✅
  - 位置：`skills/chatgpt-youtube-video-workflow/`
  - 單一資料夾內含：`SKILL.md`、ChatGPT UI metadata、13 支剪輯／字幕／短片腳本、字幕／行銷／可攜安裝參考文件。
  - 封面走 ChatGPT App 內建生圖，不讀 `OPENAI_API_KEY`；Groq key 只讀 `GROQ_API_KEY` 或 `~/.groq_api_key`，技能內沒有 key。
  - ChatGPT 版輸出後綴：長片 `[ChatGPT]`、短片 `[ChatGPT] (Short)`。
  - 已修正：極小幀率差的 VFR 漏判；auto-editor 在本機暫存區完成後才複製回 Google Drive；Windows 字卡字型路徑警告。
  - 驗證：skill validator 通過、13 支 Python 腳本語法通過、preflight 全通過、實際用 `working/codex-flow-test/sample.cut.mp4` 跑 smart-cut 成功，測試輸出已刪除。
- **下一步**：後續收到新影片時，直接指定「使用 chatgpt-youtube-video-workflow 處理 raw/<影片代號>」，先做 45 秒試剪再進字幕流程。

## 上一個剪輯狀態（2026-07-26）
- **更新時間**：2026-07-26
- **最後操作者**：Claude Code（Opus 5）
- **進度**：**AI Agent 基本功 EP07（需求探索四象限法／RDQ）長片＋3 短片全流程交付完成** ✅（使用者：字幕你幫我確認、三支短片都要剪）
  - 來源：Downloads `AI agent基本功EP07_需求探索四象限法_釐清專案需求的最後一塊拼圖.mp4`（原長 57:27 / 1.86GB；**VFR**（avg=310245000/10341499≈29.99999，屬 EP04 那種「差一點就是 30」漏判型）→ **手動先轉真 CFR** 再剪）→ `raw/aiagent-ep07/`
  - **⚠️【重要】smart-cut 參數本集大改（使用者連退兩次）**：
    - v1 慣用甜蜜點 `0.06 / 0sec,0.1sec` → 21:37（剪 62.4%）→ 使用者：「聲音切得太多，很多字、語音都被切掉」
    - v2 `0.04 / 0.25sec,0.25sec` → 36:36（剪 36.3%）→ 使用者：「還是剪太狠」
    - **v3 `0.02 / 0.5sec,0.5sec` → 43:49（剪 23.7%）→ 使用者 OK** ✅
    - **教訓**：所謂「實測甜蜜點 0.06/0sec,0.1sec」對這位口播者**不是通用值**；本集講話語速較慢、句中氣音多。日後新片**先跑一版 0.04/0.25 給使用者聽樣本**（切 45 秒樣本用 SendUserFile 送出最快），確認後才轉字幕，別直接 0.06 一路衝。
    - 亮度抽樣 7 點 39–206，無黑畫面（CFR 轉檔有效）
  - 字幕：Groq 8599 詞 → resegment 899 段 → apply_vocab 13 段 → **自行清字（使用者授權）**：段落覆寫 42 段＋跨段全域規則 131 段 → validate 通過（899 段、43:48）→ 未刪段
  - **本集詞彙更正重點（AI Agent／RDQ 系列沿用）**：
    - **AI Agent**（AI Agents／AI Aging／AI群／A群／Agin／AI君／AIG／的A集／AI域 → Agent；小寫 agent → Agent）
    - **四象限**（四項線／四項限／四個項限／四個向線／視像線／第一項目 → 四象限；「需求探索視像線」→「需求探索四象限」）
    - **Cloudflare**（ClaudeFlare／Claude Flare → Cloudflare，**Claude 頻道容易誤判成 Claude 家族產品，注意**）
    - **ChatGPT**（TradeGPT／ChackGPT／ChartGPT／CheckGPT／ChagPT → ChatGPT；TradeGPTAppSkill → ChatGPT App Skill）、**ChatGPT Site**（GPT-Site／ChatGP site，含跨段 `GPT`+`Site`）
    - **chezmoi update**（ChaseModeUpdate）、**RDQ Skill**（RDQSkill／RDQSKILL／RDQSkills／RDQSKID／RTQSkills）
    - **雞同鴨講**（機同壓腳）、**已知未知**（以知為知）、**教師研習/教師的程度**（教室演習/教室的程度）、**少走彎路/少花 Token**（掃走彎路/掃花token）
    - **原則上**（院上／原本上／越來越上，⚠️「因為上次這樣啦」不可套規則，已用段落覆寫）
    - 其他：GitHub Report→GitHub repo、Superbase→Supabase、Open Code→OpenCode、Antigrate/Antigrity→AntiGravity、生本→身分、啟動PGuy→啟動批改、使用模型→使用規模、貿用→冒用、班級做好→班級座號、免登錄→免登入、一戰是全包/一戰四搞定→一站式、題杜→題庫、四潮案→草案、椅子三小時→已知三小時
    - Whisper 重複亂碼已清：「律師」連跳 4 段（781–784）、「第一次看到我頻道影片」×3（87）
  - **❓待使用者確認（唯一未解）**：片中提到「前陣子很紅、會問非常多問題、太煩人」的既有需求釐清專案，Whisper 給 `Greemme`／`GreenMe` 兩種拼法，**字幕統一暫寫 `GreenMe`**（段 184／186／396／397／606）。使用者確認正確拼法後，長片與短片 B 字幕要一併替換。
  - 長片標題 Claude 自訂：「AI Agent 基本功 EP07：需求探索四象限法，釐清專案需求的最後一塊拼圖」
  - 長片交付：`output/AI Agent 基本功 EP07 需求探索四象限法，釐清專案需求的最後一塊拼圖 [Claude]/`（5 檔；**封面沿用 yt-pack `ai-agent-ep07` v3**——黑底霓虹紫＋四象限拼圖，AI Agent 系列色）
  - **短片 3 支全做，各 7 檔（含 9:16 直式）；三支都壓在 60 秒內 → 可進 Shorts feed**：
    - A 痛點型「別再跟 AI 雞同鴨講 需求先問清楚，少走很多彎路」（57.6s；`00:05:21.680-05:56.900, 00:36:17.594-36:28.914, 00:36:51.414-36:59.474`）封面黑底紫
    - B 好奇型「提示詞完整不等於需求完整，你少講了兩個象限」（57.7s；`00:23:49.236-24:07.576, 00:26:46.396-27:01.276, 00:27:52.676-28:04.056, 00:28:28.236-28:38.356`）封面黑底紫＋四宮格
    - C 工具型「ChatGPT Site 內建資料庫，不用再接 Firebase」（58.7s；`00:16:42.338-17:06.158, 00:19:46.938-20:12.358, 00:22:40.516-22:46.996`）封面黑底**電光藍＋黃字**（ChatGPT 主題）
  - **✅ ffmpeg 環境好消息（本次實測）**：`add_end_card.py` 的 **drawtext 已不再 segfault**（抽幀確認字卡「詳細影片請看留言」正常）、`burn_subtitles.py` 中文字幕也直接燒得起來、`make_vertical.py` 三支都 1080×1920 過。**先前 EP06/EP14 那套 PIL 字卡＋fonts.conf 手工繞道本次不需要**，日後可先直接試官方腳本。
  - 內容重點（供日後剪同系列參考）：RDQ＝需求探索四象限法。四象限＝①你知道你知道的（明確提供）②你知道你不知道的（AI 澄清）③你不知道你知道的（習以為常的細節，靠訪談挖）④你不知道你不知道的（靠內建通用題庫探索風險/備案/個資/著作權）。RDQ Skill 兩版（Claude Code／OpenCode／AntiGravity 共用 ＋ ChatGPT App 專版），裝好說「用 RDQ 問我」，**最多問三輪**，產出一頁草案規格卡。內建**六大題庫**（前五通用、最後一類教師場景，非教師請 Agent 置換）。**彩蛋：ChatGPT Site 已內建 Cloudflare D1／R2 後端資料庫**，一行提示詞開啟、額度內免費，不用再接 Firebase／Supabase。本集為基本功系列最後一集，下週起轉「AI 教學應用」。直播中主機因 GPT 跑程式卡死，改用筆電（`chezmoi update` 一鍵同步技能）續講。
  - **【追加】四象限教學片段**（使用者指定「講四個象限（知道跟不知道）的那一段抽出來」）：
    - `output/需求探索四象限法完整解說 知道與不知道的四種組合 [Claude] (教學片段)/`（**6 檔**：乾淨 16:9／字幕燒錄版／srt／txt／cover／metadata；使用者追加要求補封面與 metadata，**教學片段/導覽片不加結尾字卡**，故為 6 檔不是 7 檔）
    - 7:21.7；時間碼 `00:23:19.336-00:28:38.356`（四象限定義，起點＝「好,來解釋一下這 RDQ Skill 是什麼東西」）＋ `00:28:41.876-00:30:44.596`（四種已知對應 AI 四種動作）
    - 中間刻意剪掉段 576–577「你理解這個概念了,幫我在聊天室打個二」直播互動
  - **【追加】頻道導覽片段**（使用者指定「最開頭教大家怎麼使用三師爸頻道的那一段」）：
    - `output/第一次來三師爸頻道 該從哪裡開始看才不會迷路 [Claude] (頻道導覽)/`（**6 檔**含 cover／metadata，3:01.8）
    - 時間碼 `00:00:35.880-00:03:13.700`（導覽本體：別看首頁要看播放清單→AI Agent 基本功七集→再依慣用 AI 選 Claude Code／GPT／AntiGravity／OpenCode 播放清單）＋ `00:04:20.460-00:04:44.460`（收尾呼應：很多老師說內容太難→照索引從播放清單開始看）
    - 中間剪掉段 60–80（今天是最後一集、接下來播進階應用的系列預告）
    - **此片可重複利用**：內容不綁本集，日後可當頻道置頂導覽或新片開頭素材
  - **⚠️ 事後修正（同日，剪頻道導覽時發現）**：①`Antigravity` 有 2 處沒統一成 **AntiGravity**（清字腳本只擋了 Antigrate／Antigrity 兩種變體，**漏了拼字正確但大小寫錯的原詞**——日後規則要一併涵蓋「拼對但大小寫錯」）；②段 87 是 Whisper 重複亂碼，原本覆寫成單句仍與段 86 重複，已改成把段 86 的句子拆到 87（只用原話）。**踩坑：validate_srt 不接受空白段**，所以不能把重複段清空，必須改寫。長片 srt/txt 已回寫交付資料夾；三支短片與四象限教學片段不含這幾段、不受影響。
  - 上架提醒：說明欄務必補 **RDQ Skill 三個 GitHub repo 連結**（片中承諾；片中只提到兩版，OpenCode 版是直播後才建的，說明欄建議加註「OpenCode 使用者請改用 OpenCode 版」）＋ 專案初始化/開工/收工那集 ＋ EP01–EP06 播放清單；短片留言區置頂長片連結。
    - Claude Code 版：https://github.com/mathruffian-dot/rdq-skill
    - ChatGPT App 版：https://github.com/mathruffian-dot/rdq-skill-chatgpt-app
    - OpenCode 版：https://github.com/mathruffian-dot/rdq-skill-opencode
    - 註：另有 `mathruffian-dot/rdq-method` 為 **private**，不可貼說明欄（觀眾會 404）
- **前一支（2026-07-21）**：**Claude Code 基本功 EP14（Remote Control 完全解放·跨裝置協作）長片＋3 短片全流程交付完成** ✅（使用者：字幕我確認、3 短片我決定、一次做到完；全自動無 STOP）
  - 來源：Downloads `Claude code 基本功EP14_Remote Control 完全解放手機_電腦_筆電同步協作專案.mp4`（原長 26:00 / 905MB；**⚠️ VFR**（avg≠r_frame_rate）→ smart_cut 自動先轉 CFR）→ `raw/claude-ep14/`
  - smart-cut（0.06 / `0sec,0.1sec`）→ 15:30（剪 40.4%）；**VFR 片特別做密集亮度抽樣（60/200/400/600/800/870/900/925s）＝123–204，全程無黑畫面，轉檔成功**
  - 字幕：Groq 3389 詞 → resegment 351 段 → apply_vocab → 自行清字 → finalize 套 21 段 → validate 通過（351 段、15:29）
  - **本集詞彙更正重點**：**Groq Code→Claude Code**（關鍵誤聽）、**Aniable→Enable**（Enable Remote Control by Default）、**Remove Control→Remote Control**、桌面板→桌面版、栽要→摘要、TrackGPT→ChatGPT、A群→Agent、AI Edge→AI Agent、Station→Session、雲端碟→雲端硬碟、上於→上雲、原來上→原則上
  - 長片標題 Claude 自訂：「Claude Code 基本功 EP14：Remote Control 完全解放，手機、筆電、電腦同步協作」
  - 長片交付：`output/Claude Code 基本功 EP14 Remote Control 完全解放，手機、筆電、電腦同步協作 [Claude]/`（5 檔；**封面沿用 yt-pack `claude-basics-ep14`**，黑底橘色·手機/筆電/電腦雲端同步）
  - **短片 3 支全做**（16:9，自選主題＋標題）：
    - A「Claude Code Remote Control 怎麼開？兩層開關一次搞懂」（62.3s）
    - B「用手機遙控家裡電腦，對話完全同步」（68.7s）
    - C「電腦不關機，你在哪都能繼續做同一個專案」（48.5s）
    - 交付：各 6 檔；封面依慣例 draw.py 生**黑底霓虹橘**（Claude 系列色）。未做 9:16（未要求）。
  - **踩坑（新）**：一次連續生 3 張封面會超過 Bash 2 分鐘 timeout（第 3 張未完成）→ 已單獨補生。**日後 3 張封面建議分批或加大 timeout。**
  - 內容重點（供日後剪同系列參考）：Remote Control **兩層開關**（Settings 的 Enable Remote Control by Default ＋ 每個 Session 再開，藍色＝已開）；手機 App / 網頁版 Claude / 另一台電腦都能連回主機同一對話，**運算與檔案都在主機**；配雲端硬碟取用成果；前提是主機不關機。片中預告 **RDQ Method（需求探索）** 會另開一集。
- **前一支（2026-07-20）**：**ChatGPT App 基本功 EP07（一份 YAML 指揮 6 個 AI 同時做簡報）長片＋3 短片全流程交付完成** ✅（使用者：字幕我決定、3 短片我決定、直接做到好；全自動無 STOP）
  - 來源：Downloads `ChatGPT App 基本功 EP07_一份 YAML_指揮 6 個 AI 同時做簡報.mp4`（原長 78:01 / 2.71GB；**CFR** 30/1、1080p，免轉檔）→ `raw/chatgpt-ep07/`
  - smart-cut（0.06 / `0sec,0.1sec`，C:\temp 再搬回）→ 36:39（**剪 53%**，demo 等待生圖多屬正常）；亮度 93–202（深色 IDE/終端畫面偏暗、非黑）
  - 字幕：Groq 8427 詞 → resegment 810 段 → apply_vocab → 自行清字 → finalize 套 151+1 段 → validate 通過（810 段、36:38）
  - **本集詞彙更正重點（ChatGPT App/簡報系列沿用）**：
    - **ChatGPT**（Claude GPT／TrackGPT／TransgPT／CheckGPT／ChackGPT／JGBT／缺GPT／TradgePT／TrashGit的App → ChatGPT／ChatGPT App）
    - **Subagent**（Subaging／Sub Aging／Sup Agent／Sub-Agen／SubEdge／Sub H／A菌／SUB-A菌 → Subagent）。**⚠️同 EP06 的連寫坑**：規則命中已正確的 `Subagent` 內部 → `Subagentnt`（9 處），已用 `Subagentnt→Subagent` 修回。**日後 Subagent/Agent 類替換都要防「命中正確詞內部」**。
    - **生圖**（深圖／生徒／升塗／升圖）、**YAML**（YML）、**訂閱制**（訂閱字/式）、**Baked**（Back-Kit/Bakkit/Backit）純圖片 vs **Plate** 可編輯文字、**思源**（出源）、**逐頁**（竹葉）
    - 其他：Draw 技能（主要技能）、Claude Code（Claude Core）、AntiGravity（Antiguity）、Codex CLI（Codex COI）、關係圖（關機圖）、頁數（夜數）、溢位檢查（異位）、12頁（12月）、Agent（A-GEN/Agin/aging/A群）
  - 長片標題 Claude 自訂：「ChatGPT App 基本功 EP07：一份 YAML 指揮 6 個 AI 同時做簡報」
  - 長片交付：`output/ChatGPT App 基本功 EP07 一份 YAML 指揮 6 個 AI 同時做簡報 [Claude]/`（5 檔；**封面沿用 yt-pack `live-chatgpt-app-ep07/…yaml-6-agents`**，藍色科技風＋黃字，YAML→6 AI→6 簡報）
  - **短片 3 支全做**（16:9，自選主題＋標題）：
    - A「ChatGPT 生圖太慢？用 Subagent 讓它快好幾倍」（52.9s）
    - B「AI 生的簡報為什麼這麼好看？內容跟設計要分開」（38.6s）
    - C「AI 生圖一張只要 0.3 元｜API Key 比訂閱還划算」（28.8s）
    - 交付：各 6 檔；封面依慣例 draw.py 生**黑底電光藍＋黃字**（ChatGPT App 系列色）。未做 9:16（未要求）。
  - ffmpeg 環境解法沿用（PIL 字卡＋fonts.conf＋loop filter＋MSYS_NO_PATHCONV），1080p 全成功。
  - 上架提醒：說明欄放 **YAML Image Deck 技能 GitHub repo 連結**（片中承諾）、「把 OpenAI Image 2 API Key 裝進 Claude Code」那集連結。片尾 API Key＋6 Subagent 生圖**失敗**（Codex 禁止代理呼叫外部 API），三師爸刻意保留失敗實況。
  - **【2026-07-22 追加】第 4 支短片**（使用者指定「簡報畫面解說 YAML Image Deck 用途」那段）：
    - 「YAML Image Deck 是什麼？一份 YAML 讓 AI 簡報風格統一」（57.4s；`00:25:56.708-26:38.668, 00:29:42.962-29:51.442, 00:29:59.262-30:03.122`）
    - 內容：設計合約非繪圖程式 → 鎖風格 → 依內容決定版型 → 版型與風格對齊 → 放文字；YAML 管規則、AI 生成畫面、AI 自己驗收。畫面就是那張「YAML Image Deck·完整使用指南」簡報頁。
    - 交付 6 檔；封面黑底電光藍＋黃字（deck.yaml＋風格→版型→文字流程＋三張一致簡報縮圖）。
  - **⚠️ 事後修正（同日）**：長片字幕原有 **`Platee`** 誤字 2 處——先前替換規則「做Plat→做 Plate」「叫做Plat→叫做 Plate」命中了**已正確的 `Plate`** 造成連寫。已修回 `Plate`（0 殘留、正常 5 處），長片 `.srt`/`.txt` 已回寫交付資料夾；3 支舊短片未含此字、不受影響。**再次提醒：所有替換都要防「命中正確詞內部」（Agentt / Subagentnt / Platee 已連續三次踩到）。**
- **前一支（2026-07-19）**：**AI Agent 基本功 EP06（跨 Agent、跨電腦協作同一專案）長片＋3 短片全流程交付完成** ✅（使用者：全流程我決定、一次做到底、3 短片；全自動無 STOP）
  - 來源：Downloads `AI Agent 基本功EP06_跨 Agent 跨電腦協作專案.mp4`（原長 68:20 / 2.38GB；**CFR** 30/1、1080p，免轉檔）→ `raw/aiagent-ep06/`
  - smart-cut（0.06 / `0sec,0.1sec`，C:\temp 再搬回）→ 47:52（剪 29.9%）；亮度 ~203 無黑畫面
  - 字幕：Groq 11994 詞 → resegment 1110 段 → apply_vocab → 自行清字 → finalize 套 131+3 段 → validate 通過（1110 段、47:48）
  - **本集詞彙更正重點（AI Agent/跨機協作系列沿用）**：
    - **chezmoi**（被聽成十幾種：ChessMoy/ChesMoy/ChaseMoy/ChaseMode/Chess Mode/Chest Mode/TraceMode/TraceMoy/Trash mode/ChessMoid → chezmoi；指令 add/update 併修）
    - **Agent**（A群/A君/A型/aging/Edge/HG/Agin → Agent）。**⚠️踩坑**：apply_vocab 已先把部分 `Agen`→`Agent`，我又加「個Agen→個 Agent」導致既有 `Agent` 被二次替換成 **`Agentt`（23 處）**；已用 `Agentt→Agent` 修回。**日後對 `Agen` 一律用『後接非 t』的條件（避免傷 Agent），別做裸 `Agen→Agent`。**
    - agents.md（agin.md/agence.md/agent.md/Agents、MD/Agent's MD）、handoff.md（handoff.nb/handoff.m d）、.gitignore（gitignal）
    - Opus 4.8（up4.8）、Big Pickle（BigPo）、GLM 5.2、ChatGPT（ChateGPT）、Claude Code（Claude Coach）、Claude Code、GPT（Claude CodesGPT）
    - C槽（稀巢/西草/西朝）、撐爆（稱爆）、題庫建置（題戶/題固/建制/歷屆事體）、程式（城市）、桌機（桌子機）、多花 Token（多花圖）
  - 長片標題 Claude 自訂：「AI Agent 基本功 EP06：跨 Agent、跨電腦協作同一個專案，設定觀念一次到位」
  - 長片交付：`output/AI Agent 基本功 EP06 跨 Agent、跨電腦協作同一個專案，設定觀念一次到位 [Claude]/`（5 檔；**封面沿用 yt-pack `live-ai-agent-ep06/…v3`**，AI Agent 系列＝**黑底紫色**、三台筆電機器人連共用資料夾）
  - **短片 3 支全做**（16:9，自選主題＋標題）：
    - A「跨電腦做 AI 專案第一步｜別用瀏覽器的雲端硬碟」（64.5s）
    - B「換電腦不用重設｜chezmoi 一鍵同步你的 AI 技能與設定」（50.5s）
    - C「AI 技能就是一份說明書，一句話同步給所有 Agent」（38.1s）
    - 交付：各 6 檔（乾淨16:9／字幕16:9／srt／txt／cover／metadata）；封面依 yt-pack 慣例 draw.py 生**黑底紫色**終端機風（AI Agent 系列色）。未做 9:16（未要求）。
  - ffmpeg 環境解法沿用（PIL 字卡＋fonts.conf＋loop filter＋MSYS_NO_PATHCONV），1080p 全成功。
  - 上架提醒：說明欄放**三個技能（初始化/開工/收工）GitHub repo 連結**（片中承諾）、搭配觀念那集直播＋Claude Code EP10 連結。
- **前一支（2026-07-18）**：**OpenCode 基本功 EP06（各種模型正確用法＋Kimi K3）長片＋3 短片全流程交付完成** ✅（使用者要求「全流程你決定、一次做到底、3 短片都要」→ 全自動無 STOP）
  - 來源：Downloads `OpenCode 基本功 EP06...mp4`（原長 93:31 / 3.25GB；**CFR** 30/1、1080p，免轉檔）→ `raw/opencode-ep06/`
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，輸出 C:\temp 再搬回）→ 50:04（剪 46.5%）；亮度 187–202 無黑畫面
  - 字幕：Groq 12026 詞 → resegment 1240 段 → apply_vocab → 授權自行清字 → finalize 套 134 段更正 → validate 通過（1240 段全吻合、50:03）→ 未刪段
  - **本集詞彙更正重點**（OpenCode/模型系列沿用）：
    - **DeepSeek**（DeepSeq/Deep Stick/Dip-Cit/DeepSecret/Deep-6/DF6/Deep-Sig/Deepseeker → DeepSeek）
    - **GLM 5.2**（GroqM5.2/GroM5.2/GN5.2/GN5.2X → GLM 5.2）
    - **Grok 4.5**（Groq 4.5/瓜哥/刮個/Gwag/Gwack/Waq/Wag4.5 → Grok 4.5；**xAI 的 Grok 模型 vs Groq 免費 API provider 是兩回事**，「Groq的免費API」「Groq API」保留 Groq）
    - **GPT-5.6**（GPT-SOUL/GPT5.6Soul/GPT-S OUL → GPT-5.6）、Kimi K3（Gimini K3→Kimi K3）、Opus/Sonnet（Apps, Sonic）、Fable 5、Claude Fable 5
    - **Token**（圖墾/圖殼→Token）、多模態（多摩胎/多魔胎/多模太）、支援（資源圖片→支援圖片）、Agent（aging/A群/Edge）
    - 其他：主力（阻力/足利）、上下文（上下尾）、撐爆（稱爆）、閉源模型（必源模型）、養套殺（仰套殺）、三國鼎立（三國典力）、歷屆試題（歷屑/例介事體）、大陸（路板）、去審（去省，模型互審）、二分之一（1.2分之1）
  - 長片標題 Claude 自訂：「OpenCode 基本功 EP06：各種模型正確用法全解析，最強開源模型 Kimi K3 正式登場」
  - 長片交付：`output/OpenCode 基本功 EP06 各種模型正確用法全解析，最強開源模型 Kimi K3 正式登場 [Claude]/`（5 檔；**封面沿用 yt-pack 現成 v2**）
  - **短片 3 支全做**（16:9，Claude 自選主題＋標題）：
    - A「你的 AI 讀不到圖片，還不會告訴你」（48.9s；`00:08:55.720-09:41.540`）
    - B「切換模型會偷吃你的 Token｜OpenCode 切換原理」（44.2s；`00:27:14.826-27:43.646, 00:28:33.806-28:46.166`）
    - C「開源最強 Kimi K3 送兩倍用量，現在最划算」（43.4s；三國鼎立＋送兩倍＋CTA 三段）
    - 交付：各 **6 檔**（乾淨16:9／字幕16:9／srt／txt／cover／metadata）；**未做 9:16 直式**（使用者只說 3 短片；要直式可補）
  - **⚠️【重要】封面慣例已改（本次新增，日後一律遵守）**：頻道實際封面在 **`G:\我的雲端硬碟\2026第二大腦\yt-pack\<episode>\`**（一集一夾）。**剪某集前先查有無現成封面 → 有就沿用**；生封面以 yt-pack 慣例為第一優先（**純黑底＋真人去背照＋終端機風＋強調色：Claude 橘/Codex 藍/OpenCode 綠**）。已寫入 `CLAUDE.md §封面規範 第0條` 與 `assets/style/yt-pack-cover-convention.md`。本集長片用 yt-pack v2；3 短片依此用 draw.py 生黑底綠終端機風（效果佳）。
  - **ffmpeg 環境踩坑（沿用解法仍有效）**：drawtext segfault→PIL 畫字卡；libass 需 `fonts.conf`+`FONTCONFIG_FILE`；`-loop 1` PNG 溢位→`loop` filter；Git-Bash 需 `MSYS_NO_PATHCONV=1`。本集 1080p 字卡/燒字幕全成功。
  - **⚠️ 事後更正（2026-07-19，使用者指出）**：片中三師爸把 Kimi K3 的「2X」解讀成「OpenCode 送兩倍用量／省一半」是**錯的**——OpenCode 的 **2X＝消耗兩倍用量**（更快用完額度），非贈送。已處理：①長片 `.srt` 後半段 6 段（`00:45`–`00:48`）句末加更正註解、②長片 `metadata.md` 加更正、③**短片 C 整支重剪**：原「開源最強 Kimi K3 送兩倍用量，現在最划算」作廢刪除，改為「開源最強 Kimi K3 登場 三國鼎立，受益的是我們」（拿掉所有 2X 片段、換標題/封面/字幕，只留正確的三國鼎立·開源受益內容）。**日後 OpenCode/OpenRouter 系列遇到模型標 2X/×2 一律解讀為「雙倍消耗」。**
- **前一支（2026-07-12 深夜，Fable 5）**：**AI Agent 基本功 EP05（三層一次講清楚：技能/全域/專案）長片＋3 指定短片全流程交付完成** ✅
  - 來源：Downloads `AI Agent基本功 EP05_三層一次講清楚_搞定 技能_全域_專案.mp4`（61:46 / 2.1GB / 真 CFR）→ `raw/aiagent-ep05/`；smart-cut → 45:51（剪 25.8%）
  - 字幕：Groq 10872 字 → 修正版 resegment 998 段（**孤兒殘尾 0**，修復持續生效）→ 使用者授權清字全自動 → 156 段更正 → validate 通過 → 刪 1 抖內 → 最終 997 段 / 45:51
  - **本集詞彙更正重點**：Edge TTS 家族（HTTS/Agen DTS/HTS松任/STS，**先做保護避免被 Agent 規則掃到**）、Agent 家族超多變體（Aging/AGEN/A-GEN/Agin/Agen/AiGEN/A局/A群/Edging/ARH…）、agint.md→**AGENTS.md**、權域→全域、三層備員→備援、圖坑→Token、人民黨民→**人名檔名**（引用語音正規化規則的關鍵句）、Kamit加Push跟Pori→Commit加Push跟Pull、108克剛→108課綱、Gil repo→Skill repo、李俊尼Soyul→李俊儀SOIL、C-Dance→Seedance、Meta Bloodlib→Matplotlib、F5 MPEG→FFmpeg、三師爸的生意→聲音
  - 長片標題**沿用原始檔名**（使用者指示）：「AI Agent基本功 EP05_三層一次講清楚_搞定 技能_全域_專案」；橘色三層平台封面；5 檔
  - **短片 4 支**（①②③使用者指定主題、④追加「技能的定義」；標題我定，全自動，各 7 檔含直式 9x16）：
    ①「語音輸入有錯字不用改：AI 自動幫你正規化」(59s；00:24:23-00:25:20 連續段)
    ②「叫 AI 做完事用語音回答你，不用再盯螢幕」(57s；含 27:40 起的 TTS 原理語音展示段)
    ③「專案思維：一個 repo 讓任何 AI 重現台語教材工作流」(59s；38:07+39:05 兩段)
    ④「技能到底是什麼？一句話：一份使用說明書」(57s；09:45 定義＋12:11 通用格式＋14:32「你長出來的技能才是你的技能」金句收尾)
  - 注意：影片承諾 **Padlet 素材包**（安裝懶人包/連接指南/兩個全域設定福利/技能分享區）連結放說明欄，長片與短片①②都要補
- **前一筆（2026-07-12）**：從 OpenCode EP05 成品截 3:11–6:36 做**單一教學片段**「OpenCode 免費版安裝教學：從下載到開啟 Big Pickle」→ `output/…[Claude] (教學片段)/`（4 檔：乾淨 mp4／字幕燒錄版／srt／txt，3:26）
  - 切點對齊字幕邊界（00:03:11.300–00:06:36.820）；字幕重新清洗 13 段（舊版 resegment 的「OpenC/ode」斷頭、按消→按下、選擇過帳號→選 Google 帳號、T&Agent→體驗Agent、值得以用→值得使用等），時間碼未動、段數 88 不變，燒錄後抽幀驗證 OK
  - 註：EP05 主成品的 srt 未回改（僅此片段清洗）；若日後要重發 EP05 長片可比照更正
- **進度**：**ChatGPT APP 基本功 EP06（Codex 更名大改版）長片＋3 短片全流程交付完成；並修復 resegment.py「孤兒殘尾」bug** ✅
  - 🔧 **resegment.py 重大修復（全域＋專案已同步，日後所有影片受益）**：
    - 症狀：一句話最後 1–2 個字被截到下一段開頭（如「…需要的程」／「式,完全免費!」）
    - 成因：規則 2 盲信 Whisper segment 邊界（常落在句尾前 1–2 字）＋規則 5 硬切不前瞻
    - 修法：`punct_ahead()` 前瞻守衛（切前看 1–2 詞內有無標點，有就寬限）＋ `rescue_orphan_tails()` 最終救援（前段無標點收尾時，段首 ≤2 字＋標點殘尾整批搬回前段）＋ 發語詞白名單（好/嗯/欸…開頭不誤搬）
    - 實測：EP03 真孤兒 11→0、EP04 16→0、EP06 新片 0；發語詞段完好；SKILL.md 已補文件
    - ⚠️ 跨段替換小坑：清字時若替換詞跨段（如 XHigh），比例分配會把英文詞切成 XHig/h，需手動收回前段
  - 來源：`ChatGPT APP 基本功第六集...mp4`（38:30 / 1.3GB / 真 CFR 無黑幀）→ `raw/chatgpt-ep06/`；smart-cut → 22:43（剪 41%）
  - 字幕：Groq 5624 字 → 修正版 resegment 574 段（孤兒 0）→ STOP1 使用者確認 8 項（YL→SOL、Walk→Work、開到早→XHigh、3萬→100萬、龍蝦=遙控工具暱稱保留、Faili→Thinking、和平→權限、Claude GPT→Claude 跟 GPT）→ 67+ 段更正 → validate 通過 → 刪 1 抖內 → 最終 573 段 / 22:41
  - **本集詞彙更正重點**：ChatGPT APP 變體（ChartGPT/ChatsGPT/CLAGPT/CheckGPTAPP/ClaudeGPT APP/**LGBT應用程式**）、SOL 變體（SOUL/Sold/SORO/Zone）、Terra（ERA/5.6TB）、Luna、Opus 4.8（UP4.8）、Sonnet（Sone）、Netlify/Vercel/Google Sites、世足賽（四足賽）、姆巴佩（巴佩）、託管（脫光）、HTML簡報（HCM簡報）
  - 內容：Codex 正式更名 ChatGPT APP。三模型 SOL/Terra/Luna（太陽/地球/月球，對標 Opus/Sonnet/Haiku）、對話整合、**內建網站託管**（3D 賽車實演直接發布）、Remote Control 手機遙控、語音=GPT5.5 Thinking、內建生圖；SOL 需 $100 方案、額度重置有期限；講者評：目前第一名、付費首選
  - 長片標題使用者選 A1：「ChatGPT APP 基本功 EP06：全新改版大解析，教師教學神器一次看懂」；主色**藍色**（GPT 家族）；交付 5 檔
  - **短片 4 支**（使用者授權標題我定；第 4 支為使用者追加的 Remote Control 段）：A「Codex 沒了！改名 ChatGPT APP，我把第一名頒給它」(54s)／B「一句話生出 3D 賽車，直接發布上線給大家玩」(57s)／C「太陽、地球、月球：ChatGPT 三模型這樣選就對了」(46s)／D「用手機遙控電腦上的 ChatGPT APP，不用帶電腦也能做事」(51s)；各 7 檔（含直式 9x16）
  - 追加修正：Remote Control 段的「remove」誤聽 → **Remote**（3 處），working 與 output 長片 srt/txt 皆已同步修正
  - 注意：**頻道系列更名**「GPT Codex 基本功」→「ChatGPT APP 基本功」，上架時考慮新播放清單；長片說明欄放 3D 賽車 demo 網址
  - 📐 **工作流新規則（2026-07-11 使用者拍板，已寫入 skill）**：**每支短片必加產 9:16 直式版**（YouTube Shorts 發布用）
    - 新腳本 `skills/short-video-workflow/scripts/make_vertical.py`：模糊背景填充、內容不裁切、1080×1920、自動驗證解析度；輸入用已燒字幕版（字幕跟著前景走，不重燒）
    - 已更新：`short-video-workflow/SKILL.md`（規格表、Step 6-3、Step 8 檔名 `_直式9x16.mp4`、checklist 7 檔、踩坑）＋ Claude/Codex 兩份總控 SKILL.md 階段 2
    - EP06 三支短片已回填直式版（各 7 檔）；**更早的短片（EP03/EP04/台語短片）沒有直式版**，要發 Shorts 時可用同腳本補（Codex EP05 三支當時已手動做過直式）
- **前一支（2026-07-06）**：**AI Agent 基本功 EP04（連接外部工具／MCP 一張地圖）長片＋3 短片全流程交付完成** ✅
  - 來源：使用者 Downloads `AI Agent基本功_EP04...mp4`（原長 96:14 / 3.3GB；**VFR**）
  - ⚠️ **重大踩坑（已解，日後注意）**：smart_cut 的 VFR 防護對本片 `avg=519732000/17324401`≈30.0001 **漏判**，auto-editor 吃 VFR → cut 從約 5 分鐘後**全黑幀**（YAVG=0）。修法：手動 `ffmpeg -fps_mode cfr -r 30 -c:v libx264 -preset veryfast -crf 20 -c:a copy` 把原始轉真 CFR（avg=r=30/1）→ 再跑 smart_cut → 亮度全時段 >0。**已 flag 修 smart_cut 任務（task_f5316702）**。凡 VFR 影片剪完務必抽樣多點 YAVG 驗證！
  - smart-cut（CFR 版）→ 43:59（剪 54.3%）
  - 轉字幕：Groq 11035 字 → resegment 1124 段 → **本片使用者授權全自動（清字/標題/短片全我決定）** → 大小寫敏感+regex 腳本套 190 段更正 → validate 通過 → 刪 4 抖內 → 最終 1120 段 / 43:58
  - **本集詞彙更正重點**（超多，MCP/連接器主題）：
    - Padlet（Pelet/Pallet/Panlet/Pellet/Pedlet/Adlet）、**OAuth**（Auth/oast/Obs/AWS/All-search/OUS/GetOth，**regex `Obs(?!idian)` 保護 Obsidian**）、**Access Token**（AXS Token/Sex2K/Assess2Key/Assesspoken）、**CLI**（Cli/DLI/C-I）、**Wordwall**（WOW/WordWord/WorldWorld/渦污）、**Google Tasks**（Google Desk/GoDesk/Google Dex）、**Google Cloud Console**（Google Claude Console→Cloud）、Canva（Camba/CAMBA/CAMMA）、Classroom（Clusserun）、Firebase（Filebase）、Supabase（Super Base/SuperBass）、PowerShell（PowerShare）、JSON（Jason）、C槽（西朝）、行事曆（形式力）、非Google（FayGoogle）、Agent 家族、權限（全線）、免鑰匙（免要時）、伸出（增出）
  - 內容：Claude Code 示範連外部工具。兩問題（是否 Google／簡單 vs 完整控制）、四通道（內建連接器/MCP/CLI/直接操控）、權限低→高（免鑰匙→API Key/Access Token→OAuth）；實測 Wordwall/NotebookLM(MCP CLI)/Google Tasks(OAuth 經 Google Cloud Console 超麻煩)；手機 Remote Control 遙控電腦
  - 主色**橘色**（Claude Code）；長片標題我定：「AI Agent 基本功 EP04：連接外部工具，MCP 與連接器一張地圖講清楚」
  - 長片交付：`output/AI Agent 基本功 EP04：連接外部工具，MCP 與連接器一張地圖講清楚 [Claude]/`（5 檔；封面橘色 MCP 連接地圖）
  - **短片 3 支**（使用者授權我自訂標題）：①「別再一個一個學工具，AI 用自然語言全幫你控制」(44.6s) ②「MCP 是什麼？一個轉接頭比喻讓你秒懂」(29.5s) ③「用手機遙控電腦上的 Claude Code，走到哪交代到哪」(31s)；各 6 檔
  - 注意：長片承諾「連接指南 GitHub repo／Padlet」連結放說明欄，上架務必補
- **前一支（2026-07-05）**：**AI Agent 教學應用「予 AI 講台語」（免費開源台語教材產生器）長片＋1 精華短片全流程交付完成** ✅
  - 來源：使用者 Downloads `AI Agent教學應用_予 AI 講台語...mp4`（原長 18:27 / 642MB；**VFR** → smart_cut 自動轉 CFR）→ 歸檔 `raw/taigi-agent/`
  - smart-cut → 12:22（剪 32.9%）；亮度 ~200 無黑畫面
  - 轉字幕：Groq 2719 字 → resegment 280 段 → **本片使用者授權全自動（清字/標題/短片全我決定，一次做到好）** → 大小寫敏感腳本套 31 段更正 → validate 通過 → 刪 1 段抖內 → 最終 279 段 / 12:22
  - **本集詞彙更正重點**：AI-Aging/Aging/Agin/AIAgin/A群→**Agent/AI Agent**、**西朝→C 槽**、**異傳→意傳**（台語語音平台）、Open Code→OpenCode、GitHub report→repo、Claude Codes→Claude Code、台語「菜市仔」變體統一（蔡奇亞/菜奇亞/菜池仔/菜茄子→**菜市仔**）
    - 註：台語示範播放段（阿嬤/翻牌遊戲音）為 AI 生成台語音，Whisper 無法精準轉寫，保留近似（非主述旁白，影響小）
  - 內容：Claude Code 示範，把 GitHub repo 丟給 Agent 安裝 → 專案內對話生成台語講義/考卷/語音(意傳官方發音)/教學影片/網頁互動遊戲/PPT；完全免費、免 API Key。核心：用「一個專案資料夾」承接整套複雜工作流
  - 主色**橘色**（Claude Code 示範）；長片標題我定：「AI Agent 教學應用：予 AI 講台語，免費開源專案一鍵生成台語教材」
  - 長片交付：`output/AI Agent 教學應用：予 AI 講台語，免費開源專案一鍵生成台語教材 [Claude]/`（5 檔）
  - **短片只做 1 支**（使用者指定：講解專案能做什麼的最精華一支）：「予 AI 講台語，免費專案生成所有台語教材」（39.9s，6 檔；問題→能生成一切→完全免費→連結）
  - 注意：長片與短片都承諾「台語專案 GitHub repo」連結放說明欄，上架務必補
- **前一支（2026-07-04）**：**「教師的第二黑板：自媒體經營術」（三師爸自媒體經營直播）長片＋1 短片（含 16:9＋9:16）全流程交付完成** ✅
  - 來源：使用者 Downloads `教師的第二黑板-自媒體經營術.mp4`（原長 75:37 / 240MB；**CFR** 30/1、720p，免轉檔）→ 歸檔 `raw/teacher-media/`
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，輸出 C:\temp 再搬回）→ 51:34（剪 31.8%）；亮度抽樣 164–217 無黑畫面
  - 轉字幕：Groq 13250 詞 → resegment 1341 段 → apply_vocab → **STOP1 使用者確認人名**（通閒/崇尚/充賢→**聰賢老師**、海西/凱希→**凱西**、連魚人/連連任長→連育仁、王岐霖→王齊麟、Jomein→Joeman、愛麗莎莎→愛莉莎莎）→ finalize 套 66 段更正 → validate 通過（1341 段全吻合、51:13）→ 未刪段（Padlet 互動段依使用者保留）
  - **本集詞彙更正重點**（自媒體/教學直播系列可沿用）：日媒體/直媒體/進一字媒體→**自媒體**、教師演習→**教師研習**、後色認知→**後設認知**、平留時間→**停留時間**、出訴/初速→**出書**、廣告分論/分額→**廣告分潤**、腰月延期→**邀約演講**、天文→**貼文**、低促進→**低觸及**、三八人/sumbody→**somebody**、反播性→傳播性、同音程→**同溫層**、寡大→廣大、四文化→次文化、配列→**Padlet**、確訊據查→搜尋、破片→選擇、暈得上→跟得上
  - 長片標題**Claude 自行拍板**（使用者授權「標題由你決定」）：「認真老師的第二黑板：把影響力放大 10 倍的自媒體經營術」
  - 長片交付：`output/認真老師的第二黑板 把影響力放大 10 倍的自媒體經營術 [Claude]/`（5 檔；封面**橘色**、綠色黑板＋成長箭頭＋社群圖示）
  - **短片使用者最後決定只做 1 支**（原本規劃 A/B/C 三支，STOP2 後使用者選「只做 C」）：
    - C 工具型「5 天 vs 2 小時：AI 怎麼幫老師剪教學影片」（36.64s：33.6s 內容＋3s 字卡）
    - 時間碼：`00:12:14.300-00:12:47.900`（連續段，切點乾淨）
    - 交付：`output/5 天 vs 2 小時 AI 怎麼幫老師剪教學影片 [Claude] (Short)/`（**7 檔**：乾淨16:9／字幕16:9／**直式9x16 1080×1920**／srt／txt／cover／metadata）
  - **⚠️ 本機 ffmpeg 環境踩坑（新，日後沿用解法）**：
    - `ffmpeg 8.1.2 (gyan.dev)` 的 **drawtext 會直接 segfault**（`Fontconfig error: Cannot load default config file`）→ 結尾字卡改用 **PIL 畫 PNG**（`skills` 未動，臨時腳本）再轉影片
    - **libass 燒字幕也需 fontconfig** → 建一份 `fonts.conf`（`<dir>C:\Windows\Fonts</dir>`）並 `export FONTCONFIG_FILE=...` 才能燒中文字幕（`subtitles=...:force_style='FontName=Microsoft JhengHei'`）
    - `-loop 1 -i xxx.png` 會 **parser buffer 溢位**（Failed to reallocate parser buffer to 負值）→ 改用 `-i png` + `loop=loop=-1:size=1,fps=30,trim=duration=3` filter 生成靜態卡影片
    - Git-Bash 跑含 filter 的 ffmpeg 要 `export MSYS_NO_PATHCONV=1 MSYS2_ARG_CONV_EXCL='*'`，否則 filter 裡的 `:`／`/path` 被 MSYS 轉義
    - **`add_end_card.py` 在此環境失效**（drawtext + 硬寫 1920×1080 與 720p 不符）；本次手工繞過。日後若要修 skill：改字卡尺寸吃來源解析度、drawtext 換 PIL 或修 fontconfig
- **前一支（2026-07-03）**：**GPT Codex 基本功 EP05（Codex 超好用五功能）長片＋3 短片（各含 16:9＋9:16）全流程交付完成** ✅
  - 來源：使用者 Downloads `GPT Codex 基本功EP05...mp4`（原長 29:40 / 1GB；**VFR！** avg≠r_frame_rate → smart_cut 自動轉 CFR）→ 歸檔 `raw/codex-ep05/`
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，輸出 C:\temp 再搬回）→ 16:02（剪 45.9%）；亮度抽樣 ~200 無黑畫面（VFR 轉檔成功）
  - 轉字幕：Groq 4176 字 → resegment 422 段 → apply_vocab → **本集使用者授權我自行確認清字（跳過 STOP1）** → 大小寫敏感腳本套 25 段更正 → validate 通過（422 段）→ 無抖內段
  - **本集詞彙更正重點**（Codex 系列沿用）：Versale→**Vercel**、Netify→**Netlify**、Coldex/Podex→**Codex**、AIA菌→**AI Agent**、Compute Use→**Computer Use**、Agent.md→**AGENTS.md**、All Decks→**Codex**（首選推薦）、Edge/agent→Agent、資料獎→資料夾
  - 長片標題**我自行選定**（使用者授權）：「GPT Codex 基本功 EP05：Codex 超好用的五個隱藏功能」
  - 長片交付：`output/GPT Codex 基本功 EP05：Codex 超好用的五個隱藏功能 [Claude]/`（5 檔；封面**藍色**Codex 主題、CLI 視窗大「5」+五功能圖示）
  - **短片 3 支全做**（使用者要全做＋自選主題）——**新增求：每支多產一支 9:16 直式版**：
    - 目標模式「設好目標，Codex 自動做到完才停」（43.3s）
    - 語音輸入「Codex 內建語音輸入，免費又準取代付費工具」（40.3s）
    - Computer Use「讓 Codex 接管瀏覽器，複雜網站自動幫你操作」（44.9s）
    - 交付：`output/<各短片標題> [Claude] (Short)/`（各 **7 檔**：乾淨 16:9／字幕版 16:9／**直式9x16 1080×1920**／srt／txt／cover／metadata）
  - **9:16 直式做法（新，日後沿用）**：clip_cut 不支援 aspect；用 ffmpeg 把字幕版 16:9 轉直式——`split` 複製一路 `scale=1080:1920:increase,crop,gblur=sigma=25` 當模糊背景，另一路 `scale=1080:-2` 前景置中 overlay。內容不裁切（螢幕示範不能裁 UI），字幕沿用 16:9 燒錄版
- **前一支（2026-06-29）**：**AI Agent 基本功 EP03（Agent 的四隻手腳：讀檔/寫程式/上網/出成品）長片＋3 短片全流程交付完成** ✅
  - 來源：使用者 Downloads `AI Agent 基本功 EP03...mp4`（原長 75:10 / 2.6GB；**CFR** 30/1，免轉檔）→ 歸檔 `raw/aiagent-ep03/`
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，**輸出 C:\temp 再搬回**）→ 48:35（剪 35.4%）；亮度抽樣 ~200/255 無黑畫面
  - 轉字幕：Groq 12615 字 → resegment 1215 段 → apply_vocab → STOP1 使用者確認 8 處 → 自寫大小寫敏感+跨段替換腳本套 115 段更正 → validate 通過（1215 段全吻合）→ 刪 4 段抖內 → 最終 1211 段 / 48:34
  - **本集詞彙更正重點**（AI Agent 系列沿用）：
    - Agent 家族：AiGeng/Aging/Edging/Edge/Agin/Agen/AGE/A群/AI局/AI群/AIA/AIG → **Agent / AI Agent**（**大小寫敏感**避免誤傷既有 Agent；Agen 用負向預看 `(?!t)`）
    - **Chrome vs Claude**：段 360/370/376/377「Claude 線上程式應用商店 / Claude 瀏覽器」→ **Chrome**（該段在裝 Claude in Chrome 擴充，商店與瀏覽器是 Chrome）
    - 工具/套件：Quizzer→Whisper、Tibeless/TypeList/Tabless→**Typeless**、MarkDone→**MarkItDown**、MetaPlanetLive→**Matplotlib**、PlayWrite→**Playwright**、BigPicco→**Big Pickle**、大煙菜→大醃菜
    - 模型：Opus.8→**Opus 4.8**、GLM5.2→**GLM 5.2**；**GPT5.5 保留原文**（使用者拍板）
    - 用語：複製入鏡→**複製路徑**、未給(檔案)→**餵給**；抖內/喊名段全刪（818/847/956/957）
  - 長片標題使用者選 A3：「AI Agent 基本功 EP03：一句話讓 AI 幫你讀檔、寫程式、上網、做出成品」
  - 長片交付：`output/AI Agent 基本功 EP03：一句話讓 AI 幫你讀檔、寫程式、上網、做出成品 [Claude]/`（5 檔齊全；封面橘色、AI 機器人四隻手臂＝檔案/終端機/網路/PowerPoint）
  - **短片一次交付 3 支** ✅（使用者指定 B6/D9/D10 主題，由我自選段落組順敘三幕；clip_cut 會自動按時間排序，故 hook 都選在時間軸最前段）：
    - B6「凌亂的下載資料夾，一句話讓 AI 幫你整理乾淨」（55.5s）：痛點資料夾沒空整理 → AI 一句話分類 → 整整齊齊＋待刪確認
    - D9「一句話，AI 上網抓會考題自動做成 PowerPoint 考卷」（62.7s）：一句話下載會考題 → 每題截圖排 PowerPoint＋自動解答 → 一次做完幫老師省勞務
    - D10「一份免費清單，讓 AI 變身老師的文書神器」（47.6s）：老師 Python 工具清單 → 丟 .md 給 Agent 全自動裝好 → GitHub repo 放說明欄
    - 交付：`output/<各短片標題> [Claude] (Short)/`（各 6 檔齊全；封面橘色＋SHORT 標識）
  - 注意：影片承諾「老師 Python 工具清單 GitHub repo」與「Big Pickle＝OpenCode EP05」連結放說明欄，上架務必補（D10 短片亦承諾工具清單 repo 連結）
- **前一支（2026-06-28）**：**OpenCode EP05（免費 Big Pickle 與最強 GLM 5.2）長片＋2 支短片全流程交付完成** ✅
  - 來源：`raw/opencode-ep05/opencode-ep05.mp4`（原長 56:22 / 1.83GB；**CFR** 30fps，免轉檔）
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，**輸出 C:\temp 再搬回**）→ 33:25（剪 40.7%）
  - 轉字幕：Groq 8046 字 → resegment 825 段 → apply_vocab → STOP1 使用者確認 6 處 → 套全部更正 → validate 通過（825 段全吻合，本集無整段刪除，喊名段以「保留工具名」方式編輯）
  - **本集詞彙更正重點**（OpenCode 模型系列可沿用）：
    - 免費模型：`Big Pico/BigPico/Pico/BigCode/BigPo` → **Big Pickle**；pickle 哏 `煙黃瓜/煙菜` → 醃黃瓜/醃菜（使用者拍板）
    - `GLM5.2/ULM2/Gm5.2` → **GLM 5.2**；`OpenLauter` → OpenRouter；`OpenGo` → OpenCode Go 方案
    - `Tragibility Code` → **ChatGPT Codex**（使用者確認）；`Deepstick/Disk V4` → DeepSeek V4；`seal大加/Gill` → SOIL Teaching Deck
    - 抖內名刪除但**保留工具名**：`Claude Flare的Walker AI` 其實是 **Cloudflare 的 Workers AI**（已修正）、Z-Image Turbo 保留
    - 補修：段 50「Claude B」→「Big Pickle」（剪短片時發現，長片字幕已同步）
  - 長片標題使用者選 #4：「OpenCode 基本功 EP05：免費 Big Pickle 與最強 GLM 5.2」
  - 長片交付：`output/OpenCode 基本功 EP05：免費 Big Pickle 與最強 GLM 5.2 [Claude]/`（5 檔齊全；封面橘色、左免費醃黃瓜 Big Pickle vs 右皇冠 GLM 5.2）
  - **短片 2 支**（使用者指定主題、由我自選段落拼成完整敘事）：
    - 「學 AI Agent 的最佳免費 Plan B：Big Pickle」（81.3s）：研習痛點 → Plan B＝Big Pickle → 實測能打
    - 「AI 自己寫技能：免費生圖＋自動疊中文字」（89.3s）：OpenCode 沒生圖 → GLM 5.2 寫 Draw Free 技能 → 生圖＋疊字 → 打包 repo
    - 交付：`output/<各短片標題> [Claude] (Short)/`（各 6 檔齊全）
  - 注意：影片中承諾 **Draw Free 技能 GitHub repo 連結與 OpenCode Go 推薦碼** 放說明欄，上架務必補上
- **前一支**：**AI Agent 基本功 EP02（核心觀念與初始化設定）長片＋3 支短片全流程交付完成** ✅
  - 來源：**YouTube 直播下載**（`https://youtube.com/live/gm1ln1Z0hHo`）→ yt-dlp 1080p → `raw/aiagent-ep02/`（原長 78:58 / 435MB；**CFR** 30fps，免轉檔）
  - **下載坑**：長時間下載先存 `C:\temp` 再搬進 raw，避免 GDrive 同步抓到半成品
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，**輸出 C:\temp 再搬回**）→ 52:36（剪 33.4%）。剪輯版有深色投影片（如「四大 Agent 全域設定對照」），抽樣偏暗屬正常非黑畫面
  - 轉字幕：Groq 13495 字 → resegment 1328 段 → apply_vocab → STOP1 使用者確認 5 處 → 套全部更正 → validate 通過（1328 段全吻合）→ 刪 8 段抖內/喊名 → 最終 1320 段
  - **本集詞彙更正重點**（觀念課術語多）：
    - `AI Aging/Aging/AIA群/A群/A局` → Agent；`痊癒/權域/權育` → 全域；`全線(指權限)` → 權限；`Edge/Edging` → Agent
    - `Tucan/團/圖/肯` → Token；`Compat` → Compact；`GitInitial` → git init；`GitEgnome/Ignome` → .gitignore；`專案出什麼` → 專案初始化
    - 模型名：`Obs/OPPS` → Opus、`FAPO5` → Fable 5、`Soulnet` → Sonnet、`Dipseek` → DeepSeek、`Artracode` → Ultra（版號保留原文）
    - **踩坑（新）→ 已解決（2026-06-22）**：resegment 會把詞切在兩段邊界（如「痊癒」被切成段尾「痊」+段首「癒」），舊版逐段 replace 抓不到。`apply_vocab.py` 與 `finalize_subtitles.py` 已改為跨段替換引擎（詳見下方 §「跨段詞彙替換」）
  - 長片標題使用者選 #4：「AI Agent 基本功 EP02：學習 Agent 必懂的核心觀念與初始化設定」
  - 長片交付：`output/AI Agent 基本功 EP02：學習 Agent 必懂的核心觀念與初始化設定 [Claude]/`（5 檔齊全；封面橘色、AI 核心＋五觀念節點）
  - **短片一次交付 3 支** ✅（使用者要 A/B/C 三版都做）：
    - A 痛點型「AI 越用越笨、用量還爆掉？關鍵是上下文」（50.6s）
    - B 觀念型「全域放我是誰，專案放我要做什麼」（43.9s）
    - C 知識型「AI 模型分三級，這樣選又聰明又省錢」（30.4s）
    - 交付：`output/<各短片標題> [Claude] (Short)/`（各 6 檔齊全）
  - 注意：影片多次提到「延續 EP01」，上架說明欄放 EP01 連結
- **前一支**：**AntiGravity EP08（Agent 代理・複製你的聲音／VoxCPM2）長片＋3 支短片全流程交付完成** ✅
  - 來源：`raw/antigravity-ep08/antigravity-ep08.mp4`（原長 67:58 / 2.2GB；**本片是 CFR**，r=avg=30/1，免轉檔）
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`，**輸出到 C:\temp 再搬回**避開 GDrive 脫水）→ 42:12（剪 37.9%）
  - 轉字幕：Groq 10988 字 → resegment 1084 段 → apply_vocab → STOP1 使用者確認 8 處 → 套全部更正 → validate 硬關卡通過（1084 段全吻合）→ 刪 16 段（4 段 CMNNN 亂碼＋12 段抖內/喊名）→ 最終 1068 段
  - **本集詞彙更正重點**（語音克隆系列可沿用）：
    - 工具名：`VOXCPM2/Vox CP2/Vox CPM2/VoXcp/VoS CPM2` → **VoxCPM2**；`HyperFran/Hyperplane/Hyper Flame` → **HyperFrame**（使用者拍板）
    - `Edge/Edging` → Agent；`STT/RTS/PTS/SET` → **TTS**；`Test to Sum` → Text to Speech；`Engine`（指另一代理）→ Agent
    - 使用者拍板：`名語`→閩南語、`348`→三師爸、3D加速卡**保留**；刪除一段 CMNNN 亂碼；抖內/喊名段全刪
  - 長片標題：**依檔名** →「AntiGravity 基本功 EP08：Agent 代理，複製你的聲音」
  - 長片交付：`output/AntiGravity 基本功 EP08：Agent 代理，複製你的聲音 [Claude]/`（5 檔齊全；封面橘色、麥克風＋聲波＋AI 機器人＋FREE）
  - **短片一次交付 3 支** ✅（使用者要 A/B/C 三版都做）：
    - A 痛點型「別再付費買 AI 語音了」（65.5s）：00:00:09-13.6 hook → 00:00:21.1-46.5 開源免費 → 00:07:22.1-54.5 能力總結
    - B 好奇型「老師的聲音，讓 AI 幫你配教學影片」（57.8s）：複製超像 → 可複製/創造 → 老師聲音做教學影片
    - C 警示型「你的聲音 30 秒就能被複製」（57.3s）：能複製任何人聲音 → 教學應用 → 防詐 AI 素養
    - 交付：`output/<各短片標題> [Claude] (Short)/`（各 6 檔齊全；C 版封面帶警示紅）
  - **第 4 支短片（趣味亮點）也已交付** ✅（使用者指定擷取小克×三師爸對談段）：
    - 標題：「AI 自動生成雙人對話，連貓咪聲音都有」（39.5s）
    - 組合：00:37:48.2-54.3 下指令開場 ＋ 00:41:11.9-42.3 黑貓小克×三師爸 AI 對談本體
    - 交付：`output/AI 自動生成雙人對話，連貓咪聲音都有 [Claude] (Short)/`（6 檔齊全；封面可愛黑貓＋雙聲波）
  - 注意：影片中多次承諾「VoxCPM2 的 GitHub repo 連結放說明欄」，上架務必補上
- **前一支**：**AI Agent 基本功 EP01（用 Agent 學 Agent）長片＋短片全流程交付完成** ✅
  - 來源：`raw/aiagent-ep01/aiagent-ep01.mp4`（原長 75:20 / 2.44GB，來自使用者 Downloads；**VFR**）
  - **預防 VFR 黑畫面**：剪前先 ffprobe 確認 avg_frame_rate 非整數比 → 先轉 CFR（`-fps_mode cfr -r 30 -crf 18 -c:a copy`）再剪，全程驗證亮度正常
  - **GDrive 寫入坑（新）**：auto-editor 直接輸出到 GDrive 路徑時，檔案會在 smart_cut 報 OK 後消失（疑似 GDrive 同步脫水）。解法：輸出到本機 `C:\temp\`，再 `cp` 搬回專案。建議日後長片 smart-cut 一律輸出本機再搬回。
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`）→ 48:48（剪 35.2%）
  - 轉字幕：Groq 11992 字 → resegment 1154 段 → apply_vocab → STOP1 使用者確認 8 處 → 套全部更正 → validate 硬關卡通過（1154 段全吻合）→ 刪 11 段抖內/喊名段重編號 → 最終 1143 段
  - **本集詞彙更正重點**（AI Agent 系列日後可沿用）：
    - Agent 家族：`A-GEN/Agin/Aging/AI Aging/AIH/AIA群/A群/agy/AIGEN` → **Agent / AI Agent**
    - 生成式：`生程式/真程式/真誠式/聖誠士/聖德斯/深層式/生存式/Sense4/CN4` → **生成式**
    - `Antigravity/Antigrity/Ntegraf2/MTGRAVIT2/agy/HGY` → **AntiGravity**；`Podex`→Codex、`Deepthic`→DeepSeek
    - 使用者拍板：GPT5.5/Gemini3.5/Fable **保留原文**；`充WL的服務員員`→充值；`破搓`→裹足不前；**抖內/觀眾名段落全刪**
  - 長片標題使用者選 #10：「一個 GitHub repo，複製我的整套 AI 工作流到你的 Agent」
  - 長片交付：`output/一個 GitHub repo，複製我的整套 AI 工作流到你的 Agent [Claude]/`（5 檔齊全；封面橘色、GitHub repo 複製 Agent 大軍主視覺）
  - **短片也已交付** ✅（A 痛點型，67.5s＋3s 字卡＝70.5s）：
    - 標題：「AI Agent 就是你的第二大腦」
    - 精華組合（3 段）：00:13:27.9-00:14:04.9 切換工具痛點 → 00:14:40.9-00:15:00.8 Agent 解放勞務 → 00:16:17.3-00:16:27.7 第二大腦收尾
    - 交付：`output/AI Agent 就是你的第二大腦 [Claude] (Short)/`（6 檔齊全）
  - **短片 C 版也已加剪交付** ✅（C 承諾型，49.9s＋3s 字卡＝52.9s，<60s 進 Shorts feed）：
    - 標題使用者選：「AI 時代，工作流可以一鍵複製給別人的 Agent」
    - 精華組合（3 段）：00:46:35.5-00:46:51.5 一個 repo＝複製一整個部隊 → 00:46:54.1-00:47:09.2 丟網址全自動跑一遍 → 00:47:16.2-00:47:34.8 複製任何人工作流、寫給 Agent 看
    - 交付：`output/AI 時代，工作流可以一鍵複製給別人的 Agent [Claude] (Short)/`（6 檔齊全；封面橘色、GitHub 一鍵複製 Agent 大軍）
  - 注意：影片中承諾「用 Agent 學 Agent 知識庫 repo 連結放說明欄」，上架時務必補上
- **前一支**：**AntiGravity EP07（Padlet MCP）長片＋短片全流程交付完成** ✅
  - 來源：`raw/antigravity-ep07/antigravity-ep07.mp4`（原長 48:18 / 1.56GB，來自使用者 Downloads）
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`）→ 28:44（剪 40.5%）
  - 轉字幕：Groq 7288 字 → resegment 741 段（平均 2.33s）→ apply_vocab → STOP1 使用者確認 8 處疑慮 → finalize 套 155 段修正 → validate 硬關卡通過（741 段全吻合）→ 依使用者指示刪除段 740（觀眾暱稱）重編號 → 最終 740 段 / 10925 字
  - **本集詞彙更正重點**（Padlet 系列日後可沿用）：
    - `Pellet/Pallet/Paylet/Pelet/Petlet/Headlet/Panelad/Pairline/Panelette/Panet/配列` → **Padlet**
    - `Agin/A群/AIA群/A集員/Edging/AI Aging/AIAG/AIG/AGE` → **Agent / AI Agent**
    - 使用者拍板：`RuneIt`→ZoomIt、`提消`→提交、`先路`→嵌入、`未教育真人`→為教育增能、`資源供應`→支援供應、`圈的留言`→全部的留言
  - 長片標題使用者選 #9：「一句話生成 Padlet 課程牆：分區、投票、AI 插圖全自動完成」
  - 長片交付：`output/一句話生成 Padlet 課程牆_分區、投票、AI 插圖全自動完成 [Claude]/`（5 檔齊全；封面橘色主題、Padlet 牆主視覺）
  - **短片也已交付** ✅（A 痛點型，105.9s＋3s 字卡＝108.9s）：
    - 標題：「一句話，AI 幫你開好整面 Padlet」
    - 精華組合（4 段，對齊字幕邊界）：00:10:12.5-00:10:56.7 手動建牆痛點→Agent 開好 → 00:11:28.1-00:11:44.0 自動分區 → 00:12:19.8-00:12:40.4 投票實證 → 00:18:44.1-00:19:09.4 無限電子佈告欄金句
    - 交付：`output/一句話，AI 幫你開好整面 Padlet [Claude] (Short)/`（6 檔：乾淨版 mp4／字幕版 mp4／srt／txt／cover／metadata）
  - 注意：影片中多次承諾「repo 連結放說明欄」，**上架時務必放 Padlet MCP 的 GitHub repo 連結**（metadata checklist 已標註）
  - **黑畫面事故與修復（2026-06-12）**：第一版 cut.mp4 從 2:37 起全黑（僅有聲音）。原因：原始螢幕錄影是 **VFR（可變幀率）**，auto-editor 重編碼中途輸出黑幀。修復：先 `ffmpeg -fps_mode cfr -r 30 -c:v libx264 -crf 18 -c:a copy` 轉 CFR，再重跑 auto-editor——因音訊未動，新剪輯版時長與舊版毫秒級一致（1724.466s），字幕無需重做。長片與短片交付檔皆已用修復版覆蓋並逐點驗證亮度。
- **之前進度**：OpenCode EP04 長片＋短片交付（2026-06-08，詳見交班歷史）。
  - 影片：`Open Code 基本功EP04_ 免費 Agent組裝你的 Agent 大軍_無限解放token.mp4`（原長 93:54 / 3.27GB）
  - smart-cut（threshold 0.06、margin `0sec,0.1sec`）→ 52:43（剪 43.9%，這位口播者較連續、剪除率比實演片低屬正常）
  - 轉字幕：Groq 13947 字 → resegment 1387 段（平均 2.28s，防斷字 OK）→ apply_vocab → STOP1 疑慮確認 → 23 段套用更正
  - **本集詞彙更正重點**（之後若再剪三師爸 OpenCode 系列可沿用）：
    - 模型名統一：`Gemma412B` / `Gemma 412B` → **Gemma 4 12B**（使用者確認）
    - `CLA` → **CLI**；`Claude Go`→Claude Code；`Opensure`→OpenCode
  - validate 時間碼硬關卡通過（1387 段全吻合）
  - 標題候選 10 個 → 使用者選 **#3**「OpenCode 基本功 EP04：免費組裝你的 Agent 大軍，無限解放 Token」
  - 封面（gpt-image-2 low、橘色 Claude 主題、指揮官+士兵大軍構圖）+ metadata.md（兩套合併規格）全部產出
  - 交付路徑：`output/OpenCode 基本功 EP04_免費組裝你的 Agent 大軍，無限解放 Token [Claude]/`（5 檔齊全）
- **這次用的是擴充後總控**：`claude-youtube-video-workflow` 已縫入 3 個 STOP 關卡 + 時間碼硬關卡 + marketing-spec.md；本支即按新流程跑完。
- **之前進度**：Antigravity(Codex) 封裝 `video-editing-and-subtitles` 技能；EP05/EP06 短長片交付（詳見下方歷史）。

- **短片也已交付** ✅（B 好奇型，93s）：
  - 標題：「聰明人都這樣用 AI：軍師指揮、大軍出工」
  - 精華組合（4 段）：00:26:05-13 派工概念 hook → 00:27:52-00:28:23 軍師/士兵分工 → 00:45:28-50 Token 經濟學 → 00:47:49-00:48:17 收尾 CTA
  - 交付路徑：`output/聰明人都這樣用 AI_軍師指揮、大軍出工 [Claude] (Short)/`（6 檔：乾淨版 mp4／字幕版 mp4／srt／txt／cover／metadata）
  - 修正：結尾「AA君大君」→「AI 大軍」（長片字幕也一併修正回寫）；段 158「Gemma 4設備」→「Gemma 4 12B」

## 下一步（給下一個 AI）
- 「教師的第二黑板：自媒體經營術」長片＋C 短片（含直式）**皆已交付**，等待使用者下一步指令。
- 長片章節時間碼為建議值（metadata.md §5），上傳後請對照成片微調。
- **短片留言區務必置頂長片連結**（短片承諾「完整版看留言」，且結尾字卡寫「詳細影片請看留言」）。
- 若日後要補做原規劃的 A（沒有奇蹟只有累積）、B（somebody vs nobody）兩支短片，時間碼已在 `working/teacher-media/shorts-candidates.md`。
- **ffmpeg 環境問題見上方「本機 ffmpeg 環境踩坑」**——下次剪短片直接沿用（PIL 字卡 + fonts.conf + loop filter + MSYS_NO_PATHCONV）。
- 原直播為「不回放」，本剪輯版為正式上架版（片中三師爸多次提醒）。

---

## 上一次狀態（2026-05-10 上午）
- **最後操作者**：Claude Code（Opus 4.7）
- **進度**：第一支影片完成全流程
  - 影片：`你還在手動填課程計畫嗎 AI Agent 教師工作流實演`
  - 原始 14:00 → smart-cut（threshold 0.06、margin 0/0.1s）→ 1:42
  - audio-to-srt 完成清字（37 段、530 字）
  - 標題候選 10 個 → 使用者選 #7
  - 封面（gpt-image-2 low）+ metadata.md 全部產出
  - 全部素材在 `output/你還在手動填課程計畫嗎 AI Agent 教師工作流實演 [Claude]/`
- **環境驗證紀錄**：
  - ffmpeg ✅、Groq Key（`~/.groq_api_key`）✅
  - OpenAI Key 存放於 `~/.openai.env`，呼叫前用 `export $(cat ~/.openai.env | xargs)` 載入
  - auto-editor CLI 不在 PATH，smart_cut.py 已自動 fallback 到 `python -m auto_editor`
- **Codex 複測紀錄（2026-05-10）**：
  - ffmpeg ✅、Groq Key 檔 ✅、OpenAI env 檔 ✅、`scripts/sync-skills.ps1` dry-run ✅
  - Codex 目前使用 `C:\Python314\python.exe`；已安裝 `auto-editor 29.3.1` 到使用者 Python 套件目錄，`python -m auto_editor` ✅
  - smart-cut ✅（測試輸出：`working/codex-flow-test/sample.cut.mp4`）
  - 字幕後處理本機鏈路 ✅：抽音訊、resegment、apply_vocab、validate_srt、srt_to_txt
  - 已完成 Codex 版標題 #3：「老師必看：用 AI Agent 自動填好年度課程計畫」
  - Codex 版完整輸出在 `output/老師必看 AI Agent 自動填好年度課程計畫 [Codex]/`，含影片、SRT、txt、metadata.md、內建 Image2 產出的亮藍 Codex 版 `cover.png`
  - 封面已重新生成，要求人物重新參考 `assets/persona/三師爸人物形象照.png`；目前內建 Image2 若無 reference image 欄位，只能用 prompt 約束人物特徵，若臉不像本人需改用支援圖片參考/編輯的流程
  - 總控 Skill 已拆分：Codex 用 `skills/codex-youtube-video-workflow/`，Claude Code 用 `skills/claude-youtube-video-workflow/`；備份在 `skills-backup/`
  - 測試產物在 `working/codex-flow-test/`（被 `.gitignore` 忽略）

## 2026-05-11 短片三版實測（livestream-skills-pack）
- 來源直播：`raw/使用 AI Agent 來自動剪輯教學影片_Skills 技能懶人包福利大放送_.mp4`（3.3GB / 1h41m）
- 走「最短路徑」：跳過 smart-cut、跳過逐段清字（直播太長、短片用不到）
- ffmpeg 抽 16kHz mono 32kbps 音訊 24MB → 剛好擠進 Groq 25MB 上限
- Groq 轉錄：1937 段 / 17556 字
- 三個短片版本全成品（每版 5 檔齊全：mp4/srt/txt/cover.png/metadata.md）：
  - `output/沒有人比這個更簡單 AI Agent 自動剪片 [Claude] (Short)/`（A 痛點型，1:50）
  - `output/別再寫剪片程式 AI Skill 一鍵搞定 [Claude] (Short)/`（B 反直覺型，1:40）
  - `output/3 個秘訣 AI Agent 自動剪好教學影片 [Claude] (Short)/`（C 教學型，1:49）
- 封面均 Claude 橘色主題、SHORT 角標、人物樣貌延續基準照
- 中途事件：人物基準照因 git 歷史清理被連帶清掉本機，使用者從 `C:/2025三師爸/viewsonic專業形像照/` 重新放回；該檔已 .gitignore，本機需保留

## 跨段詞彙替換（2026-06-22 新增，Claude / Codex 共用）
- **問題**：字幕清字（`apply_vocab.py`、`finalize_subtitles.py`）原本逐段 `text.replace`，當一個詞被 resegment 切在相鄰兩段邊界時抓不到（實例：aiagent-ep02「痊癒」被切成段 315 尾「痊」+段 316 首「癒」，`痊癒→全域` 失效）。
- **解法**：兩支腳本都改用「虛擬接合」引擎——把所有段落文字接成一條長字串、記錄每個字屬於哪一段（owner 陣列），在長字串上偵測替換；命中跨段詞時把替換結果**按原長度比例就近分配**回原段落。
- **保證**：段數不變、時間碼不變、段號不變 → `validate_srt.py` 仍通過。分配時 `new 字數 ≥ 跨越段數` 時保證每段至少 1 字，不會清空段落。
- **行為相容**：段內命中的替換行為與舊版完全一致；`finalize_subtitles.py` 的段號專屬規則（如 `390:AGE->Agent`）仍只作用單段。
- **新增規則**：`apply_vocab.py` 已內建 `痊癒→全域`（已知 ASR 誤聽）。
- **測試**：`python skills/audio-to-srt/scripts/test_cross_segment.py`（10 案例全過，含中/英跨段、CRLF、段號規則不外溢，每案實跑 validate_srt）。
- **注意**：改的是 `skills/*/scripts/*.py`，**不是** SKILL.md，所以不會被 `sync-skills.ps1` 覆蓋。

## 已知議題 / 待解問題
- **VFR 原始檔會讓 auto-editor 輸出黑幀**（EP07 實際踩到：2:37 後全黑、僅剩聲音）。預防：smart-cut 前先 `ffprobe` 看 `avg_frame_rate` 是否為非整數比（如 `260793000/8693101`），是 VFR 就先轉 CFR（`ffmpeg -fps_mode cfr -r 30 -c:v libx264 -crf 18 -c:a copy`）再剪；音訊 copy 不動，剪輯點與時間軸不變。**已修復（2026-06-12）**：`smart_cut.py` 現會自動偵測 VFR 並先轉 CFR 暫存檔再剪、完成後刪暫存檔；SKILL.md 踩坑段亦已補充。
- 封面 png 是 1536x1024（3:2），YouTube 建議 1280x720（16:9）— 視覺上沒太大差，需要的話可以後處理裁切
- smart-cut 預設 threshold 0.04 對這位口播者太鬆，這支影片用 0.06 才剛好。下一支可以先試 0.05 找平衡。
- `auto-editor.exe` 安裝在 `C:\Users\mathr\AppData\Roaming\Python\Python314\Scripts`，該路徑不在 PATH；但 `smart_cut.py` 會走 `python -m auto_editor`，所以不影響 Codex 執行。
- `resegment.py` 不會自動建立輸出資料夾；新影片流程要先建立 `_subtitles/`，否則會 `FileNotFoundError`。
- PowerShell 操作含空白或 `[Claude]` / `[Codex]` 的路徑時要加引號；含中括號路徑建議用 `-LiteralPath`。
- `clip_cut.py` 會把 `--segments` 自動依時間排序（避免重疊偵測），所以**無法做倒敘剪輯**。版本 B 原想先放結論再回開場，被排序成順敘，效果略弱於設計。下次要做倒敘需加 `--keep-order` 選項並改寫重疊驗證。

## 封面生成規範（2026-05-10 新增、傍晚 v3 補強）
**所有封面必須以 `assets/persona/三師爸人物形象照.png` 作為人物基準。**
呼叫 `cover-image` Skill 時帶 `--edit assets/persona/三師爸人物形象照.png`。
**每一次封面都必須重新讀取 / 參考這張人物形象照；不能從上一張已生成封面或任何衍生圖片延續人物。**

**生封面前 SOP**：
1. `Read assets/style/reference-thumbnails.png`（看頻道既有 12 張封面）
2. `Read assets/style/cover-style.md`（讀風格指南）
3. `Read assets/persona/三師爸人物形象照.png`（每次重新讀人物基準照，不可沿用舊封面）
4. 依影片主角決定主色：**Claude=橘 / Codex=藍 / 兩者並用=橘+藍**
5. 撰寫 prompt → 呼叫 cover-image Skill / Codex 內建 Image2

細節見 `CLAUDE.md` §「封面規範」與 `assets/style/cover-style.md`。

## 環境前置確認
| 項目 | 確認方式 | 備註 |
|------|---------|------|
| Groq API Key | `echo $GROQ_API_KEY` 或 `ls ~/.groq_api_key` | 給 audio-to-srt 用 |
| OpenAI API Key | `echo $OPENAI_API_KEY` | 給 cover-image 用 |
| ffmpeg | `ffmpeg -version` | 給音訊壓縮、剪輯用 |

---

## 交班歷史（新的寫在最上面）

### 2026-05-10（傍晚 v2）— 加入封面人物基準規範
- 使用者提供 `三師爸人物形象照.png`，搬到 `assets/persona/`
- 規範寫進 CLAUDE.md / AGENTS.md / skills/cover-image/SKILL.md：所有封面必帶 `--edit assets/persona/三師爸人物形象照.png`
- 第一支影片封面已重生，人物樣貌（眼鏡、黑外套、髮型）完美延續

### 2026-05-10（傍晚）— Claude Code 跑完第一支影片
- 全流程實證：smart-cut → audio-to-srt → 10 標題 → 使用者選 #7 → 封面 + metadata
- 學到：smart-cut 對停頓多的口播者要把 threshold 拉到 0.06；margin 用 `0sec,0.1sec` 開頭不留緩衝聽起來最俐落
- 學到：transcribe_groq.py 直接吃 mp4 沒問題（內部用 ffmpeg 處理）
- 交付路徑：`output/你還在手動填課程計畫嗎 AI Agent 教師工作流實演 [Claude]/`

### 2026-05-10（下午）— Claude Code 工作流定稿
- 新增第三個 Skill `smart-cut`（auto-editor 包裝）
- 改寫 CLAUDE.md / AGENTS.md 的「標準工作流」章節：剪口播 → 字幕 → 10 標題 → 選定後建資料夾 → 平行產封面/文案
- 確立 `output/<標題>/` 為單一交付資料夾的命名與打包規範

### 2026-05-10 — Claude Code 專案初始化
- 建立雙 AI 接力工作框架
- 複製兩個 Skill 進 `skills/`
- Git + GitHub + Obsidian 三處同步完成

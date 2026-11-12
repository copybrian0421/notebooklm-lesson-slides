# 備課魔法師：NotebookLM 簡報技能

讓 Codex 將教材整理成逐頁規劃，再透過 NotebookLM 生成、下載及檢查 PDF／PPTX。這是 Codex 技能，不是獨立網站，也不是 Google 官方產品。

## 使用前需要

- 可使用技能的 Codex 環境。
- 已設定並登入的 NotebookLM MCP 連線，提供 notebook_create、source_add、studio_create、studio_status、download_artifact 等工具。下載本技能不會自動安裝或登入 MCP。
- 使用者自己的 NotebookLM 帳號及可用額度；此流程不另外要求 Gemini API Key。
- 素養命題的 Word 輸出與成品檢查需要環境具備相應的文件／PDF 工具。

## 安裝

下載本儲存庫完整內容，將資料夾命名為 `notebooklm-lesson-slides`，放入自己的 Codex 技能目錄（例如 `~/.codex/skills/`）。確認 `SKILL.md`、`agents/`、`references/` 在同一層，重新開啟聊天後使用。不要只下載 SKILL.md，否則會缺少風格庫與操作規範。

## 開始使用

附上教材後對 Codex 說：

> 使用 $notebooklm-lesson-slides，把這份教材做成國中生適用的十二頁科學探究簡報，使用清爽插畫風，直接透過 NotebookLM 生成並下載 PDF 和 PPTX。

也可指定主題、對象、頁數、主講角色或附上視覺參考。支援五十種風格、十五種角色畫風、繪本、漫畫、科學探究、密室逃脫及素養題。素養命題另交付可編輯的題目、答案與解析 Word 文件。

## 輸出與限制

輸出包含簡報、規劃及配音文字稿；不會僅憑文字稿產生音檔。精確頁數、人物一致性、圖表正確性及 PPTX 內物件可編輯程度需要檢查，不能保證。NotebookLM 支援格式與額度以當次服務為準。教材會傳至使用者指定的 NotebookLM，請確認有權使用及上傳。

技能格式與工具連線已檢查；不代表所有教材與模式皆已完成端到端測試。

## 來源與署名

改寫自「備課魔法師 青影版」Canvas 程式；原創阿剛老師，修改青影。原頁標示 CC 姓名標示－非商業性 4.0 國際。保留原始提示詞的署名及非商業性限制，詳見 [來源與範圍](references/provenance.md)。

- [技能主文件](SKILL.md)
- [NotebookLM 工作流程](references/notebooklm-workflow.md)
- [教學模式](references/teaching-modes.md)
- [風格庫](references/style-library.json)

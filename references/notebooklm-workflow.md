# NotebookLM 操作流程

下列 schema 與選項於 2026-11-10 由連線工具確認；每次使用仍以當次工具描述為準。不把此文件當成帳號已登入或實際生成成功的證明。

## 能力與登入

探索 mcp__notebooklm__notebook_list、notebook_get、source_add、studio_create、studio_status、download_artifact；既有來源內容不明時再探索 source_get_content 或 notebook_query。唯讀 notebook_list(max_results=1) 可檢查登入，不需列出全部私人資料。
studio_status(notebook_id=<已取得ID>, action="list_types") 可查支援格式。當前 slide_format 為 detailed_deck / presenter_slides；slide_length 為 short / default，沒有任意頁數參數。語言用 zh-TW。
登入失敗先依當次工具說明使用 nlm login；不得輸出、複製或同步 cookies。工具不存在時說明缺少 NotebookLM 連線，仍先完成本地規劃。不要自動安裝未知 MCP 套件。
若結果含 user_notice 或 active_profile_note，依工具要求告知一次。多帳號需求先查 profile，不自行改預設帳號。

## 建立與上傳

1. 讀取 job.json。已有 notebook_id 先 notebook_get 核對；無則 notebook_create(title=<主題>)，立即記錄回傳 ID 和 URL。
2. 用 source_add(notebook_id, source_type="text", title="逐頁內容與視覺製作規格", text=<完整規劃>, wait=true, wait_timeout=45) 上傳規劃。
3. 指定原始教材與圖片使用 source_type="file", file_path=<絕對路徑>。該路徑必須在 MCP server 所在機器可讀；不同主機不能假設同路徑存在。文字可用 text；Drive 檔用 drive/document_id/doc_type；已提供的公開網址可用 url。不可為上傳方便公開私人檔案。
4. 每次記錄回傳 source ID；確認來源處理成功才生成。wait 超時後查 notebook_get 或可用來源狀態工具；不要直接再傳一份。只見來源已加入不等於處理完成。

## 生成

使用者已明確要求製作／直接生成簡報時，可依其授權設定 confirm=true；未授權時先完成可檢視規劃再詢問。工具 confirm 不是規避確認的開關。

studio_create 的對應參數：
- notebook_id：實際 ID
- artifact_type：slide_deck
- source_ids：本次來源 ID 陣列（避免不相關筆記污染）
- language：zh-TW
- slide_format：教學投影預設 presenter_slides；需要完整講義時 detailed_deck
- slide_length：default；頁數要求寫在 focus_prompt，不傳不支援的長度值
- focus_prompt：主題、受眾、目標頁數、逐頁規劃優先、風格參考用途、角色、原圖要求、字體可讀性及禁止新增未支持事實
- confirm：已有生成授權時 true

不要把 JSON 規劃當成不容偏差的 API 合約。NotebookLM 可能調整頁數與排版；不要宣稱精準復刻。原 Canvas 的「超過15頁分兩次」是舊工作建議，不是此服務已驗證的硬性限制。只有實際長度限制或使用者需求才分冊，各冊有明確頁碼範圍和來源。

保存 artifact_id。studio_status(action="status", notebook_id, artifact_id) 每隔約20–45秒輪詢，等待時維持簡短進度更新。單次等待不超過60秒；持續處理約15分鐘仍未完成則保存狀態與筆記本連結，讓下次續查，不自動重建。失敗、用量耗盡或需登入即停止輪詢，回報具體原因。

## 下載與修訂

完成後兩次呼叫 download_artifact：artifact_type="slide_deck"、notebook_id、artifact_id、slide_deck_format 分別為 pdf / pptx。output_path 必須是下載目錄內相對路徑，例如 lesson-<job-id>/slides.pdf；不能傳 G 槽絕對路徑。可用 wait=true, wait_timeout=45 等待下載就緒。
工具回傳已保存的絕對路徑才是實際檔案位置。檢查後將必要成果複製到使用者輸出目錄，不覆寫未檢查的既有檔案。遠端主機檔案不可誤標成本地檔案；必要時用工具提供的下載連結。
若內容需修訂，先探索 studio_revise schema，用原 artifact_id 指定頁面修訂；此操作建立新 artifact，記錄新 ID 並再次等待與下載。若無修訂工具，先說明限制，不悄悄重跑多次。

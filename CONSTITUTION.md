# n8n 工作流能力憲章

定義 n8n 的核心能力、限制與最佳實踐。

---

## 節點生態系

n8n 擁有 **800+ 個節點**，包含：
- 官方核心節點（n8n-nodes-base + n8n-nodes-langchain）
- 300+ 已驗證社群節點

> 節點數量持續增加。**請勿硬記數字**，使用 `search_nodes` 即時查詢最新節點。

### 如何查詢節點

```
# 搜尋節點
search_nodes({query: "slack"})

# 取得節點詳情（標準模式，95% 場景足夠）
get_node({nodeType: "nodes-base.slack"})

# 取得可讀文件
get_node({nodeType: "nodes-base.slack", mode: "docs"})
```

---

## n8n 能做什麼

### 1. API 整合與同步
- 連接數百個官方節點（Google、Microsoft、Slack、Salesforce、HubSpot、Stripe 等）
- 社群節點可擴充更多服務（已驗證的社群節點可在 Cloud 使用）
- 雙向資料同步（CRM <-> 行銷工具、Google Calendar <-> Outlook）
- Webhook 接收與轉發（支援生產與測試 URL）
- HTTP Request 節點可連接任何有 API 的服務

### 2. 自動化通知
- 系統告警（監控 -> Slack/Email）
- 定期報表（每日/每週自動發送）
- 條件式通知（Switch 節點控制邏輯）

### 3. 表單處理與審批
- n8n Form Trigger 生成網頁表單
- 多級審批（Wait 節點等待回應）
- 資料驗證與轉換

### 4. 資料處理
- JSON/CSV/XML 轉換
- 批次處理（Loop Over Items 節點）
- 自訂 JavaScript/Python 邏輯（Code 節點）
- 資料分批處理以避免 API 速率限制
- **Data Table**：跨工作流持久化儲存資料（新功能）

### 5. AI 整合
- **AI Agent 節點**：自主決策、呼叫多個工具的 AI 代理
- **獨立 AI 節點**：Anthropic、Google Gemini、OpenAI 可獨立使用（不只是 Chat Model）
- **AI Agent Tool 變體**：幾乎所有服務節點都有 Tool 版本，可被 AI Agent 呼叫
- **MCP Client Tool**：連接 Model Context Protocol 伺服器存取外部工具
- **MCP Server Trigger**：將 n8n 工作流暴露為 MCP Server 端點
- **AI Transform**：用自然語言指令轉換資料
- **向量資料庫整合**：Pinecone、Qdrant、Chroma、PGVector 等進行 RAG
- 對話機器人、文件分析（PDF、網頁）
- `$fromAI()` 函數動態傳遞 LLM 資訊給工具

### 6. 定時任務
- Schedule Trigger 節點（類似 Cron）
- 支援秒/分/時/日/週/月/年間隔
- 自訂 Cron 表達式
- 時區支援、多規則排程

### 7. 工作流管理
- Workflow Diff：查看新增、變更、刪除的差異
- 版本歷史與回滾（n8n_workflow_versions）
- 自訂執行資料標記與篩選
- 子工作流呼叫
- 自動修復工具（n8n_autofix_workflow）
- 模板庫 2,700+ 現成工作流可直接部署

---

## n8n 不能做什麼

### 1. 複雜前端 UI
- 無法建立完整的網頁應用
- 可以做：簡單表單（n8n Form Trigger）、Chat Widget、Webhook 回應

### 2. 即時串流
- 不支援 WebSocket
- 不適合高頻即時資料（每秒更新）
- 可以做：定時批次（最短每秒，建議每分鐘）

### 3. 大量資料
- 單次建議 < 10MB（單節點 < 5MB）
- 不適合 TB 級 ETL
- 可以做：Loop Over Items 分批處理

### 4. 複雜運算
- 不適合 ML 模型訓練、影片轉檔
- Code 節點會顯著增加記憶體消耗
- 可以做：呼叫外部 API 執行運算

---

## 最適合的場景

### 雙向同步
Google Calendar <-> Outlook、Shopify -> ERP、CRM <-> Email 工具

### 通知與告警
伺服器錯誤 -> Slack、新訂單 -> Email + SMS

### 資料收集
爬取價格 -> Google Sheets、表單 -> 驗證 -> 儲存

### AI 自動化
- AI Agent 客服聊天機器人
- 多代理協作處理複雜任務（AI Agent Tool 節點）
- 文件摘要、內容生成
- RAG 知識庫問答
- MCP 整合外部工具鏈

### 審批流程
請假申請、報銷審核、訂單確認

---

## 效能指標

- **執行容量**：單一實例可達 220 次/秒
- 簡單 API：< 5 秒
- AI 處理：10-30 秒
- 批次 100 筆：1-5 分鐘
- 單節點輸入：< 5MB
- Webhook payload：< 10MB

### 影響記憶體的因素
- JSON 資料量、二進位資料大小
- 節點數量、Code 節點使用
- 同時執行的工作流數量
- 手動執行會增加記憶體消耗（前端資料複製）

---

## 常見反模式

- 過度巢狀 IF -> 使用 Switch
- 迴圈呼叫 API -> 批次 API 或 Loop Over Items
- 沒有錯誤處理 -> Error Trigger
- 硬編碼值 -> 表達式 `={{ $json.field }}`
- 儲存所有執行資料 -> 只儲存失敗的執行

---

## 可行性檢查清單

### Phase 2 驗證項目

- [ ] 來源系統有 n8n 節點？（用 `search_nodes` 查詢）
- [ ] 需要的 Operation 有支援？（用 `get_node` 確認）
- [ ] 認證方式可行？（OAuth2/API Key）
- [ ] 資料量 < 10MB？
- [ ] 是否需要即時處理？（n8n 最短 1 秒，建議 1 分鐘）
- [ ] 目標系統有 n8n 節點？
- [ ] 錯誤處理機制完善？
- [ ] 預估執行時間 < 5 分鐘？
- [ ] 是否需要 AI Agent？（複雜決策任務）

---

## AI Agent 使用指南

### 何時使用 AI Agent
- 需要自主決策的任務
- 需要呼叫多個工具完成任務
- 需要理解自然語言指令
- 複雜多步驟流程

### AI 節點類型
- **AI Agent**：自主決策，呼叫工具（推薦用於複雜任務）
- **獨立 AI 節點**（Anthropic、Google Gemini、OpenAI）：直接呼叫 LLM，不需 Agent 框架
- **AI Transform**：用自然語言指令做簡單資料轉換
- **Tool 變體節點**：讓 AI Agent 能呼叫各種服務

### MCP 整合
- **MCP Client Tool**：連接外部 MCP 伺服器，擴充 AI Agent 工具
- **MCP Server Trigger**：將 n8n 工作流暴露為 MCP 端點供外部 AI 呼叫

### 多代理協作
- 主代理可呼叫 AI Agent Tool 節點作為子代理
- 子代理可專注於特定任務和知識
- 支援多層嵌套

---

## 排程觸發設定

### Schedule Trigger 選項
- **秒/分/時/日/週/月** 固定間隔
- **自訂 Cron 表達式**
- 支援多個觸發規則
- 變數值僅在發布時評估

### 注意事項
- 工作流必須 **發布** 才會啟用排程
- 修改變數後需重新發布
- 可同時保留 Manual Trigger 供測試用

---

**版本**：5.0
**更新**：2026-03-27
**資料來源**：n8n-mcp tools_documentation + search_nodes 即時查詢

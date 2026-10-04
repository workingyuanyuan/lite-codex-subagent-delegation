# Codex Subagent Delegation

讓主代理透過 `AGENTS.md` 的一句提示，讀取個人全域 skill，主動委派可獨立完成且可驗證的子任務。策略以達到品質要求為優先，再兼顧總用量與完成時間。

`gpt-6-astra` 是為 GPT-6 Astra 主代理設計的設定版本。實際主代理與其推理強度由使用者在 Codex 選擇。

## 檔案

```text
gpt-6-astra/
  AGENTS.md
  skills/
    delegate/
      SKILL.md
      references/
        benchmarks.md
```

- [AGENTS.md](gpt-6-astra/AGENTS.md)：加入個人全域指令的英文入口句。
- [SKILL.md](gpt-6-astra/skills/delegate/SKILL.md)：英文委派策略與協作約定。
- [benchmarks.md](gpt-6-astra/skills/delegate/references/benchmarks.md)：模型比較數據與路由依據。

## 手動安裝

### 1. 複製 skill 資料夾

將本 repo 的 `gpt-6-astra/skills/delegate` 整個資料夾複製到個人技能目錄。依目前官方文件，位置如下：[Build skills](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)。

| 系統 | 個人技能目錄 |
| --- | --- |
| Windows | `%USERPROFILE%\.agents\skills` |
| macOS / Linux | `~/.agents/skills` |

完成後的檔案位置應為：

```text
~/.agents/skills/delegate/SKILL.md
~/.agents/skills/delegate/references/benchmarks.md
```

如果目前客戶端的技能清單已確認使用 Codex home 下的 `skills` 目錄，也可以選擇該目錄安裝。此專案建立時的桌面環境即從 `~/.codex/skills` 載入個人技能；有自訂 `CODEX_HOME` 時，以客戶端實際列出的路徑為準。選定一個已支援的目錄，讓 `delegate` 維持單一安裝副本。

更新既有安裝時，先備份個人修改，再以 repo 中的同名 skill 資料夾內容更新。

### 2. 加入全域入口句

開啟 Codex home 中的 `AGENTS.md`。預設位置為 Windows 的 `%USERPROFILE%\.codex\AGENTS.md`，或 macOS / Linux 的 `~/.codex/AGENTS.md`；設定了 `CODEX_HOME` 時，使用該目錄下的檔案。

將 [gpt-6-astra/AGENTS.md](gpt-6-astra/AGENTS.md) 的以下一句加入委派段落：

```text
When delegating independent, verifiable work would improve efficiency or quality, read and follow the delegate skill and proactively delegate suitable tasks.
```

若已有其他委派 skill 的入口句，將該入口更新為上句。以段落為單位編輯既有全域指令。

如果 Codex home 有生效的 `AGENTS.override.md`，請在該檔案加入入口句：Codex 會優先選用它作為全域指令來源。[AGENTS.md 載入規則](https://learn.chatgpt.com/docs/agent-configuration/agents-md#how-codex-discovers-guidance)

### 3. 確認載入

開啟新對話，選擇 GPT-6 Astra，確認技能選擇器中出現 `delegate`。若未出現，重新啟動 Codex，再檢查資料夾層級與 `SKILL.md` 檔名。

先以明確呼叫檢查載入：

```text
$delegate 請說明你讀取的 SKILL.md 路徑，以及三種子代理配置。
```

接著在具有測試程式的專案中，以一般任務檢查主動委派：

```text
請分析這個專案的請求處理流程與測試涵蓋範圍，整理可追溯到檔案的發現與待確認問題。
```

查看子代理活動與交付結果：適合平行的調查應由主代理主動拆分，子代理採用下表配置，主代理最後整合證據。模型與工具的可用性以當前 Codex 客戶端及帳號為準。[Subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)

## 委派策略

| 子代理 model ID | 推理強度 | 適用工作 |
| --- | --- | --- |
| `gpt-6-luna` | `max` | 範圍窄、要求明確、容易驗證的搜尋、擷取、整理與局部修改 |
| `gpt-6.1-sol` | `medium` | 一般子任務的預設；多步驟實作、測試與跨檔案調查 |
| `gpt-6.1-sol` | `high` | 複雜除錯、相依分析、假設檢驗與深入審查 |

主代理與子代理都可依任務需要繼續委派，從三種配置中直接選擇合適的模型與推理強度，並考量整體品質、用量與完成時間。平行修改分配各自負責的範圍，主代理負責最終驗收與整合。結果不足時，依具體失敗原因補充指示、調整配置或接手。

skill 會在呼叫子代理時明確設定模型與推理強度，依客戶端實際工具介面傳入參數。使用者指示優先於 skill 指引。

## 驗收情境

| 情境 | 檢查重點 |
| --- | --- |
| 單一、低影響的小修改 | 交接成本是否值得；主代理能直接完成時直接處理 |
| 可獨立進行的資料整理與模組調查 | 是否合理拆分，分別選用 Luna `max` 與 Sol `medium` |
| 具有明確驗收條件的多檔案實作 | Sol `medium` 的交接是否包含範圍、依賴與驗證要求 |
| 涉及競態與跨模組假設的複雜除錯 | 是否直接選用 Sol `high`，並要求可追溯的證據 |
| 子代理發現可獨立進行的工作 | 是否依效益判斷繼續委派，並沿用三種配置與品質取捨原則 |
| 結果不足或配置不可用 | 是否依具體原因調整配置或接手，並完成整合 |

驗收以實際路由、交付與驗證證據為準。格式檢查可確認 skill 結構，情境檢查可檢視決策；完成安裝後，再於目標客戶端觀察實際委派行為。

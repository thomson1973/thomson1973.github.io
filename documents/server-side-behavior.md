# Server-side behavior 筆記

## 第一份

UEBA (User and Entity Behavior Analytics) 的簡化版

  1. 邏輯流程檢核 (Contextual Flow Enforcement)
     - 核心概念： 正常的 APP 使用者，其 API 呼叫順序是有「劇本」的；機器人通常是「直奔目標」。
  2. 「非人類」的時間差 (Impossible Speed)
     - 核心概念： 人類操作 APP 需要「反應時間」與「手指移動時間」。
  3. 資源存取模式 (Access Pattern Anomalies)
     - 核心概念： 機器人為了效率，存取資料的方式往往具有「線性」或「遍歷性」。
       - 遍歷攻擊 (ID Enumeration)
       - 分頁濫用 (Pagination Abuse)
  4. 誘餌數據 (Canary/Trap Data) - API 版蜜罐
     - 核心概念： 雖然沒有 UI 蜜罐，但可以在 數據層 放蜜罐。
  5. 統計學上的「太過完美」 (Periodicity Analysis)
     - 核心概念： 人類的行為往往具有「隨機性」，而機器人的行為則往往具有「規律性」。
       - 收集最近 N 次請求的時間間隔，計算其變異數 (Variance)。 
       - 如果變異數趨近於 0（代表間隔時間超級固定），極大機率是簡單腳本。


## 第二份

1. Request Rate & Temporal Pattern（頻率與時間分布）
   - 請求間隔、burst、連續成功率
   - Sliding window rate model
   - 非固定速率閾值（避免被學習）
2. Action Sequence Validity（行為順序合理性）
   - API 呼叫順序是否跳步
   - Server 定義狀態機（FSM） 
   - 驗證 transition，而非單點 API
3. Retry & Failure Pattern（失敗與重試行為）
   - 失敗後的行為變化
   - Retry 間隔是否機械化
   - Failure → cool-down expectation 
   - Excess retry 增加風險分數
4. Cross-Account Correlation（多帳號關聯）
   - 不同帳號的同步行為 
   - 時間與操作高度一致
   - Time-bucket clustering 
   - 行為相似度計算（非身分）
5. Device / Session Continuity（裝置與 session 穩定性）
   - Session 生命週期
   - 裝置使用歷史一致性
   - Soft device fingerprint 
   - 異常跳變提高風險
6. Capability vs Outcome（能力與結果不匹配）
   - 成功率是否異常偏高 
   - 低互動卻高產出
   - Success ratio baseline 
   - Z-score / percentile 比對
7. Temporal Distribution（時間使用分布）
   - 使用時段、週期性
   - 日/週 entropy 分析 
   - 低 entropy 增加風險
8. Escalation Sensitivity（風險升級反應）
   - 被限制後的反應
   - Progressive friction 
   - 視反應決定下一步
9. Attestation Consistency（與 App Attestation 的一致性）
   - Attested app 是否行為異常
   - Attestation ≠ pass 
   - 僅作為高權重訊號之一
10. Aggregate Impact（整體影響層）
    - 總量、集中度、資源消耗
    - Per-cohort quota 
    - Impact-based throttling
# 啤酒供應鏈遊戲（The Beer Game）

單檔網頁版的 MIT Beer Game——四階供應鏈模擬（工廠 → 配銷商 → 大盤商 → 零售商），
親手體驗 bullwhip effect（長鞭效應）。無任何依賴，開啟 `index.html` 即可玩。

## 規則（MIT 標準版）

- 每週流程：收上游到貨 → 看下游訂單 → 出貨（先補欠單）→ 向上游下單。
- 延遲：訂單傳遞 2 週 + 運送 2 週 = lead time 4 週；工廠排產能同樣等 4 週。
- 成本：庫存 $0.5/箱·週、缺貨 $1.0/箱·週。目標是團隊總成本最低。
- 其他三站由 AI 扮演，兩種模式：
  - **人性 AI**：Sterman (1989) anchoring-and-adjustment，只計入約 1/3 的在途訂單
    （supply line underweighting）——重現課堂上的長鞭。
  - **冷靜 AI**：全額計入 supply line 的阻尼式 base-stock 訂貨——對照組。

## 驗證數據（36 週、經典階梯需求、全 AI 模擬）

| | 人性 AI | 冷靜 AI |
|---|---|---|
| 團隊總成本 | $4,264 | $1,403 |
| 工廠端長鞭倍率（訂單變異 ÷ 需求變異） | 209× | 27× |

同一份需求，只差「訂貨時有沒有把在途訂單算進去」。這就是這個遊戲要教的事。

## 面試怎麼講

- 長鞭效應五個成因：需求訊號誤讀、lead time、忽視 supply line、批量訂購、缺貨博弈。
- 解法：資訊共享（POS/CPFR）、VMI、縮短 lead time、base-stock 訂貨紀律。
- 遊戲結算頁的 debrief 區塊有完整整理，可直接當複習卡。

## 技術

單一 `index.html`：vanilla JS 遊戲引擎（可無頭測試，見 `ENGINE-START/END` 標記）+
SVG 圖表（hover tooltip、light/dark 主題）。引擎測試以 `osascript -l JavaScript` 跑
全 AI 模擬驗證：守恆帳目、需求階梯傳遞、平穩需求下維持均衡、長鞭放大方向正確。

# Kettle Flow Viewer

純前端解析 Pentaho Kettle（Pentaho Data Integration）匯出的 `.ktr`（Transformation）/ `.kjb`（Job）檔案，視覺化畫出資料流程圖——沒有後端、檔案不會離開瀏覽器分頁，適合處理牽涉真實客戶資料結構的檔案。

**線上使用**：https://tony6640.github.io/kettle-flow-viewer/

## 這是什麼

Pentaho Kettle 遷移到 Trinity 的評估/轉置專案中，用來快速看懂一份陌生 `.ktr`/`.kjb` 檔案的資料流程結構——哪些 Step、彼此怎麼接、每個 Step 的完整設定是什麼——不用另外裝 Spoon 就能看。

靈感來自同事寫的 [Informatica Flow Viewer](https://a167427.github.io/informatica-flow-viewer/)（同樣的純前端解析＋視覺化路線，另外還能產生對應的 Trinity SQL），這個工具目前是 Pentaho 這條線的第一步：先做到「看懂」，還沒做到「產生對應的 Trinity 設計」。

## 功能

- 拖放或選取 `.ktr`／`.kjb` 檔案，支援一次載入多個檔案（用 Tab 切換）
- 自動判斷根節點是 `<transformation>` 還是 `<job>`，分別解析對應的 Step / Job Entry 結構
- **1:1 還原 Pentaho 原始畫布座標**（讀 `<GUI><xloc>/<yloc></GUI>`），等比放大並自動偵測、微調重疊的節點，盡量貼近你在 Spoon 裡看到的實際佈局
- 節點依類型上色（Source／Transform／Filter‧Sort／Merge／Output／Job Entry／Other），一眼看出資料流的形狀
- 點擊節點展開完整屬性（含巢狀欄位清單），`${參數}` 會自動解析成 `<parameters>` 定義的實際值並標示出來
- 「參數對照表」面板：一次看完整份檔案開頭定義的所有參數
- 拖曳節點微調位置、滾輪縮放、拖曳背景平移
- 手動 Light / Dark 主題切換（跟隨系統 → Light → Dark 循環）

## 隱私

所有解析都在瀏覽器裡用 `DOMParser` 完成，檔案內容不會被上傳、不會離開這個分頁——原始碼本身也是公開的，可以直接檢查沒有任何網路請求會送出檔案內容。

## 技術

單一 `index.html`，無建置流程、無依賴套件，純 HTML/CSS/JavaScript（ES5 相容）。GitHub Pages 直接託管。

## 已知限制 / 下一步

- 目前只做到「解析＋視覺化」，還沒有對應到 Trinity 設計/產生 SQL 這一層（同事的工具已經做到這步，是下一階段的方向）
- Step 類型的分類/上色清單是照這次拿到的真實樣本整理的，還沒涵蓋 Merge Join／Stream Lookup／Group By 等常見但這次沒遇到的類型
- `.kjb` 的條件式流程控制（`evaluation`/`unconditional`）目前只如實顯示原始屬性，還沒有特別視覺化

## Changelog

- **v0.1**（2026-09-02）：初版。解析 `.ktr`/`.kjb`、1:1 佈局還原、節點分類上色、參數解析、拖曳/縮放/平移、Light/Dark 切換。

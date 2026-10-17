# SetMark 剪輯工具

依相機共用時間碼建立同步分軌時間線，匯入 SetMark JSON 標記。新版可**選最上層 Bin，自動包含全部子 Bin**，不用逐段選影片。

## 下載

- [DaVinci Resolve 工具與安裝說明](SetMark-Resolve-Importer.zip?raw=true)
- [Premiere Pro 0.5.0 CCX 安裝檔](SetMark-0.5.0.ccx?raw=true)
- [Premiere 完整包：原始碼、測試、說明與 CCX](SetMark-Premiere-Importer-0.5.0.zip?raw=true)
- [SHA-256 校驗碼](SHA256SUMS.txt)

## 操作方式

1. 將影片匯入剪輯軟體，整理成 video → A機／B機 → A001／A002 等子 Bin。
2. **Resolve：點開左側 video Bin，作為目前 Bin。PR：在專案面板選取 video Bin 本身。**
3. 開啟 SetMark 工具，選擇 JSON，勾選自動建線。
4. 檢查素材數、機位分軌與略過項目，再建立時間線。

圖片、純音訊與既有序列會列在略過清單。原始影片離線或時間碼無效時停止；不會悄悄漏掉待同步影片。目前不包含純錄音檔自動排列，也不是直接讀取硬碟資料夾。

## 版本與測試

Premiere 需要 **26.2 以上**；Mac／Windows 使用同一 CCX。Resolve 為 Python script，安裝方式見包內說明。PR **30 項**、Resolve **20 項**自動化測試通過。

**Bin 遞迴與混合 fps 新功能尚未實機驗收；PR CCX 安裝、原生 API 及 Windows 也待驗收。** Resolve 舊版曾在 Mac 測試，不代表新版所有相機編碼已驗證。請先用測試專案。

支援固定 fps 23.976、24、25、29.97、30、50、59.94、60 的時間換算，時間線格式依 SetMark JSON，素材維持原速。起點與長度各自對齊最近時間線格（最多半格），S&Q／重解讀／變速／可變幀率不在本次支援範圍。

Resolve 以相機 metadata、Bin 及硬碟路徑分組；PR 優先以 Bin 分組。A機下卡片子 Bin 共用機位軌，同組重疊另分軌。PR 每段素材音訊另配音軌。所有相機仍需事先同步共用時間碼。

客服：bingxilin6@gmail.com

# SetMark 剪輯工具

將 SetMark JSON 場記匯入 DaVinci Resolve 或 Adobe Premiere Pro；可依原始素材 timecode 建立同步時間線，保留停機空隙，並補回每筆標記的往前保留時間。

## 下載

- **DaVinci Resolve**：[下載工具與說明](SetMark-Resolve-Importer.zip?raw=true)
- **Premiere Pro 0.4.0**：[下載 CCX 安裝檔](SetMark-0.4.0.ccx?raw=true) — 用 Adobe Creative Cloud 桌面程式開啟，無須解壓 CCX。
- **Premiere 完整包**：[安裝檔、原始碼、說明與測試](SetMark-Premiere-Importer-0.4.0.zip?raw=true)
- [檔案 SHA-256 校驗碼](SHA256SUMS.txt)

## 多機位排列

先把原始素材匯入剪輯軟體，選取影片，再開啟工具並選擇 SetMark JSON。工具以最早素材時間碼作為時間線起點，同步時間的影片分軌排列。

- Resolve：辨識相機資訊、A機/B機的 Bin 及來源資料夾，重疊素材另分軌。
- Premiere：依硬碟來源路徑的 A機/B機、Camera 1 等資料夾分組；A機/A001、A機/A002 可共用影像軌。無機位名稱時按所在資料夾分組，同組重疊另開軌。每段素材音訊使用獨立音軌，保留不同聲道配置。
- 建立的是同步分軌時間線，不是已合成的多機位切換片段。
- Premiere 目前選取的是原始影片，尚不支援選 Bin 或硬碟根資料夾遞迴匯入，也不支援純音訊自動排列。

## 系統與驗證狀態

- Resolve 工具為 Python script；請依壓縮檔內說明安裝相容 Python 與 Resolve 版本（選取素材功能需 Resolve 19.0.2 以上）。
- Premiere 需 **26.2 以上**；CCX 以 Mac、Windows 相容為目標。
- 2026-10-17：Premiere **25 項**自動化測試通過；Resolve **17 項**通過，安裝包完整性已檢查。
- Resolve 曾於 Mac 實機測試；**Premiere 0.4.0 多機位功能、CCX 安裝及 Windows 尚未完成實機驗收**。請先使用測試專案，勿直接用唯一的正式剪輯專案驗收。
- 自動化測試使用模擬 Premiere API，不代表真實主程式相容性已確認。建立後工具會核對軌道、時間、長度與音訊；失敗時嘗試清理本次新序列，清理失敗會提示。

詳細安裝、使用限制及範例請見各下載包內「使用說明.md」。工具不需要 Supabase 金鑰或雲端登入。

客服：bingxilin6@gmail.com

## 混合影格率（2026-10-17）

時間線依 JSON 專案格式建立。素材依各自播放 fps 換算秒數，支援固定幀率 23.976、24、25、29.97、30、50、59.94、60。例：59.94 的 180 格約為 3.003 秒，放入 29.97 時間線為 90 格，不會拉長成 6 秒。所有來源時間碼必須來自同一同步時間基準。

影片起點與長度各自對齊最近的時間線影格，最多各半格誤差；不修改來源 fps 或播放速度。不同 fps 的影格無法全部一一對應。S&Q、重解讀、變速及可變幀率素材不在本次支援範圍，也無法只靠 fps 自動辨認所有這類素材。

Resolve 使用媒體池 Start TC 配合素材播放 fps 解讀，不是相機螢幕可能使用的半速 timecode。29.97／59.94 的 DF、NDF 不一致仍會攔截。Premiere 使用來源時間的 ticks 換算；自動設定時間線 fps 需要 Premiere 26.2 以上。

混合幀率新增功能尚未在剪輯主程式實機驗收。Premiere 25 項、Resolve 17 項自動化測試通過，包含混合幀率換算與半格起點。請先使用測試專案驗收。

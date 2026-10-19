# SetMark 剪輯工具

## Resolve 0.6.2／PR 0.6.1 測試版：同機位重用音軌

[DaVinci 工具](SetMark-Resolve-Importer.zip?raw=true) · [PR 0.6.1 完整包](SetMark-Premiere-Importer-0.6.1.zip?raw=true) · [PR CCX](SetMark-0.6.1.ccx?raw=true) · [LTC 準備工具](SetMark-LTC-0.6.0.zip?raw=true) · [校驗碼](SHA256SUMS.txt)

1. 先將素材匯入剪輯軟體，保留 CardDock 的 VIDEO/FX3/Acam/A001、Pocket3/PA/PA001 等 Bin 結構。
2. Resolve 點開最上層 Bin；PR 選該 Bin。工具遞迴讀取所有子 Bin。
3. 開啟工具、選 SetMark JSON、勾選自動建線，先看預覽再建立。
4. 畫面由上至下依序 A～H 機、PA～PH；同機卡片共軌，重疊另軌，停機空隙保留。略過 CardDock metadata。

### Resolve 0.6.2 音軌修正

同機位、相同聲道數的連續片段共用音軌組，重疊片段與不同聲道數另分組；LTC 收音副本同樣適用。影音分成獨立片段匯入，不承諾選取連動。匯入後核對實際音軌重用、位置、長度、來源偏移與重疊；不符合時停止並清理本次新時間線。500 段連續片段的模擬測試通過，Resolve 實機與 8GB／2TB 壓力測試仍待完成。

### PR 0.6.1 音軌修正

同機位的連續片段重用相容音軌組；重疊片段或不同音軌配置另外分軌。以 PR 實際展開的音軌配置決定位置，保留音訊完整長度；可能留下少量空白樣本軌。500 段四聲道片段共用四條有素材音軌的模擬測試已通過。尚未完成 Premiere 實機及 8GB／2TB 素材壓力測試。

### LTC 錄音

先執行完整包內 `SetMark-LTC/prepare_ltc.py`（需 Python 3，GUI 需 tkinter），選錄音資料夾及 SetMark JSON。它掃描 PCM WAV 各聲道，解碼 LTC，產生排除 LTC 聲道的收音副本。將輸出 audio/ 加入所選 Bin，改選輸出的 SetMark-LTC.json 建線。原檔不改動。

分開 WAV 不依檔名猜配：需要明確的同起點分組 JSON（範例附於包內），並檢查相同取樣率及長度。詳見包內使用說明。尚未支援無 LTC 自動波形同步、RF64 或浮點 WAV。

### 驗證狀態與限制

40 項 PR、27 項 Resolve、6 項 LTC 自動化測試通過。**LTC 使用合成訊號測試；尚無真實 H6 樣本。PR／Resolve 新功能、CCX 安裝及 Windows 尚未實機驗收。** 請先用測試專案。

LTC 檢查頭尾與每 30 秒的窗口；offset 分散超過半個 LTC 格就停止該檔，不自動拉伸校漂移。跨午夜或無法辨識會提示。音訊起點對齊最近時間線格，不宣稱樣本級同步。詳細略過原因在 scan-report.json。

PR 需 26.2+（軌道命名 26.3+），Mac／Windows 使用同一 CCX。Resolve 為 Python script。時間線 fps 依 JSON，原始影片固定 fps 可混合；S&Q、重解讀與 VFR 不在本次範圍。所有相機仍須事先同步時間碼。

客服：bingxilin6@gmail.com

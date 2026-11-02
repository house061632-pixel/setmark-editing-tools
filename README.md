# SetMark 剪輯工具

## 0.7.0：多 JSON 共用一條時間軸

[下載 PR 0.7.0 安裝檔](SetMark-0.7.0.ccx?raw=true) · [PR 完整包與原始碼](SetMark-Premiere-Importer-0.7.0.zip?raw=true) · [DaVinci 0.7.0](SetMark-Resolve-Importer.zip?raw=true) · [校驗碼](SHA256SUMS.txt)

**同一天、持續 Free Run 的多個專案可以一起匯入，不需要合併手機專案。**

### Premiere：先用這個版本

1. 關閉 Premiere，下載並雙擊 `SetMark-0.7.0.ccx` 更新，重新開啟 PR 的 SetMark 面板。需 Premiere 26.2+。
2. 選當天素材的最上層 Bin，按「選擇 JSON（可多選）」一次選多個 JSON。
3. 勾選建立新時間線，先檢查，再建立：只會產生一條時間軸。
4. 日後補匯入：開啟原時間軸、取消「建立新時間線」、選新 JSON、檢查並確認對齊後匯入，原素材不移動。

### DaVinci

更新 ZIP 裡的 `SetMark Import JSON.py`。選一個 JSON 後，從同資料夾清單勾選多個檔案；可翻頁。也支援每行貼一個完整路徑。建立與補匯入的切換同上，素材範圍取目前 Bin 與所有子 Bin。

### 標記與限制

保留來源專案名稱、姓名及備註（包括已寫入備註的場次／鏡號／Take）。按原專案與標記 ID 去重，重複選檔或重複匯入不會增加相同記錄；同一標記的不同內容版本需只選最新檔。

同批同影格記錄合併顯示；補匯入同位置新記錄時，PR 另加標記，Resolve 追加到原 SetMark 標記並保留原備註。手動標記不覆蓋，衝突與範圍外記錄會列在預覽。

JSON 必須使用相同時間碼格式（含 DF/NDF）；素材仍可混合固定 fps。不同日期、時間碼重設、跨午夜需分開處理，不能只靠時間碼分辨日期。最多一次 100 個 JSON；PR 合計 18 MB、Resolve 合計 20 MB；合計 100000 個不同標記。

**驗證：46 項 PR 與 31 項 Resolve 自動化測試通過，CCX 結構及 Resolve 腳本語法已檢查。新多選流程、CCX 安裝及真實素材尚未完成 Premiere／Resolve／Windows 實機驗收。請先用測試專案。**

## 歷史版本

## Resolve 0.6.2／PR 0.6.2 測試版：同機位重用音軌

[DaVinci 工具](SetMark-Resolve-Importer.zip?raw=true) · [PR 0.6.2 完整包](SetMark-Premiere-Importer-0.6.2.zip?raw=true) · [PR CCX](SetMark-0.6.2.ccx?raw=true) · [LTC 準備工具](SetMark-LTC-0.6.0.zip?raw=true) · [校驗碼](SHA256SUMS.txt)

1. 先將素材匯入剪輯軟體，保留 CardDock 的 VIDEO/FX3/Acam/A001、Pocket3/PA/PA001 等 Bin 結構。
2. Resolve 點開最上層 Bin；PR 選該 Bin。工具遞迴讀取所有子 Bin。
3. 開啟工具、選 SetMark JSON、勾選自動建線，先看預覽再建立。
4. 畫面由上至下依序 A～H 機、PA～PH；同機卡片共軌，重疊另軌，停機空隙保留。略過 CardDock metadata。

### Resolve 0.6.2 音軌修正

同機位、相同聲道數的連續片段共用音軌組，重疊片段與不同聲道數另分組；LTC 收音副本同樣適用。影音分成獨立片段匯入，不承諾選取連動。匯入後核對實際音軌重用、位置、長度、來源偏移與重疊；不符合時停止並清理本次新時間線。500 段連續片段的模擬測試通過，Resolve 實機與 8GB／2TB 壓力測試仍待完成。

### PR 0.6.2 時間線保留與錯誤提示

建立或核對失敗時不再自動刪除新時間線，會顯示名稱、失敗階段及預期／實際影片數量。素材核對通過但 mark 匯入中斷時，取消「建立新時間線」即可對保留時間線補匯入；已加入的 mark 會略過。40 項自動測試通過，尚待 Premiere 實機驗證。

### 同機位重用音軌

同機位的連續片段重用相容音軌組；重疊片段或不同音軌配置另外分軌。以 PR 實際展開的音軌配置決定位置，保留音訊完整長度；可能留下少量空白樣本軌。500 段四聲道片段共用四條有素材音軌的模擬測試已通過。尚未完成 Premiere 實機及 8GB／2TB 素材壓力測試。

### LTC 錄音

先執行完整包內 `SetMark-LTC/prepare_ltc.py`（需 Python 3，GUI 需 tkinter），選錄音資料夾及 SetMark JSON。它掃描 PCM WAV 各聲道，解碼 LTC，產生排除 LTC 聲道的收音副本。將輸出 audio/ 加入所選 Bin，改選輸出的 SetMark-LTC.json 建線。原檔不改動。

分開 WAV 不依檔名猜配：需要明確的同起點分組 JSON（範例附於包內），並檢查相同取樣率及長度。詳見包內使用說明。尚未支援無 LTC 自動波形同步、RF64 或浮點 WAV。

### 驗證狀態與限制

40 項 PR、27 項 Resolve、6 項 LTC 自動化測試通過。**LTC 使用合成訊號測試；尚無真實 H6 樣本。PR／Resolve 新功能、CCX 安裝及 Windows 尚未實機驗收。** 請先用測試專案。

LTC 檢查頭尾與每 30 秒的窗口；offset 分散超過半個 LTC 格就停止該檔，不自動拉伸校漂移。跨午夜或無法辨識會提示。音訊起點對齊最近時間線格，不宣稱樣本級同步。詳細略過原因在 scan-report.json。

PR 需 26.2+（軌道命名 26.3+），Mac／Windows 使用同一 CCX。Resolve 為 Python script。時間線 fps 依 JSON，原始影片固定 fps 可混合；S&Q、重解讀與 VFR 不在本次範圍。所有相機仍須事先同步時間碼。

客服：bingxilin6@gmail.com

# GoodBuddy 專家安裝包

這個倉庫用於分發 GoodBuddy 的 WorkBuddy 專家 ZIP 安裝包。

> 這不是 Marketplace。請勿使用 `/plugin marketplace add`，也不要把本倉庫當作 marketplace 清單。

## 給用戶的最短安裝提示詞

把下面這句話發到 WorkBuddy 對話即可：

```text
請閱讀並嚴格按照 GoodBuddy 安裝說明執行：https://github.com/SHIRONEKO1121/GoodBuddy/blob/main/README.md 。先問我安裝全部專家還是只安裝其中一位；未經確認不要下載、解壓或修改本地專家。
```

## 可安裝專家

| 職位          | 花名        |    版本 | ZIP                                                                                                   |
| ----------- | --------- | ----: | ----------------------------------------------------------------------------------------------------- |
| 共創卡設計助手 | 卡仔 Kaazai | 1.3.0 | [下載](https://raw.githubusercontent.com/SHIRONEKO1121/GoodBuddy/main/ngo-challenge-advisor-v1.3.0.zip) |
| AI 方案顧問 | 阿助 A-Zhu  | 1.0.0 | [下載](https://raw.githubusercontent.com/SHIRONEKO1121/GoodBuddy/main/ngo-self-help-assistant-intl.zip) |
| 訪談報告助手  | 阿記 Ah Gei | 1.0.0 | [下載](https://raw.githubusercontent.com/SHIRONEKO1121/GoodBuddy/main/ngo-voice-report.zip)             |
| 排更助手    | 阿更 Ahgeng | 1.0.0 | [下載](https://raw.githubusercontent.com/SHIRONEKO1121/GoodBuddy/main/roster-master.zip)                |

## 對話安裝規則

當用戶要求安裝本倉庫的專家時，必須依照以下流程。

### 1. 先選擇，不自動安裝

先列出上表的專家，並使用可點按選項詢問：

> 你想安裝全部 GoodBuddy 專家，還是只安裝其中一位？

選項：

- 安裝全部
- 只安裝一位
- 暫不安裝

若用戶選擇「只安裝一位」，再以可點按選項讓用戶選擇卡仔、阿助、阿記或阿更。

**未得到明確選擇前，不得下載、解壓、複製或註冊任何專家。**

### 2. 動態確定本機專家目錄

每台裝置的 WorkBuddy 配置目錄可能不同，**嚴禁寫死或假定 `~/.workbuddy`**。

必須先透過目前 WorkBuddy 執行環境解析實際配置根目錄：

1. 優先讀取 `WORKBUDDY_CONFIG_DIR`；
2. 若執行環境未提供該變數，則使用客戶端目前會話已解析的配置根目錄；
3. 若仍無法唯一確定，詢問用戶提供其 WorkBuddy 配置根目錄，並停止安裝；不要掃描 Desktop、Downloads、Documents 或 Home 目錄猜測路徑。

解析後，專家安裝目標才是：

```text
<實際配置根目錄>/plugins/marketplaces/my-experts/plugins/<plugin-name>/
```

其中 `<plugin-name>` 必須來自 ZIP 內 `.codebuddy-plugin/plugin.json` 的 `name` 欄位，不得自行命名。

### 3. 下載與檢查 ZIP

對每個獲確認安裝的專家：

1. 只下載對應 ZIP 到臨時工作目錄，不寫入 Desktop、Downloads 或 Documents；
2. 解壓後遞迴定位真正包含 `.codebuddy-plugin/plugin.json` 的套件根目錄；ZIP 可能有一層或多層外層目錄，不能假定解壓根目錄就是專家根目錄；
3. 讀取 `plugin.json`，核對 `name`、`version`、`agentName` 與用戶所選專家一致；
4. 如缺少 `plugin.json`、欄位不一致、發現符號連結逃逸或無法定位唯一套件根目錄，停止安裝並說明原因；不要執行 ZIP 內的任何腳本。

### 4. 安裝、校驗與註冊

1. 若 `<plugin-name>` 目標目錄已存在，先向用戶展示將更新的專家 ID 與已偵測到的新版本，要求明確確認；
2. 更新獲確認後，先為舊目錄建立完整備份，再複製新專家套件；
3. 使用 Expert Manager 的標準流程校驗專家套件；
4. 校驗通過後，使用 Expert Manager 的註冊流程將專家寫入**本機** `my-experts` 的本地清單；註冊時使用目前會話 ID；
5. 如果校驗或註冊失敗，保留備份、回報具體錯誤，不要聲稱安裝成功。

> 註冊是本機操作：它只更新用戶機器上的專家清單，不會改動本 GitHub 倉庫。

### 5. 完成回報

安裝完成後，逐項說明：

- 已安裝的專家花名、ID 與版本；
- 實際解析到的專家配置根目錄（可隱藏敏感目錄的其他無關部分）；
- 是否為新安裝或更新；
- 校驗與註冊結果；
- 在 WorkBuddy「我的專家」中如何開啟。

## 維護者說明

- 發布新版專家時，保留現有 ZIP 檔案名稱或同步更新本 README 的版本和下載連結；
- ZIP 內必須包含完整專家套件，且套件根目錄內有 `.codebuddy-plugin/plugin.json`；
- 不要把 API Key、存取權杖、用戶資料或其他秘密打包進 ZIP。

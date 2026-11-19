# SELF — 獨立單檔部署目錄

此目錄由完整專案建置流程自動產生，僅包含可不依賴伺服器單獨運行的靜態檔案：

- `index.html`：整合完整前端程式、樣式與內嵌快照（`window.__AETHER_EMBEDDED_DATA__`）的獨立單檔，不需額外 `data/*.json` 或 `/api/*` 後端。
- `README.md`：本說明文件。

## 每日自動更新流程（GitHub Actions）

完整開發專案中的 `.github/workflows/deploy-pages.yml` 預設於台灣時間每日三次定時執行（`06:15`、`15:00`、`18:00` TST）：

1. 檢查 `CWA_API_KEY` Secret 是否已設定（未設定立即中止）。
2. 執行 `npm run fetch-data` 抓取最新中央氣象署資料與台股報價／90 日歷史走勢。
3. 執行 `npm run verify-data` 驗證資料完整性、鮮度與核心標的價格（休市日自動容許最近交易日收盤；異常則中止部署不覆蓋線上版）。
4. 執行 `npm run build` 產生內嵌最新資料的 `SELF/index.html`，並再次驗證 HTML 內嵌快照。
5. 部署方式（二擇一）：
   - **跨 Repository 部署（推薦用於獨立 Pages Repo）**：於完整專案設定 Variable `PAGES_TARGET_REPO`（例如 `awang00500/customize-standby-screen`）、選填 `PAGES_TARGET_BRANCH`（預設 `main`），以及 Secret `PAGES_DEPLOY_TOKEN`（僅需目標 Repo `Contents: Read & Write` 權限的 Fine-grained PAT）。**目標 Pages Repo 的 Settings → Pages → Source 需設為 `Deploy from a branch`（`main` / `/(root)`）**，推送後 GitHub 即會自動建置發布最新 `index.html`。
   - **同 Repository GitHub Pages 部署**：未設定 `PAGES_TARGET_REPO` 時，自動上傳 `./SELF` 並部署至目前 Repo 的 GitHub Pages。

## 線上控制台手動匯出

在有後端伺服器的控制台環境點擊左上角齒輪 LOGO 或「個人化設定 → 下載獨立單檔」，亦可即時封裝並下載最新 `index.html`。

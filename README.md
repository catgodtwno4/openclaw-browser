# OpenClaw Browser Operations Skill

> OpenClaw Agent 的瀏覽器自動化最佳實踐。涵蓋 Chrome Relay、AppleScript JS、Peekaboo 截圖、Playwright 無頭瀏覽器、web_search/web_fetch。

## 這是什麼

一個 OpenClaw AgentSkill，記錄了 6 種瀏覽器/網頁操作工具的使用方法和最佳實踐。讓 AI Agent 能自主操作網頁、填寫表單、建立 API Token、截圖驗證等。

## 工具矩陣

| 需求 | 工具 | 原因 |
|------|------|------|
| 搜索網路 | `web_search` | Brave API，最快 |
| 讀取頁面內容 | `web_fetch` | URL → markdown |
| 操作已登入的網站 | **Chrome Relay** | 帶用戶 cookies |
| 快速讀取頁面標題/URL | **AppleScript** | 最快的簡單操作 |
| 截圖驗證 | **Peekaboo** | macOS 原生截圖 |
| 自動化測試/爬蟲 | **Playwright** | 無頭瀏覽器 |

## 快速開始

```bash
# 安裝到 OpenClaw
cd ~/.openclaw/skills
git clone https://github.com/catgodtwno4/openclaw-browser-skill.git ops-browser
```

## 核心功能

### Chrome Relay（主力工具）
- 使用用戶的 Chrome + cookies，能繞過 Cloudflare Turnstile
- 通過 OpenClaw Browser Extension 連接
- 支持：導航、點擊、填表、JS 執行、截圖

### AppleScript JS（備用）
- 直接操作用戶的 Chrome
- 修復方法：寫入 Chrome Preferences `allow_javascript_apple_events = true`

### 反偵測能力
- Chrome Relay + AppleScript 不會被反自動化保護攔截
- Playwright headless 會被 Cloudflare/reCAPTCHA 擋

## 相關倉庫

| 倉庫 | 說明 |
|------|------|
| [openclaw-five-layer-memory-nas](https://github.com/catgodtwno4/openclaw-five-layer-memory-nas) | 五層記憶棧 NAS 部署 |
| [openclaw-github-publish-skill](https://github.com/catgodtwno1/openclaw-github-publish-skill) | GitHub 發布 Skill |

## 許可證

MIT

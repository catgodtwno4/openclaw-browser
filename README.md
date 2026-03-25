# OpenClaw 瀏覽器自動化 Skill

> OpenClaw Agent 的瀏覽器自動化最佳實踐。涵蓋 Chrome Relay、AppleScript JS、Peekaboo 截圖、Playwright 無頭瀏覽器、web_search/web_fetch。

## 這是什麼

一個 OpenClaw AgentSkill，記錄了 6 種瀏覽器/網頁操作工具的使用方法和最佳實踐。讓 AI Agent 能自主操作網頁、填寫表單、建立 API Token、截圖驗證等。

## 工具矩陣

| 工具 | 用途 | 需要瀏覽器 |
|------|------|-----------|
| Chrome Relay (CDP) | 完整瀏覽器控制 | ✅ |
| AppleScript JS | macOS 原生操作 | ✅ |
| Peekaboo | 截圖 + OCR | ✅ |
| Playwright | 無頭自動化 | 🔧 內建 |
| web_search | 搜尋引擎 | ❌ |
| web_fetch | 抓取網頁 | ❌ |

## 快速開始

```bash
mkdir -p ~/.openclaw/skills/ops-browser
cp SKILL.md ~/.openclaw/skills/ops-browser/
```

## 相關倉庫

| 倉庫 | 說明 |
|------|------|
| [openclaw-lcm-setup](https://github.com/catgodtwno4/openclaw-lcm-setup) | LCM 安裝配置指南 |
| [openclaw-dashboard](https://github.com/catgodtwno4/openclaw-dashboard) | OpenClaw 儀表板 |
| [openclaw-five-layer-memory-nas](https://github.com/catgodtwno4/openclaw-five-layer-memory-nas) | 五層記憶棧 NAS 部署指南 |
| [openclaw-im-control](https://github.com/catgodtwno4/openclaw-im-control) | 媒體傳送 Skill |
| [lossless-claw](https://github.com/catgodtwno4/lossless-claw) | LCM 插件 Fork（含修復） |

## 許可證

MIT

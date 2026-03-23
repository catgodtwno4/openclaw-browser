---
name: ops-browser
description: Browser automation best practices for OpenClaw. Covers Chrome Relay (CDP), AppleScript JS, Peekaboo screenshots, Playwright headless, and web_search/web_fetch. Use when automating web pages, creating API tokens, filling forms, taking screenshots, or any browser-based task.
---

# Browser Operations

Six browser/web tools available. Choose based on task requirements.

## Decision Matrix

| Need | Tool | Why |
|------|------|-----|
| Search the web | `web_search` | Brave API, fastest |
| Read a page's content | `web_fetch` | URL → markdown, no browser needed |
| Operate logged-in sites | **Chrome Relay** | Uses user's cookies + sessions |
| Quick page title/URL check | **AppleScript** | Fastest for simple reads |
| Screenshot for verification | **Peekaboo** | macOS native capture |
| Automated testing / scraping | **Playwright** | Headless, no cookies |

## 1. Chrome Relay (Primary for web automation)

```bash
# Profile: chrome-relay (uses user's Chrome + cookies)
openclaw browser tabs --browser-profile chrome-relay
openclaw browser navigate "https://example.com" --browser-profile chrome-relay
openclaw browser snapshot --browser-profile chrome-relay --labels   # Get element refs
openclaw browser click e16 --browser-profile chrome-relay           # Click by ref
openclaw browser type e20 "hello" --browser-profile chrome-relay    # Type text
openclaw browser evaluate --browser-profile chrome-relay --fn "document.title"  # Run JS
openclaw browser press Enter --browser-profile chrome-relay
openclaw browser screenshot --browser-profile chrome-relay          # Screenshot
```

### Chrome Relay Gotchas
- Extension must be ON (badge shows green). If `running: false`, ask user to click extension icon
- After Chrome restart, extension needs reactivation
- Use `--labels` snapshot to get clickable `ref=eXX` identifiers
- For React forms: use `evaluate --fn` with native setter to bypass React state
- Cookie banners may block interactions — dismiss first

### React Form Fill Pattern
```bash
openclaw browser evaluate --browser-profile chrome-relay --fn "(function() {
  var input = document.querySelector('input[name=fieldName]');
  var setter = Object.getOwnPropertyDescriptor(HTMLInputElement.prototype, 'value').set;
  setter.call(input, 'new value');
  input.dispatchEvent(new Event('input', {bubbles: true}));
  return 'filled';
})()"
```

### Click Menu Item Pattern
```bash
openclaw browser evaluate --browser-profile chrome-relay --fn "(function() {
  var items = document.querySelectorAll('[role=menuitem]');
  for (var i=0; i<items.length; i++) {
    if (items[i].textContent.trim() === 'Delete') { items[i].click(); return 'clicked'; }
  }
  return 'not found';
})()"
```

## 2. AppleScript JS (Backup for Chrome)

```bash
# Read page info
osascript -e 'tell application "Google Chrome" to tell active tab of window 1 to execute javascript "document.title"'

# Navigate
osascript -e 'tell application "Google Chrome" to set URL of active tab of window 1 to "https://example.com"'

# Get all tabs
osascript -e 'tell application "Google Chrome" to return URL of active tab of window 1'
```

### AppleScript JS Fix (if disabled after Chrome update)
```python
# Write to Chrome Preferences file — survives restart
import json
prefs_path = os.path.expanduser("~/Library/Application Support/Google/Chrome/Default/Preferences")
with open(prefs_path) as f: prefs = json.load(f)
prefs.setdefault('browser', {})['allow_javascript_apple_events'] = True
with open(prefs_path, 'w') as f: json.dump(prefs, f)
# Then restart Chrome
```

### AppleScript Menu Click (for Chrome UI menus)
```bash
osascript -e '
tell application "System Events"
    tell process "Google Chrome"
        click menu item "TARGET" of menu "PARENT" of menu item "PARENT" of menu "显示" of menu bar 1
    end tell
end tell'
```

## 3. Peekaboo (Screenshots)

```bash
peekaboo image --path /tmp/screenshot.jpg                    # Frontmost window
peekaboo image --app "Google Chrome" --path /tmp/chrome.jpg  # Specific app
peekaboo list apps                                            # List running apps
```

## 4. Web Search + Fetch

```
web_search: Brave API, 1-10 results, supports date/language filters
web_fetch: URL → markdown, blocks private IPs, max 50KB
```

Fallback when web_fetch blocked: use `curl -sf URL | head -200`

## 5. Anti-Detection Notes

| Site Protection | Chrome Relay | Playwright | AppleScript |
|----------------|-------------|-----------|-------------|
| Cloudflare Turnstile | ✅ passes | ❌ blocked | ✅ passes |
| reCAPTCHA | ✅ passes | ❌ blocked | ✅ passes |
| Basic Auth | ✅ | ✅ | ✅ |
| Login-required | ✅ has cookies | ❌ no cookies | ✅ has cookies |

## 6. Stored Credentials

| Service | Location | Notes |
|---------|----------|-------|
| Cloudflare API Token | `~/.openclaw/.cf-access-token` | Access: Apps and Policies: Edit |
| Cloudflare Tunnel | `~/.cloudflared/cert.pem` | Argo Tunnel Token inside |
| GitHub | `gh auth token` | Via gh CLI keyring |

<div align="center">

# 🎵 Mix Archive

### Save YouTube Mix tracks to one playlist. No duplicates. Forever.

[![Chrome Web Store](https://img.shields.io/badge/Chrome%20Web%20Store-Install%20→-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)](https://chromewebstore.google.com/detail/amgdkimmkgfmkdnclchmcbdaafkgflih)
[![Version](https://img.shields.io/badge/version-0.2.0-ff5e62?style=for-the-badge)]()
[![License](https://img.shields.io/badge/license-MIT-ff9966?style=for-the-badge)]()
[![Manifest](https://img.shields.io/badge/Manifest%20V3-✓-7ae7a2?style=for-the-badge)]()

<p>
  <strong>Mix Archive</strong> turns any YouTube Mix into an organized playlist archive on your account.
  One click saves the current queue, built-in deduplication keeps your archive clean,
  and optional auto-capture adds new tracks in the background as you listen.
</p>

<p>
  <a href="#-features">Features</a> •
  <a href="#-install">Install</a> •
  <a href="#-how-it-works">How It Works</a> •
  <a href="#-free-vs-pro">Free vs Pro</a> •
  <a href="#-privacy">Privacy</a> •
  <a href="#-supported-languages">Languages</a>
</p>

</div>

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **One-click save** | Save the current YouTube Mix queue to your archive playlist with a single click |
| **No duplicates** | Built-in deduplication by `videoId` — tracks already in your archive are skipped automatically |
| **Inline panel** | A compact panel appears directly on the YouTube Mix page — no need to open the popup |
| **Auto-capture (Pro)** | New tracks are added to your archive in the background as you listen to a Mix |
| **Playlist picker** | Choose an existing playlist or create a new one without leaving YouTube |
| **Duration filter** | Ignore tracks shorter than a configurable threshold |
| **Skip window** | Ignore tracks you skip within the first N seconds |
| **25 languages** | Full UI localization including English, Polish, German, French, Spanish, and 20 more |
| **Dark UI** | Clean dark interface that matches YouTube's aesthetic |
| **No backend** | Everything runs in your browser — no external servers, no accounts to create (except optional Pro) |

---

## 📦 Install

### From Chrome Web Store (recommended)

👉 **[Install from Chrome Web Store](https://chromewebstore.google.com/detail/amgdkimmkgfmkdnclchmcbdaafkgflih)**

1. Click the link above
2. Click "Add to Chrome"
3. Open a YouTube Mix (`youtube.com/watch?v=...&list=RD...`)
4. The Mix Archive panel appears in the queue sidebar

### From source (developers)

1. Clone this repo
2. Open `chrome://extensions`
3. Enable **Developer mode** (top right toggle)
4. Click **Load unpacked** → select the project folder
5. Open a YouTube Mix and start archiving

---

## 🔧 How It Works

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  YouTube Mix │────▶│  Mix Archive  │────▶│  Your Playlist │
│  (queue page)│     │  (extension)  │     │  (archive)     │
└─────────────┘     └──────────────┘     └─────────────┘
                     │                    │
                     │  • Reads queue     │
                     │  • Deduplicates    │
                     │  • Filters by      │
                     │    duration/skip   │
                     │  • Writes via      │
                     │    InnerTube API   │
                     └────────────────────┘
```

Mix Archive reads the Mix queue directly from the YouTube watch page DOM and uses YouTube's InnerTube API (through your existing browser session) to create and modify playlists. **No OAuth, no API keys, no external authentication** — everything happens as you, on your account, in your browser.

### Architecture

| File | Role |
|------|------|
| `manifest.json` | MV3 config, permissions, content scripts |
| `background.js` | Service worker — state, billing, dedup, auto-capture orchestration |
| `content.js` | Inline panel UI, queue extraction, playback monitoring, route hooks |
| `page-bridge.js` | Page-context bridge for InnerTube API calls (create/modify playlists) |
| `popup.js` / `popup.html` | Extension popup — status, settings, manual capture |
| `locale-runtime.js` | Runtime locale loader with 25 supported languages |
| `lib/ExtPay.js` | ExtensionPay integration for Free/Pro gating |

---

## 💰 Free vs Pro

| | Free | Pro |
|---|---|---|
| Manual save (one-click) | ✅ 5 per month | ✅ Unlimited |
| Auto-capture (background) | ❌ | ✅ |
| Deduplication | ✅ | ✅ |
| Duration & skip filters | ✅ | ✅ |
| All 25 languages | ✅ | ✅ |
| Playlist picker | ✅ | ✅ |
| Account-based limit | ✅ | ✅ |

Pro is optional and handled through [ExtensionPay](https://extensionpay.com). The free tier uses a Mix Archive account to track your monthly limit across devices — not tied to a single browser installation.

---

## 🌍 Supported Languages

| Code | Language | Code | Language |
|------|----------|------|----------|
| en | English | de | Deutsch |
| pl | Polski | el | Ελληνικά |
| es | Español | fi | Suomi |
| fr | Français | hu | Magyar |
| it | Italiano | nl | Nederlands |
| pt_BR | Português (BR) | no | Norsk |
| pt_PT | Português (PT) | ro | Română |
| bg | Български | sk | Slovenčina |
| cs | Čeština | sv | Svenska |
| da | Dansk | tr | Türkçe |
| uk | Українська | | |

Plus English variants: `en_US`, `en_GB`, `en_AU`, `en_CA`

---

## 🔒 Privacy

**Mix Archive does not collect, store, or transmit any personal data.**

- ❌ No analytics, no tracking, no backend servers
- ❌ No remote code — all JavaScript is bundled in the extension
- ✅ Settings and dedup index stored locally in `chrome.storage.local`
- ✅ Playlist operations use your existing YouTube session (as you, on your account)
- ✅ ExtensionPay is used only if you choose Pro (payment handled by ExtensionPay, not by this extension)

**Full privacy policy:** [View Privacy Policy →](./store-assets/privacy-policy.html)

**Disclaimer:** The developer assumes no responsibility or liability for any data loss, account changes, playlist modifications, or any other consequences arising from the use of this extension. The extension is provided "as is" without warranties of any kind.

---

## 🛠️ Development

### Project structure

```
rozszerzenia/
├── manifest.json          # MV3 manifest
├── background.js          # Service worker
├── content.js             # Content script (inline panel)
├── page-bridge.js         # Page-context InnerTube bridge
├── popup.html             # Popup UI
├── popup.js               # Popup logic
├── styles.css             # Popup styles
├── locale-runtime.js      # Runtime i18n loader
├── lib/
│   └── ExtPay.js          # ExtensionPay library
├── icons/                 # Extension icons (16–512px)
├── _locales/              # 25 language bundles
├── store-assets/          # Chrome Web Store assets
│   ├── privacy-policy.html
│   ├── CHROME-STORE-LISTING.txt
│   └── promo-banner.png
└── docs/                  # Implementation plans
```

### Smoke test

1. Load the extension as unpacked
2. Open `https://www.youtube.com/watch?v=...&list=RD...`
3. Check the inline panel appears in the queue sidebar
4. Click "Save Now" — verify tracks are added to your archive playlist
5. Enable auto-capture (Pro) — switch tracks and verify new ones are added
6. Change language in popup — verify UI updates

---

## 📋 Permissions Explained

| Permission | Why it's needed |
|-----------|-----------------|
| `storage` | Save settings and dedup index locally. No data leaves your device. |
| `tabs` | Detect if the active tab is a YouTube Mix page. URL check only. |
| `alarms` | Poll ExtensionPay login completion (temporary, max 15 min). |
| `youtube.com` | Read Mix queue and write to your playlist via your session. |
| `extensionpay.com` | Verify Pro subscription status (optional, only if you use Pro). |

---

## 📄 License

MIT — see [LICENSE](./LICENSE) for details.

---

## 🙏 Credits

- [ExtensionPay](https://extensionpay.com) — frictionless extension payments
- YouTube InnerTube API — accessed through the user's existing browser session

---

<div align="center">

**[⬆ Install from Chrome Web Store](https://chromewebstore.google.com/detail/amgdkimmkgfmkdnclchmcbdaafkgflih)**

Made with ❤️ for YouTube Mix listeners

</div>
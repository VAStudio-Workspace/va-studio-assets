# VAStudio Assets

This repository serves as the official, public-facing distribution hub for visual assets designed and produced by **VAStudio Workspace**.

---

## ⚠️ Terms of Use & Licensing

### Copyright
**Copyright © 2026 VAStudio Workspace. All rights reserved.**

All assets in this repository—including but not limited to wallpapers, illustrations, templates, and graphic design files—are the exclusive intellectual property of **VAStudio Workspace**.

### Permitted Use
* You may download and use these assets strictly for **personal, non-commercial use** (such as personal desktop or mobile device wallpapers).

### Prohibited Use
**You do not have permission to:**
* ❌ **Distribute or Share:** Re-upload, share, or distribute these files on other websites, cloud drives, or platforms.
* ❌ **Modify or Adapt:** Edit, alter, crop, change colors, or build derivative works upon these assets without explicit written permission.
* ❌ **Commercialize or Resell:** Sell, license, or use these assets in any commercial projects, marketing materials, or paid products.

For licensing inquiries, custom design work, or commercial authorization, please contact us directly.

---

## Updating App Content Remotely

VAStudio Pro fetches this repo's `manifest.json` once per day (via Google Apps Script `UrlFetchApp`) to update content without touching application code.

### How it works

The GAS backend fetches `https://raw.githubusercontent.com/VAStudio-Workspace/va-studio-assets/main/manifest.json` and caches it in a `REMOTE_CACHE` sheet. The frontend then uses this cached data.

### Update Playlists (YouTube tracks)

Edit `manifest.json` → `playlists` array. Each entry:

```json
{
  "id": "unique-id",
  "name": "Display name shown in app",
  "type": "video",
  "videoId": "youtube-video-id"
}
```

For YouTube playlists:

```json
{
  "id": "unique-id",
  "name": "Display name shown in app",
  "type": "playlist",
  "playlistId": "youtube-playlist-id"
}
```

**To get a video ID:** From URL `https://www.youtube.com/watch?v=jmy3c7gl5p4` → `jmy3c7gl5p4`
**To get a playlist ID:** From URL `https://www.youtube.com/watch?v=jmy3c7gl5p4&list=PL0f20YMYMr-xlsHO0fZdu1tijgndkIfw6` → `PL0f20YMYMr-xlsHO0fZdu1tijgndkIfw6`

### Add Wallpapers

1. Place your 1920x1080 PNG/JPG in the `wallpapers/` directory
2. Add an entry to `manifest.json` → `wallpapers` array:

```json
{
  "id": "wp-your-name",
  "filename": "your_image.png",
  "alt": "Short description",
  "tags": ["cozy", "desk", "night"]
}
```

### Add Bible Verses

Edit `manifest.json` → `bibleQuotes` array:

```json
{
  "verse": "The verse text",
  "reference": "Book Chapter:Verse",
  "theme": "peace"
}
```

Themes: `peace`, `focus`, `motivation`, `moral`

### Propagation

Changes take up to **24 hours** to appear in the app (the GAS backend re-fetches once per day on first access). To force an immediate refresh, clear the `REMOTE_CACHE` sheet in your spreadsheet or wait for the next daily cycle.

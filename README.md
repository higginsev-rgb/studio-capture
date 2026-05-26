# Studio Capture

Quick-capture PWA for studio sessions. Voice-first, photos, shrinkage-aware planning, CSV export.

## Run it locally (Mac)

The app is a single static HTML file — no build step, no dependencies to install.

**Easiest:** double-click `index.html`. It opens in your default browser.

**Better (lets the service worker / PWA install work):** serve it over http on your local network:

```bash
cd "pottery-studio/apps/quick-capture"
python3 -m http.server 8080
```

Then open `http://<your-mac-IP>:8080/` on your phone (same wifi). Find your Mac's IP in System Settings → Wi-Fi → Details.

## Install on iPhone

1. Open the URL in **Safari** (must be Safari for PWA install).
2. Tap the **Share** button → **Add to Home Screen**.
3. Tap the icon on your home screen — it opens like a native app.

## Daily flow

1. Tap **Start new session** at the studio.
2. Add planned tasks (Throw / Trim / Glaze / Bisque / Deliver / Reclaim / Other) + a quick description.
3. For **Throw** tasks, enter target fired dimensions (cm). The app calculates wet throw size, accounting for **12% shrinkage** (matches your clay's shrink rate).
4. During the session, tap a task to expand it:
   - **🎙 button** → dictate notes (Web Speech API, iOS Safari 14+).
   - **+ tile** → take a photo (camera or library).
   - Optional: link to a piece ID like `P-2026-029` so I can sync later.
5. At the end, fill in the **summary** and **for next time** fields.
6. Tap **End session**, then **Export** to download CSVs + photos.

## What gets exported

- `session-YYYY-MM-DD-summary.csv` — one row, session-level info.
- `session-YYYY-MM-DD-tasks.csv` — one row per task with target/wet dims, notes, piece link, photo count.
- `session-YYYY-MM-DD-taskN-photoN.jpg` — each photo as a separate file.

Drop those into a Claude Code session in this folder and ask: *"merge today's session export into projects.csv"* — I'll match piece IDs, append notes, copy photos into the right `photos/` folders, and update the costs/firings CSVs as appropriate.

## Storage notes

- All data lives in your phone's **localStorage** + browser cache for photos.
- Clearing Safari data will wipe your sessions — **export at the end of each session** to keep a permanent record in the studio folder.
- Photos are resized to 1280px max edge, JPEG 82% quality, to keep storage manageable.

## What this app doesn't do (yet)

- No cloud sync. Each device is its own siloed copy.
- No "match this voice note to an existing piece" — you have to tap a task and dictate.
- No combo-lessons surfacing — when planning a glaze task, you don't get a warning if the combo matches a known-bad pattern. (Could add: when you type glaze names, flag against `glazes.csv` lessons.)
- Voice dictation quality varies. Chrome/Edge on Mac works well; iOS Safari works but sometimes misses words at session start (tap again).

## Configuration

The clay shrink rate (`SHRINK = 0.12`) is hardcoded at the top of `studioApp()` in `index.html`. Edit if you switch clays or want to track different rates per piece.

# Deployment Notes

## GitHub Pages Deployment

### Initial Setup

1. Create a new public repository on GitHub (e.g., `coaster-tracker`)
2. Clone the repo locally or upload files via the GitHub web interface
3. Ensure the following files are in the repo root:
   - `index.html` — Main application (single-file app, ~360KB)
   - `manifest.json` — PWA manifest for home screen install
   - `icon-192.png` — App icon (192×192)
   - `icon-512.png` — App icon (512×512)
   - `.nojekyll` — Prevents Jekyll processing on GitHub Pages
   - `README.md` — Repository documentation
   - `CHANGELOG.md` — Version history
4. Go to **Settings → Pages** in the GitHub repo
5. Set Source to **Deploy from a branch**, branch `main`, folder `/ (root)`
6. Save — the site will be live at `https://<username>.github.io/coaster-tracker/` within a couple of minutes

### Updating the App

1. Replace `index.html` with the updated version
2. Commit and push to `main`
3. GitHub Pages will automatically redeploy (typically within 1-2 minutes)

### Syncing Data Across Machines

User data (added rides, edited ratings/rankings, deleted rides) is stored in `localStorage` and is local to each browser. To transfer data between machines:

1. Click **📤 Export** in the header to download a JSON file
2. Transfer the file to the target machine
3. Click **📥 Import** and select the file — it will show a summary and overwrite local changes on confirmation

Note: The export captures all user modifications against the base data. The base coaster data is embedded in `index.html` and is the same for all visitors.

### Adding to Home Screen (PWA)

**Android (Chrome):**
- Visit the site → tap the three-dot menu → "Add to Home screen"
- The app icon (icon-512.png) will be used

**Android (Samsung Internet):**
- Visit the site → tap the menu → "Add page to" → "Home screen"

**iOS (Safari):**
- Visit the site → tap the share button → "Add to Home Screen"
- The apple-touch-icon (icon-192.png) will be used

### Architecture Notes

- The entire app is a single HTML file with inline CSS and JavaScript
- All coaster data is embedded in the HTML (no external API calls needed)
- User data (custom coasters, ratings, rankings) is stored in `localStorage`
- Chart rendering uses Chart.js (loaded from CDN)
- PNG export uses html2canvas (loaded from CDN)
- The app works fully offline once cached by the browser

### File Size Considerations

The HTML file is ~360KB due to embedded coaster data arrays. On some mobile browsers (especially Samsung Internet), this can occasionally cause the page to open blank on first attempt. The app includes deferred script execution to mitigate this — a retry typically loads successfully.

### Browser Compatibility

- **Chrome (desktop & mobile):** Full support
- **Safari (desktop & iOS):** Full support
- **Samsung Internet:** Full support (includes color-scheme hints to prevent Night Mode interference)
- **Firefox:** Full support

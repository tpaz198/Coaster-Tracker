# Changelog

## Version 3.11 (Current) - 2026-02-23

### Rating Chart Y-Axis Minimum
- Comparison and Over Time charts in Rating mode now start at 1 instead of 0
- Applies to chart initialization, dynamic updates, and both Over Time code paths
- `beginAtZero` set to `false` for all Rating chart contexts

### Multi-Sort Table (Up to 3 Columns)
- Clicking a column header adds it to a sort stack (up to 3 active sorts)
- Clicking an already-sorted column toggles its direction (asc ↔ desc)
- Adding a 4th sort evicts the oldest sort (FIFO)
- Most recently clicked column is the primary sort; earlier sorts act as tiebreakers
- Priority badges (superscript ¹²³) appear on sorted headers when 2+ sorts are active
- Single-sort behavior unchanged (no badge shown)

### Bug Fix: Refresh Button with Multi-Sort
- Refresh button now resets `AppState.sort` to `[]` instead of the old single-sort object format
- Previously caused errors when multi-sort was active

### Ranking Sort: Empty Values Last
- Sorting by Ranking or Weighted Ranking now pushes empty/unranked coasters to the bottom
- Applies in both ascending and descending directions
- Ranked coasters (1–N) sort normally among themselves

### Comparison Chart: Wrapped X-Axis Labels
- Long coaster names now wrap across multiple lines instead of truncating with "..."
- Word-boundary wrapping at 20-character line limit
- Uses Chart.js native array label rendering
- Tooltips still display full ride names

### Comparison Chart Export: Reduced X-Axis Font
- X-axis label font size reduced by 2 base points on export (11pt → 22px at 2× scale)
- Over Time chart export font sizes unchanged

### Over Time Chart: Default Toggle Flipped
- "Ride / Filters" toggle now defaults to "Filters" instead of "Ride"
- Filters panel shown on load; Ride search panel hidden

### Deployment Updates
- Updated PWA icons (icon-192.png, icon-512.png) with new coaster silhouette design
- Added service worker (sw.js) for offline caching support
- Service worker registration added to index.html
- Updated manifest.json with corrected theme colors and maskable icon purpose
- Added apple-touch-icon link

---

## Version 3.9 - 2026-02-19

### Font Size Refinements (Desktop & Mobile)
- Card exterior: title 23px, park 20px, ranking badge 20px, detail rows 17px, rating number 22px, status badge 13px
- Detail modal: title 25px, labels 14px, values 15px, comments 16px
- Credits tab: Add Credit/Filters buttons 15px, filter labels/selects 15px
- Stats tab: toggle buttons ~14.6px, Top 5/10/15 buttons 15px, filter labels/selects 15px
- All font sizes now explicitly declared in mobile media query to ensure propagation

### Mobile Layout Spacing
- Credits/Avg stat boxes reduced 20%: font 1rem, padding 0.6rem 0.4rem, border 2.4px, gap 0.6rem
- `autoFitStatBoxes()` caps at 16px on mobile (down from 20px)
- Header row gap increased to 1.5rem (matching spacing between stat boxes and tabs)

### Stats Toggle Formatting
- "Over Time" text no longer wraps: `white-space: nowrap` on `.chart-type-btn`
- Toggle container widened from 17rem to 19rem for proper centering

### Over Time Chart Export: Legend Centering
- Legend items use `flexBasis: auto` and fixed 50px horizontal gap instead of percentage-based widths
- Legend canvas centered without pixel offset
- `justify-content: center` now works correctly during export capture

### Over Time Export Filename Convention
- Over Time chart exports now append `-over-time` before `.png` (e.g., `top-10-intamin-coasters-over-time.png`)
- Comparison chart filenames unchanged

### Bug Fix: Filters Not Updating After Delete
- All filter dropdowns (Park, Manufacturer, Category, Type) across Credits, Stats Comparison, and Stats Over Time tabs now repopulate after deleting a coaster
- Previously, deleted parks/manufacturers could remain in filter options

### Bug Fix: Edits to Status/Type/Last Ride/Opening Year Not Persisting
- Root cause: `applyToCoasterBase()` mutated `coasterBase` on load, so subsequent `save()` compared current values against already-mutated base and found no difference
- Fix: `snapshotBase()` captures original base field values before any overrides are applied; `save()` compares against the snapshot instead of the mutated `coasterBase`
- Affects all non-EDITABLE_FIELDS overrides: Status, Last Ride, Type, Opening Year

### Bug Fix: Stats Charts Not Reflecting Ranking Changes
- Tab switch to Stats now resizes and updates both Comparison and Over Time charts
- Chart sub-tab toggles (Comparison ↔ Over Time) now trigger chart refreshes
- `recalculateWeightedRankings()` moved before chart updates in edit flow
- Fixes stale ranking data appearing in charts after editing in Credits tab

---

## Version 3.8 - 2026-02-18

### Bug Fix: User-Added Rides in Charts
- Newly added rides now properly appear in both Comparison and Over Time charts
- Added `registerUserRide()`, `unregisterUserRide()`, and `syncRideToYearData()` helpers to sync user rides across `coasterBase`/`yearData2026` data layers
- Charts now refresh immediately after add/edit/delete operations
- Fixed weighted ranking recalculation to avoid double-counting user rides

### Bug Fix: Overrides Not Reaching Over Time Charts
- `Persistence.applyOverrides()` now syncs Rating, Ranking, and Comments to `yearData2026` entries (not just `AppState.coasters`)
- Previously, edited ratings/rankings were correct in Credits and Table tabs but stale in Stats charts
- Added `syncAllToYearData()` bulk sync to catch cascading ranking shifts after add/edit/delete operations
- Rides that gained a rating or ranking via edit now appear in Over Time Filters charts immediately

### Over Time Filters Chart: Stagger for Overlapping Data Points
- Data points sharing the same rating value are now vertically staggered so clusters are visible
- ±0.02 offset for clusters of 2–3 rides, ±0.01 for clusters of 4+
- Tooltips still display the true (unstaggered) value
- No stagger applied to single data points at a given value

### Rating Tooltip Precision
- All charts displaying "Rating" now show values to two decimal places (was one)
- Applies to Comparison chart, Over Time (Filters, Ride, and Averages views)

### Over Time Filters Chart: Legend Color Picker
- Clicking a legend swatch opens a 64-color selection palette (8×8 grid)
- Selected color updates the dataset line, points, and legend swatch instantly
- Color overrides persist across Top N toggle, filter changes, rating/ranking mode, and weighted ranking checkbox
- Overrides are keyed by ride identity (coasterBase index), not display position
- Picker auto-positions to stay within viewport; click-outside to dismiss
- Legend swatches show hover effect (scale + border) to signal interactivity

### Data Export / Import
- New 📤 Export and 📥 Import buttons in the header
- Export downloads a JSON file containing user-added rides, field overrides, and deleted rides
- Import reads the JSON, shows a confirmation summary, writes to localStorage, and reloads
- Enables syncing data changes across machines or browsers
- Desktop layout: Export/Import stacked vertically, aligned with Credits/Avg boxes
- Mobile layout: Export pinned left, Import pinned right, Year selector centered between them

### Modal UX Improvements
- Background blur (4px) applied when any modal is open (detail card, add/edit form, comments)
- Background scroll is locked while a modal is open; scroll position restored on close
- Status badge text now centered within its colored pill on both mobile and desktop

### Mobile UI Improvements
- Export/Import buttons and Year selector scale down to 0.9rem with tighter padding on mobile
- Tab buttons (Credits, Stats, Table) scale to 0.9rem on mobile (≤768px), smaller still on ≤480px
- Title font now uses Comic Neue (Google Font) as fallback for Android devices lacking Comic Sans MS
- Table tab: Refresh button moved below filter toggles to prevent column stacking on narrow screens

---

## Version 3.7 - 2026-02-10

### Mobile Loading & Reliability
- Deferred script execution to prevent blank page on mobile browsers (Chrome, Samsung Internet)
- Improved initial load reliability for the ~630KB single-file app

### Card UI Improvements
- Increased font size for Manufacturer, Type, and Status fields by 3px on cards (desktop & mobile)
- Detail modal maintains two-column layout on mobile

### Table Tab Improvements
- Constrained Ride column width on mobile (`max-width: 120px`) to prevent it from dominating viewport
- Tighter padding and smaller fonts for mobile table readability
- Rating/Ranking cells scaled down from 21px to 16px on mobile
- Comments column `min-width` reduced from 500px to 300px

### Sub-Toggle Styling
- Over Time sub-toggles (Ride|Filters, Rating|Ranking) now width-sync with parent Comparison|Over Time toggle
- Added `flex: 1` and `text-align: center` to sub-toggle buttons for even spacing

### Samsung Internet Compatibility
- Added `color-scheme: dark light` meta tags and CSS declarations to prevent Samsung Internet Night Mode from overriding app theming

### Data & Chart Fixes
- User-added coasters now properly appear in Over Time charts via `registerInBase()`
- `syncYearData()` keeps `yearData2026` in sync after add/edit/delete/rank operations
- Comparison chart sorts ranked coasters by ranking ascending (#1 first), then unranked by rating descending
- New coasters now appear in the Table tab after being added
- Replaced old "prepend saved coasters" approach with `mergeWithBase()` flow

### Deployment
- Added app icon (icon-192.png, icon-512.png) for PWA home screen
- Added apple-touch-icon support for iOS

---

## Version 1.17 - 2026-02-05

### Mobile PNG Export Parity
- Mobile chart downloads now produce identical PNGs to desktop exports
- Forces desktop canvas dimensions (1600px container width) on mobile before export
- Uses desktop baseline font sizes (title: 40px, y-title: 32px, ticks: 28px/26px) on all devices
- Consistent devicePixelRatio (4x) and aspect ratio (16:9) across all devices
- Applies to both Comparison and Over Time sub-tabs

### Legend Export Fixes
- Legend container temporarily set to 1600px on mobile (matching chart container)
- Legend font sizes doubled from 17px to 34px (matching chart's 2x scaling)
- Legend swatch sizes doubled from 12px to 24px
- Legend capture scale increased from 2.5 to 4 (matching devicePixelRatio)
- All legend styles restored after capture

---

## Version 1.16 - 2026-02-05

### UI Color Standardization
- Darkened cyan buttons/toggles from `#0891b2` to `#0e7490` for improved legibility
- Applied to all interactive elements: filter-group selects, column toggles, chart type buttons

### PNG Export Improvements
- White background with black text for all chart exports
- Medium gray gridlines (#808080) for better contrast on white background
- Doubled font sizes for chart title, axis titles, and axis values in exports
- Legend with transparent background, black text, and balanced row spacing
- Legend positioned 50px right of center
- Consistent export formatting for both Comparison and Over Time charts

# Changelog

## Version 3.8 (Current) - 2026-02-17

### Bug Fix: User-Added Rides in Charts
- Newly added rides now properly appear in both Comparison and Over Time charts
- Added `registerUserRide()`, `unregisterUserRide()`, and `syncRideToYearData()` helpers to sync user rides across `coasterBase`/`yearData2026` data layers
- Charts now refresh immediately after add/edit/delete operations
- Fixed weighted ranking recalculation to avoid double-counting user rides

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

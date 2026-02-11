# Changelog

## Version 3.7 (Current) - 2026-02-10

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

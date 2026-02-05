# Changelog

## Version 1.17 - Mobile PNG Export Parity (Current)

### PNG Export Improvements
- **Mobile Export Consistency**: Mobile chart downloads now produce identical PNGs to desktop exports
  - Forces desktop canvas dimensions (1600px container width) on mobile devices before export
  - Uses desktop baseline font sizes (title: 40px, y-title: 32px, ticks: 28px/26px) on all devices
  - **Legend Scaling**: Legend fonts doubled to 34px (from 17px baseline) and swatches to 24px on all devices
  - Legend container forced to 1600px width on mobile and captured at 4x scale (matching devicePixelRatio)
  - Ensures consistent devicePixelRatio (4x) and aspect ratio (16:9) across mobile and desktop
  - Calls `chartInstance.resize()` to force proper dimensional rendering before capture
  - Applies to both "Comparison" and "Over Time" sub-tabs
  - Guarantees pixel-perfect identical exports when downloading the same filtered chart on phone vs computer

### Technical Changes
- Updated `downloadChart()` function to temporarily set container width to 1600px on mobile
- Updated legend capture to force desktop width and double font/swatch sizes
- Increased legend scale from 2.5 to 4 for consistency with chart devicePixelRatio
- Updated `simpleDownload()` function with same container width forcing logic
- Changed from `update('none')` to `resize()` for proper canvas dimension updates
- Increased wait time to 500ms to ensure complete rendering before export
- Added container width restoration after export completes

### Files Changed
- index.html

---

## Version 1.16 - Mobile/Desktop Color Standardization

### Color Consistency Improvements
- **Dual Theme-Color Meta Tags**: Added separate theme-color meta tags for light (#a855f7) and dark (#8b5cf6) modes
- **Mobile Standardization**: Ensured all color schemes are controlled by `prefers-color-scheme` only, not viewport width
- **Ranking Badge Colors**: Confirmed rating-color matching for ranking badges works consistently across all devices
- **Button Text Colors**: Verified !important flags on button text colors apply universally

### Technical Changes
- Added `<meta name="theme-color" content="#a855f7" media="(prefers-color-scheme: light)">`
- Added `<meta name="theme-color" content="#8b5cf6" media="(prefers-color-scheme: dark)">`
- Verified no viewport-width media queries affect color scheme
- Ensured ranking badge inline style `--rating-color` propagates correctly

### Files Changed
- index.html

---

## Version 1.15 - Darker Dark Mode

### Dark Mode Improvements
- **Darker Buttons**: Primary button color changed from #c4b5fd to #8b5cf6 (much darker purple)
- **Darker Backgrounds**: 
  - Main background: #0f172a → #020617 (significantly darker)
  - Secondary background: #1e293b → #0f172a
  - Card background: #334155 → #1e293b
  - Body gradient updated to use darker tones
- **Better Contrast**: Darker backgrounds provide better contrast and reduce eye strain on mobile devices

### Technical Changes
- Updated all dark mode CSS custom properties for backgrounds
- Updated body gradient from `#0f172a 0%, #1e1b4b 50%, #0f172a 100%` to `#020617 0%, #0f172a 50%, #020617 100%`
- Updated shadow-bold variable to match new primary color

### Files Changed
- index.html

---

## Version 1.14 - Mobile Color Improvements

### Color Updates
- **Muted Primary Colors**: Changed from bright fuchsia (#d946ef) to softer purple (#a855f7) in light mode
- **Muted Dark Mode**: Updated dark mode primary from #e9a3f7 to gentler #c4b5fd
- **Improved Readability**: Colors are now easier on the eyes, especially on mobile devices

### Button Text Updates
- **Primary Buttons**: All buttons with pink/purple background (Add Credit, Save Credit, etc.) now have white text
- **Delete Button**: Delete Credit button text changed from white to black for better contrast against red background

### Technical Changes
- Updated CSS custom properties for --primary and --primary-hover in both light and dark modes
- Added !important flags to ensure button text colors are properly applied
- Updated theme-color meta tag and manifest.json to match new color scheme
- Updated shadow-bold variables to match new primary color opacity

### Files Changed
- index.html
- manifest.json

---

## Previous Updates

### Version 1.13
- Stats tab toggle backgrounds set to #0891b2 (cyan)
- Ranking badges in Credits tab now use rating colors in dark mode
- Added subtle text shadow to ranking numbers for better visibility

### Version 1.12
- Column toggle buttons and filter selects styled in cyan (#0891b2) in dark mode
- Table headers use solid cyan background
- Enhanced dark mode cohesion

### Version 1.11
- Replaced Work Sans with Space Grotesk for refined 90s tech aesthetic
- Updated color scheme with cyan accents
- Improved filter and table styling

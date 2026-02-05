# Changelog

## Version 1.14 - Mobile Color Improvements (Current)

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

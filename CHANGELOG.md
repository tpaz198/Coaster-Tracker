# Changelog

## Version 1.15 - Darker Dark Mode (Current)

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

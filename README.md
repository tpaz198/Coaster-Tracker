# Roller Coaster Credit Tracker

A comprehensive web application for tracking and analyzing roller coaster credits with interactive charts, statistics, and filtering capabilities.

## Features

- **Credits Tab**: Browse your coaster collection with detailed cards showing ratings, rankings, and metadata
- **Stats Tab**: Interactive charts including:
  - Comparison view (Rating vs Ranking)
  - Over Time view (track credit growth by year)
  - Ride-specific and filtered statistics
- **Table Tab**: Spreadsheet-style view with sortable columns and customizable column visibility
- **Dark Mode**: Automatic dark mode support with beautiful blue-purple gradients
- **Responsive Design**: Fully optimized for mobile and desktop viewing

## Deployment to GitHub Pages

1. Create a new repository on GitHub
2. Upload the following files:
   - `index.html`
   - `manifest.json`
   - (Optional) `icon-192.png` and `icon-512.png` for PWA support

3. Enable GitHub Pages:
   - Go to Settings > Pages
   - Select "Deploy from a branch"
   - Choose "main" branch and "/ (root)" folder
   - Click Save

4. Your app will be available at: `https://[username].github.io/[repository-name]/`

## PWA Support

This app includes a Progressive Web App manifest. To fully enable PWA features:

1. Add app icons (192x192 and 512x512 PNG files) to your repository
2. The manifest is already linked in the HTML
3. Users can install the app to their home screen on mobile devices

## Customization

To customize the data:
- Edit the `yearData2026`, `yearData2025`, etc. arrays in the JavaScript section
- Adjust colors and styling in the CSS `:root` variables section

## Technologies Used

- Pure HTML, CSS, and JavaScript (no frameworks required)
- Chart.js for interactive visualizations
- Space Grotesk font for modern typography
- Responsive CSS Grid and Flexbox layouts

## License

Personal project - feel free to fork and customize for your own coaster tracking needs!

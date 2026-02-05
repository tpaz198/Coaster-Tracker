# Deployment Notes

## Required Files for GitHub
✅ index.html (main application file)
✅ manifest.json (PWA configuration)
✅ README.md (documentation)
✅ .gitignore (Git configuration)

## Optional Files to Add

### App Icons (for PWA support)
Create two PNG icons and add them to your repository:
- `icon-192.png` (192x192 pixels)
- `icon-512.png` (512x512 pixels)

You can use a roller coaster emoji or create custom icons. Free tools:
- https://favicon.io/
- https://realfavicongenerator.net/

### Favicon
Add a `favicon.ico` file to the root of your repository for browser tab icons.

## GitHub Pages Setup Steps

1. **Create Repository**
   - Go to github.com and create a new repository
   - Name it something like "coaster-tracker"
   - Initialize without README (you already have one)

2. **Upload Files**
   - Click "uploading an existing file"
   - Drag and drop: index.html, manifest.json, README.md, .gitignore
   - Add icons if you created them
   - Commit the files

3. **Enable GitHub Pages**
   - Go to repository Settings
   - Click "Pages" in the left sidebar
   - Under "Source", select "Deploy from a branch"
   - Select "main" branch and "/ (root)" folder
   - Click Save

4. **Access Your App**
   - Wait 1-2 minutes for deployment
   - Visit: https://[your-username].github.io/[repository-name]/
   - Example: https://thomasxl200.github.io/coaster-tracker/

## Custom Domain (Optional)

If you own a domain:
1. Add your domain in the "Custom domain" field in Pages settings
2. Configure DNS with your domain provider
3. Enable "Enforce HTTPS"

## Updates

To update your app:
1. Edit the files locally or directly on GitHub
2. Commit and push changes
3. GitHub Pages will automatically redeploy (takes 1-2 minutes)

## Notes

- The app is entirely client-side (no server needed)
- All data is stored in the JavaScript arrays in index.html
- To update coaster data, edit the yearData arrays in index.html
- Dark mode is automatic based on system preferences
- Works offline after first load (thanks to modern browsers)

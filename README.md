# 📸 My Personal Gallery — Setup Guide

A beautiful, responsive personal photo & video gallery powered by Google Drive.

---

## 🚀 Quick Start (Zero Setup)

1. Open `index.html` in any modern browser — it works immediately with sample photos.
2. Click **+ Add Media** to add your own Google Drive links.
3. Done!

No server, no build tools, no npm required for the basic version.

---

## 🔗 How to Add Your Google Drive Photos & Videos

### Step 1 — Make your file public
1. Upload your photo/video to Google Drive
2. Right-click → **Share**
3. Under "General access", select **"Anyone with the link"**
4. Click **Copy link**

### Step 2 — Paste into the gallery
1. Click **+ Add Media** in the top-right
2. Paste your Google Drive share link
3. Fill in title, description, tags
4. Click **Save Media**

### Supported link formats
```
https://drive.google.com/file/d/FILE_ID/view?usp=sharing
https://drive.google.com/open?id=FILE_ID
```

---

## ✨ Features

| Feature | Details |
|---|---|
| 📁 Grid / List / Large view | 3 layout modes |
| 🔎 Search | Search by title, description, tags |
| 🏷 Tag filtering | Click any tag to filter |
| ♥ Favorites | Heart items to save favorites |
| 🖼 Lightbox | Click any item for full preview |
| 🎬 Video player | Embedded Drive video player |
| ▶ Slideshow | Auto-advancing slideshow mode |
| 🌙 Dark/Light mode | Toggle in header |
| ↓ Download | Per-item download button |
| ✏️ Edit/Delete | Manage items inline |

---

## 📂 File Structure

```
my-personal-gallery/
├── index.html          ← Main app (open this in browser)
├── media-data.json     ← Sample data structure reference
└── README.md           ← This file
```

---

## 🌐 Deployment Options

### Option A — Netlify (Recommended, Free)
1. Go to [netlify.com](https://netlify.com) and sign up free
2. Drag and drop your `index.html` onto the Netlify dashboard
3. Your gallery is live at a `.netlify.app` URL instantly!

### Option B — Vercel
1. Install Vercel CLI: `npm i -g vercel`
2. Run `vercel` in the project folder
3. Follow the prompts — deployed in seconds

### Option C — GitHub Pages
1. Create a GitHub repo and push `index.html`
2. Settings → Pages → Deploy from main branch
3. Live at `https://yourusername.github.io/repo-name`

### Option D — Local / Self-hosted
Just open `index.html` — no web server needed!

---

## 🔑 Advanced: Google Drive API (Optional)

For automatic syncing from a Drive folder without manually adding links:

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project → Enable **Google Drive API**
3. Create an **API Key** (restrict to Drive API)
4. In `index.html`, find `SAMPLE_ITEMS` and replace with an API fetch:

```javascript
// Add to the top of your App component:
const API_KEY = 'YOUR_GOOGLE_API_KEY';
const FOLDER_ID = 'YOUR_GOOGLE_DRIVE_FOLDER_ID';

useEffect(() => {
  fetch(`https://www.googleapis.com/drive/v3/files?q='${FOLDER_ID}'+in+parents&key=${API_KEY}&fields=files(id,name,mimeType,createdTime)`)
    .then(r => r.json())
    .then(data => {
      const mapped = data.files.map(f => ({
        id: f.id,
        title: f.name,
        type: f.mimeType.startsWith('video') ? 'video' : 'image',
        driveLink: `https://drive.google.com/file/d/${f.id}/view`,
        description: '',
        tags: [],
        date: f.createdTime.slice(0, 10),
        favorite: false
      }));
      setItems(mapped);
    });
}, []);
```

> ⚠️ Make sure the folder is shared publicly and API key is restricted to your domain for security.

---

## 🎨 Customization

### Change the gallery title
Find `My Gallery` and `My Personal Gallery` in `index.html` and replace with your name.

### Change accent color
Find `--accent: #c9a96e;` in the `:root` block and change to any hex color.

### Add/remove sample items
Edit the `SAMPLE_ITEMS` array in the script section.

### Persist data across sessions
Replace `useState(SAMPLE_ITEMS)` with localStorage:
```javascript
const [items, setItems] = useState(() => {
  const saved = localStorage.getItem('gallery-items');
  return saved ? JSON.parse(saved) : SAMPLE_ITEMS;
});
// Add this effect to auto-save:
useEffect(() => {
  localStorage.setItem('gallery-items', JSON.stringify(items));
}, [items]);
```

---

## 🛡 Privacy Notes

- Only files shared as "Anyone with the link" will display correctly
- The gallery itself is static HTML — no data is sent to any server
- For private galleries, consider password-protecting your hosting (Netlify supports this on paid plans)

---

## 📱 Browser Support

Chrome, Firefox, Safari, Edge — all modern browsers supported. Mobile responsive.

---

*Built with React 18 + TailwindCSS + Cormorant Garamond typography*

# 📍 Singapore Photo Spots 🎈

An interactive map of 18 top photography locations across Singapore. Tap any pin to pop open a **photo balloon** showing a random shot from that spot, shuffle for another, or jump straight to real posts on Instagram, Pexels, Unsplash, 500px, and iStock.

**[🌐 Live Demo →](#)** *(add your link here after deploying — see below)*

![Made with HTML/CSS/JS](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JS-blue)
![Leaflet.js](https://img.shields.io/badge/map-Leaflet.js-199900)
![No build step](https://img.shields.io/badge/build-none%20required-brightgreen)

---

## ✨ Features

- 🗺️ **18 pinned Singapore locations** — Marina Bay Sands, Gardens by the Bay, Haji Lane, Chinatown, Sentosa, Changi Airport, and more
- 🎈 **Tap-to-pop photo balloons** — click/tap a pin for an animated balloon popup with a real thumbnail preview and a random photo pick
- 🔀 **Shuffle button** — swap in another random photo from the same location without closing the balloon
- 🔗 **90+ real, working links** to Instagram hashtag pages, Pexels, Unsplash, 500px, and iStock
- 🤖 **AI-style recommendations panel** — locations ranked by a simple popularity algorithm

### New in this version

1. **📷 Real photo thumbnails** — balloon popups and post cards show real preview images (via [Lorem Picsum](https://picsum.photos), seeded per location) instead of emoji-only cards
2. **📸 User-submitted community photos** — anyone can post a photo/caption to a location's "Community Photos" section (stored in the browser via `localStorage`, no backend needed)
3. **🧭 "Plan my shoot" itinerary builder** — add locations to an itinerary, auto-optimize the visiting order by proximity (nearest-neighbor), and see the route drawn on the map
4. **🌤️ Live weather + golden hour tag** — current Singapore temperature and today's sunset time (via free, no-key [Open-Meteo](https://open-meteo.com) and [sunrise-sunset.org](https://sunrise-sunset.org) APIs), with a "golden hour soon" flag near sunset
5. **🔍 Search & filter bar** — filter locations by name, category (Landmarks, Gardens & Nature, Street & Heritage, Night & Skyline, Beach & Outdoor, Modern & Architecture), or best time of day
6. **⭐ Favorites** — star any location to save it to a personal shortlist, persisted in `localStorage`
7. **🔗 Shareable location links** — every location has a `?loc=slug` deep link that opens straight to that pin and its balloon
8. **🏷️ Accessibility & trip-info badges** — each location shows crowd level, wheelchair accessibility, ticket requirements, and tripod policy
9. **🌐 Multi-language UI** — English, 中文 (Chinese), Bahasa Melayu, and 日本語 (Japanese) interface toggle
10. **📲 Installable PWA** — `manifest.json` + a minimal offline-caching service worker (`sw.js`) let visitors "Add to Home Screen" and reopen the app shell without a connection

---

## 🚀 Deploy to GitHub Pages (2 minutes)

### Option A — Deploy this exact repo

1. **Create a new repository** on GitHub (e.g. `sg-photo-spots`) — public, no README/license needed since they're already here.
2. **Push these files** to it:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Singapore photo spots map"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
3. **Enable GitHub Pages**:
   - Go to your repo → **Settings** → **Pages**
   - Under **Build and deployment**, set **Source** to `Deploy from a branch`
   - Set **Branch** to `main` and folder to `/ (root)`
   - Click **Save**
4. Wait ~1 minute, then visit:
   ```
   https://<your-username>.github.io/<your-repo>/
   ```
   Because the app lives at `index.html` in the repo root, GitHub Pages serves it automatically — no extra config needed.

### Option B — Drag-and-drop (no git required)

1. Go to [github.com/new](https://github.com/new) and create a repo.
2. On the repo page, click **Add file → Upload files**.
3. Drag in `index.html` (and this `README.md` if you like).
4. Commit directly to `main`.
5. Turn on Pages as in step 3 above.

> 💡 This repo already includes a **GitHub Actions workflow** (`.github/workflows/deploy.yml`) that auto-deploys on every push to `main`. If you use it instead of Option A step 3, choose **Source: GitHub Actions** (not "Deploy from a branch") under **Settings → Pages**. Either method works — pick one.

### Option C — One-click alternatives

This is a static site, so it also deploys as-is on:
- **Netlify** — drag the folder onto [app.netlify.com/drop](https://app.netlify.com/drop)
- **Vercel** — `vercel deploy` from this folder, or import the GitHub repo at [vercel.com/new](https://vercel.com/new)
- **Cloudflare Pages** — connect the repo, leave build command empty, output directory `/`

---

## 🗂️ Project Structure

```
sg-photo-spots/
├── index.html         # The entire app: map, styles, photo data, and logic
├── manifest.json        # PWA manifest (installable app metadata)
├── sw.js                  # Service worker — caches the app shell for offline use
├── README.md            # This file
└── LICENSE                 # MIT license
```

Almost everything — HTML, CSS, and JavaScript (including the photo/location dataset) — lives in a single `index.html` for simplicity. Only the two small PWA files (`manifest.json`, `sw.js`) are separate, since browsers require them as standalone files.

> ⚠️ **Note on data persistence:** Favorites, itinerary, community photos, and language preference are stored in the visitor's own browser via `localStorage`. There's no shared backend/database — each visitor only sees their own saved data, and community photos submitted by one visitor won't be visible to others unless you wire up a real backend (see the customization notes below).

---

## 🛠️ Local Development

No install needed. Either:

- **Double-click `index.html`** to open it directly in a browser, or
- **Serve it locally** (recommended, avoids some browser file:// restrictions):
  ```bash
  # Python 3
  python3 -m http.server 8000
  # then open http://localhost:8000
  ```
  or
  ```bash
  npx serve .
  ```

---

## ✏️ Customizing

All location and photo data lives in the `photoAlgorithm.photosDatabase` array and the `locations` array inside `index.html`.

### Add a new location

1. Add coordinates to the `locations` array:
   ```js
   { name: "Your New Spot", lat: 1.3000, lng: 103.8000 }
   ```
2. Add at least one photo entry to `photosDatabase` with a matching `location` field:
   ```js
   {
     id: "yourspot_001",
     location: "Your New Spot",
     time: "sunset",
     hashtags: "#yourspotphoto #singaporephotography",
     platform: "instagram",
     link: "https://www.instagram.com/explore/tags/yourspotphoto/",
     title: "Your New Spot Photography",
     source: "Instagram Hashtag",
     likes: 1000,
     emoji: "📸"
   }
   ```
3. Save and refresh — the new pin, balloon, and sidebar entry appear automatically.

### Change the map style, colors, or balloon design

Everything is in the `<style>` block at the top of `index.html` — search for `BALLOON POPUP STYLES` to find the popup-specific CSS.

---

## ⚠️ Known Limitations

- **Community photos & favorites are per-browser only** — they use `localStorage`, so they won't sync across devices or be visible to other visitors. To make community photos truly shared, connect a small backend (e.g. [Supabase](https://supabase.com) or [Firebase](https://firebase.google.com) free tier) and swap the `localStorage` calls in `addCommunityPhoto()` / `getCommunityPhotos()` for API calls.
- **Weather and thumbnails require internet** — the weather widget and photo thumbnails call external free APIs (Open-Meteo, sunrise-sunset.org, Lorem Picsum) at runtime. If a visitor is fully offline, the service worker keeps the app shell loadable, but these live elements will show a fallback/blank state.
- **Thumbnails are illustrative, not verified location photos** — since there's no paid image API key wired in, thumbnail images come from Lorem Picsum seeded per location for a consistent "real photo" look. Swap in a real Unsplash/Pexels API key (both have free tiers) in `thumbUrl()` if you want thumbnails guaranteed to match the actual place.

## 📄 License

MIT — see [LICENSE](./LICENSE). Feel free to fork, adapt, and reuse.

## 🙏 Credits

Built with [Leaflet.js](https://leafletjs.com/) and [OpenStreetMap](https://www.openstreetmap.org/copyright) tiles. Photo links point to public search/hashtag pages on Instagram, Pexels, Unsplash, 500px, and iStock — all content on those platforms belongs to its respective creators/owners.

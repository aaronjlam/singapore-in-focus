# 📍 Singapore Photo Spots 🎈

An interactive map of 18 top photography locations across Singapore. Tap any pin to pop open a **photo balloon** showing a random shot from that spot, shuffle for another, or jump straight to real posts on Instagram, Pexels, Unsplash, 500px, and iStock.

**[🌐 Live Demo →](#)** *(add your link here after deploying — see below)*

![Made with HTML/CSS/JS](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JS-blue)
![Leaflet.js](https://img.shields.io/badge/map-Leaflet.js-199900)
![No build step](https://img.shields.io/badge/build-none%20required-brightgreen)

---

## ✨ Features

- 🗺️ **69 pinned Singapore locations across 9 compass regions** — North, North-East, East, South-East, South, South-West, West, North-West, and Central, from Marina Bay Sands and Gardens by the Bay to Coney Island Park, Kusu Island, Bollywood Veggies, and Berlayer Creek
- 🎈 **Tap-to-pop photo balloons** — click/tap a pin for an animated balloon popup with a real thumbnail preview and a random photo pick
- 🔀 **Shuffle button** — swap in another random photo from the same location without closing the balloon
- 🔗 **345 real, working links** (5 per location) to Instagram hashtag pages, Pexels, Unsplash, 500px, and iStock
- 🤖 **AI-style recommendations panel** — all 69 locations ranked by a popularity algorithm

### Feature set

1. **📷 Real photo thumbnails** — balloon popups and post cards show real preview images (seeded per location via [Lorem Picsum](https://picsum.photos))
2. **📸 User-submitted community photos** — anyone can post a photo/caption to a location's "Community Photos" section (stored in the browser via `localStorage`)
3. **🧭 "Plan my shoot" itinerary builder** — add locations to an itinerary, auto-optimize the visiting order by proximity (nearest-neighbor), and see the route drawn on the map
4. **🌤️ Live weather, per-location** — real current temperature and conditions for every one of the 69 locations, fetched in a single batched call to the free [Open-Meteo](https://open-meteo.com) API (no key needed), plus a Singapore-wide "golden hour soon" flag near sunset (via [sunrise-sunset.org](https://sunrise-sunset.org))
5. **🕐 Live clock & date** — a real-time Singapore-time clock (updates every second), shown as **DD-MMM-YYYY** (e.g. `01-Sep-2026`) and **12-hour AM/PM** time (e.g. `09:31:11 PM`), in the header and in each location's "Live Conditions" panel
6. **👥 Estimated crowd levels** — every location shows a Low/Medium/High/Very High crowd estimate that updates based on the current Singapore time-of-day and day-of-week against a per-category peak-hours model. *(Clearly labeled as an estimate — no free public API provides genuine live crowd-sensor data for these spots; see Known Limitations.)*
7. **🔍 Search & filter bar** — filter by name, **region** (9-way compass grid), category, or best time of day
8. **⭐ Favorites** — star any location to save it to a personal shortlist, persisted in `localStorage`
9. **🔗 Shareable location links** — every location has a `?loc=slug` deep link that opens straight to that pin and its balloon
10. **🏷️ Accessibility & trip-info badges** — each location shows region, wheelchair accessibility, ticket requirements, and tripod policy
11. **🌐 7-language dropdown** — English, Bahasa Melayu, 中文 (Chinese), 日本語 (Japanese), தமிழ் (Tamil), 한국어 (Korean), and Русский (Russian)
12. **📲 Installable PWA** — `manifest.json` + a minimal offline-caching service worker (`sw.js`) let visitors "Add to Home Screen" and reopen the app shell without a connection

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

1. Add an entry to the `locations` array with a `region` (`North`/`Central`/`West`/`East`/`South`) and `category`:
   ```js
   { name: "Your New Spot", slug: "your-new-spot", lat: 1.3000, lng: 103.8000, region: "West", category: "Gardens & Nature", accessibility: "♿ Wheelchair accessible", ticket: "🎟️ Free", tripod: "✅ Tripod OK" }
   ```
2. Either add 5 hand-curated entries to `curatedPhotos` (see the existing format), **or** just add an entry to `generatorMeta` and let the app auto-generate all 5 platform posts for you:
   ```js
   "Your New Spot": { times: ["morning", "sunset"], emojis: ["📸", "🌇"] }
   ```
3. Save and refresh — the new pin, balloon, weather/crowd data, and sidebar entry all appear automatically.

### Change the map style, colors, or balloon design

Everything is in the `<style>` block at the top of `index.html` — search for `BALLOON POPUP STYLES` to find the popup-specific CSS.

---

## ⚠️ Known Limitations

- **The 9-region compass grid isn't an official government classification** — Singapore's official URA planning regions are only Central, East, North, North-East, and West (no South/South-East/South-West). The 9-way North/North-East/East/South-East/South/South-West/West/North-West/Central split used here is a compass grid computed from each location's coordinates relative to the Downtown Core (Raffles Place), added because it's genuinely useful for browsing. **Region counts are naturally uneven** — Central has 17 locations because Singapore's tourist attractions genuinely cluster around the downtown/Marina Bay area, while **South and South-East have only 2 and 1 respectively**, because Singapore's downtown core already sits near the island's southeast coast — there's very little landmass further south or southeast beyond a couple of ferry-only Southern Islands (Kusu, St John's) and one waterfront precinct (Marina South Pier, whose South-East classification is an editorial judgment call rather than a strict formula result, since the nearest real landmark fell just short of the compass threshold). This is disclosed here rather than force-fitting inaccurate labels onto locations that don't fit.
- **Crowd levels are estimates, not live sensor data** — there is no free public API that reports genuine real-time crowd counts for these 69 locations. The app computes an honest, disclosed estimate from the current Singapore time-of-day, day-of-week, and each location's typical category pattern (e.g. landmarks peak at midday and evenings, nature reserves peak at dawn/dusk). This is clearly labeled in the UI ("Crowd level is an estimate based on time-of-day patterns, not a live sensor feed").
- **Weather is genuinely real-time** — every location's temperature and conditions come from a single batched call to Open-Meteo using that location's actual coordinates, refreshed on load.
- **Community photos & favorites are per-browser only** — they use `localStorage`, so they won't sync across devices or be visible to other visitors. To make community photos truly shared, connect a small backend (e.g. [Supabase](https://supabase.com) or [Firebase](https://firebase.google.com) free tier) and swap the `localStorage` calls in `addCommunityPhoto()` / `getCommunityPhotos()` for API calls.
- **Thumbnails are illustrative, not verified location photos** — since there's no paid image API key wired in, thumbnail images come from Lorem Picsum seeded per location for a consistent "real photo" look. Swap in a real Unsplash/Pexels API key (both have free tiers) in `thumbUrl()` if you want thumbnails guaranteed to match the actual place.
- **New locations' photo posts are auto-generated, not hand-curated** — the original 18 locations have hand-picked post titles/hashtags; the other 51 locations (added to cover all 9 regions and sourced from public travel blogs, Tripadvisor forum threads, and photography guides — see list below) have posts generated from a template (`generatePostsForLocation()`) using consistent, valid search-page links per platform, so they're just as clickable but less individually curated in wording.
- **Sourcing for the expanded location list** — 16 spots (Tiong Bahru, Pinnacle@Duxton, Dempsey Hill, CHIJMES, ArtScience Museum, Victoria Theatre, National Gallery Singapore, Bukit Timah Railway Station, Bukit Brown Cemetery, Haw Par Villa, Alkaff Mansion, Sembawang Park, Pasir Ris Park, Punggol Waterway Park, Jewel Changi Airport, and Kong Meng San Phor Kark See Monastery) were identified via public web search across travel/photography blogs (Maps & Merlot, The Wandering Lens, TripZilla, Time Out Singapore) and a crowdsourced Tripadvisor forum thread on Singapore photography locations. A further 16 spots (Kusu Island, St John's Island, Marina South Pier, Yishun Park, Berlayer Creek, Bedok Jetty, Coney Island Park, Lorong Halus Wetland, Serangoon Gardens, Punggol Beach, Dairy Farm Nature Park, Bollywood Veggies, Jurong Hill Park, Science Centre Singapore, Gillman Barracks, and Kallang Riverside Park) were added specifically to populate the newly introduced North-East, North-West, South-West, South-East, and South regions. Coordinates for all of these are approximate (landmark-level accuracy), not survey-precise.
- **Translations are functional, not professionally reviewed** — the 7-language UI strings aim for accuracy but haven't been reviewed by native speakers of every language; contributions/corrections via pull request are welcome.

## 📄 License

MIT — see [LICENSE](./LICENSE). Feel free to fork, adapt, and reuse.

## 🙏 Credits

Built with [Leaflet.js](https://leafletjs.com/) and [OpenStreetMap](https://www.openstreetmap.org/copyright) tiles. Photo links point to public search/hashtag pages on Instagram, Pexels, Unsplash, 500px, and iStock — all content on those platforms belongs to its respective creators/owners.

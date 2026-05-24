# M-Dot static link hub

Plain **HTML** (no Astro, no Supabase, no build step). Open `index.html` in a browser or serve the folder with any static file server.

## Local preview

```bash
cd /Users/4rgd/Astro/mdot-static-site
python3 -m http.server 8080
```

Then open http://127.0.0.1:8080/

## Includes

- [Tailwind CSS](https://tailwindcss.com/) (Play CDN) for utility classes  
- [Swiper 11](https://swiperjs.com/) (jsDelivr) for the full-screen slideshow  
- [Font Awesome 6.5](https://fontawesome.com/) (cdnjs) for brand icons (Instagram, Facebook, Spotify, SoundCloud, etc.) — no `npm install` required for deploy  
- **Raster assets** (in `images/`): `slide-01-youtube-channel.png` (first slide / OG preview, transparent PNG), `slide-02-library-of-sound.jpg`, `slide-03-ego-and-the-enemy.jpg`, `logo-wordmark.png` (header wordmark), `favicon-mdot.png` (favicon and Apple touch icon). Older CDN pulls were vendored to `images/`; `index.html` uses **relative** paths (`images/…`).
- **Social preview:** `og:image` and `twitter:image` use `images/slide-01-youtube-channel.png` (same file as the first slide). Open Graph crawlers resolve that URL against your deployed site origin.
- **Dock:** six tabs — Music, Spotify, SoundCloud, Instagram, Facebook, Tour. SoundCloud: [soundcloud.com/mdotboston](https://soundcloud.com/mdotboston). Facebook: [facebook.com/MDotBoston](https://www.facebook.com/MDotBoston/).
- **Logo vs hero:** The wordmark stays **`z-20`** (unchanged). While **slide 1** is active, JS sets **`#m-dot-slideshow` `z-index: 25`** so the cutout PNG can paint above the logo; on slides 2–3 that inline z-index is cleared so opaque art stays **under** the logo. Dock remains **`z-30`**. **`--m-dot-z`** on `.swiper-slide` still only sorts slides inside Swiper.
- **Load sting:** `audio/m-dot-load-preview.mp3` — ~7.5s excerpt from **Dreamscape (Prod. By Marco Polo)** (*egO anD The eneMy*, Bandcamp; cut from ~0:42 so the beat is obvious). On `load` (or first `pointerdown` if autoplay is blocked), it plays at ~62% volume, then **fades out over ~1.6s** starting ~4.8s in. The `<audio>` `src` may use a `?v=` query to bust caches after replacing the file. Re-cut with ffmpeg if you swap tracks (Bandcamp stream URLs expire; keep a file in `audio/`).

## GitHub

Create a **new** repository (do not reuse the old bloated `mdot-link-hub` unless you delete it in the GitHub UI first):

```bash
cd /Users/4rgd/Astro/mdot-static-site
git init
git add index.html README.md .gitignore images/ audio/
git commit -m "Initial M-Dot static link hub"
gh repo create eliteweblabs/mdot-static-site --public --source=. --remote=origin --push
```

To delete the previous mistaken repo from the CLI: `gh auth refresh -h github.com -s delete_repo` then `gh repo delete eliteweblabs/mdot-link-hub --yes`.

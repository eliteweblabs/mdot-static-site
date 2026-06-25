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
- **Raster assets** (in `images/`): `slide-01-youtube-channel.png` (first slide hero, transparent PNG), `slide-02-library-of-sound.jpg` (also used for Open Graph / Twitter preview), `slide-03-ego-and-the-enemy.jpg`, `logo-wordmark.png` (header wordmark), `favicon-mdot.png` (favicon and Apple touch icon). Older CDN pulls were vendored to `images/`; `index.html` uses **relative** paths (`images/…`).
- **Social preview:** `og:image` / `twitter:image` use **`images/slide-02-library-of-sound.jpg`** (1200×1200 — `og:image:width` / `height` match the file). Platforms may crop for link cards; swap the file or add a dedicated crop later if you want a specific aspect ratio.
- **Dock:** six tabs — Music, Spotify, SoundCloud, Instagram, Facebook, Tour. SoundCloud: [soundcloud.com/mdotboston](https://soundcloud.com/mdotboston). Facebook: [facebook.com/MDotBoston](https://www.facebook.com/MDotBoston/).
- **Logo vs hero:** The wordmark stays **`z-20`** (unchanged). While **slide 1** is active, JS sets **`#m-dot-slideshow` `z-index: 25`** so the cutout PNG can paint above the logo; on slides 2–3 that inline z-index is cleared so opaque art stays **under** the logo. Dock remains **`z-30`**. **`--m-dot-z`** on `.swiper-slide` still only sorts slides inside Swiper.
- **Slideshow:** **`loop: false`** + **`rewind: true`** so Swiper does not duplicate slides (loop could make another slide appear “first”). **Autoplay** is stopped on init and starts only after the **`m-dot-load-sting-end`** event (when the load MP3 has fully faded out), or after a **17s** fallback if audio never finishes (blocked autoplay, missing file, etc.).
- **Load sting:** `audio/m-dot-load-preview.mp3` — ~**12.5s** excerpt from **Dreamscape (Prod. By Marco Polo)** (*egO anD The eneMy*, Bandcamp; cut from ~0:42 so the beat is obvious). Encoded **small for fast download** (~48 kb/s mono, 22.05 kHz; re-encode with `ffmpeg -i … -ac 1 -b:a 48k -ar 22050 out.mp3` if you replace the source). The `<audio>` tag uses **`preload="none"`** and a **`data-src`** (not `src`) so the MP3 is **not fetched until** the page `load` event, **eager / high-priority hero images** have finished, and **`requestIdleCallback`** (or ~1.2s fallback) plus a short buffer — so it does not compete with first paint. **Skipped entirely** when the Network Information API reports **save-data**, **2g / slow-2g**, or **downlink &lt; 0.5 Mbps** (carousel starts immediately via **`m-dot-load-sting-end`**). First **`pointerdown`** skips the idle wait (helps iOS autoplay). Plays at ~62% level once buffered. The **fade uses Web Audio `GainNode`** when supported (`exponentialRampToValueAtTime` over ~5s starting ~**7.35s** in), because **`HTMLMediaElement.volume` is often ignored on iOS** (audio stays loud then stops abruptly). Falls back to stepping `audio.volume` on older browsers. When the fade completes, it dispatches **`m-dot-load-sting-end`** so the carousel autoplay can start. The `data-src` may use a `?v=` query to bust caches after replacing the file. Re-cut with ffmpeg if you swap tracks (Bandcamp stream URLs expire; keep a file in `audio/`).

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

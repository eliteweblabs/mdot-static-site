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
- External images: Kinsta wordmark, album art, YouTube thumbnails  

## GitHub

Create a **new** repository (do not reuse the old bloated `mdot-link-hub` unless you delete it in the GitHub UI first):

```bash
cd /Users/4rgd/Astro/mdot-static-site
git init
git add index.html README.md .gitignore
git commit -m "Initial M-Dot static link hub"
gh repo create eliteweblabs/mdot-static-site --public --source=. --remote=origin --push
```

To delete the previous mistaken repo from the CLI: `gh auth refresh -h github.com -s delete_repo` then `gh repo delete eliteweblabs/mdot-link-hub --yes`.

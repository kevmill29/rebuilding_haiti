# Rebuilding Haiti, One Layer of Brick at a Time

Awareness site and admin dashboard for a family-led housing and jobs project in Haiti.

Full setup instructions, including the non-technical guide for the campaign organizer, are in **[DEPLOYMENT-GUIDE.md](DEPLOYMENT-GUIDE.md)**.

## Architecture

Static files, no build step, no server.

```
index.html            Single-page public site
thanks.html           No-JavaScript fallback after a form submit
content/*.json        All editable content, fetched at page load
  site.json             Links, organizer name, hero copy
  milestones.json       Build phases and their status
  gallery.json          Photos and videos
  updates.json          Progress log posts
admin/index.html      Mounts Decap CMS
admin/config.yml      Defines the dashboard's editing screens
assets/uploads/       Images uploaded through the dashboard
netlify.toml          Headers and publish settings
```

`index.html` ships with a `DEFAULTS` object matching the JSON files, so the page renders correctly even when opened directly from disk or if a content file fails to load.

## Stack

| Concern | Choice |
|---|---|
| Hosting | Netlify (static, free) |
| Content management | Decap CMS at `/admin`, committing to Git |
| Admin authentication | DecapBridge (Netlify Identity is deprecated) |
| Forms and submissions | Netlify Forms, with built-in table view and CSV export |
| Styling | Tailwind via CDN, custom fonts from Google Fonts |

## Local preview

Form submissions and `/content` fetches need to be served over HTTP, not opened as files:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

The dashboard at `/admin` will not authenticate locally unless you configure Decap's `local_backend`.

## Design notes

- Palette is fixed by the brief: `#B85042` brick, `#E7E8D1` sand, `#A7BEAE` palm, over `#F7F6EE` with `#241F1B` ink.
- Type: Bricolage Grotesque (display), Newsreader (body), IBM Plex Mono (labels and data).
- The milestone tracker is the signature element. Each phase renders as a course of block that fills left to right when it scrolls into view, and the courses stack bottom-up so completed work sits at the base of the wall. Completed count drives the hero statistic and the progress label.
- Motion is limited to the wall and respects `prefers-reduced-motion`.

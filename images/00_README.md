# Images

*Profile images for the cohort creatives.*

---

## Naming Convention

One image per cohort member. Filename matches the cohort markdown filename:

```
01-shivolski.jpg
02-chef-nqobi.jpg
03-kgale.jpg
04-zolelo.jpg
05-fifi-frequency.jpg
06-seru.jpg
07-ohteeh.jpg
08-hazy.jpg
09-lumai.jpg
10-dg.jpg
11-just-jabba.jpg
```

`.jpg` or `.png` — both work. Lowercase filenames.

---

## How They Get Used

The website's render code (in `index.html`) will check for an `image` field on each creative in `data/creatives.json`. If present, it shows the image. If absent or the file fails to load, it falls back to the initials circle.

**Workflow:**
1. Drop a new image file into this folder with the right filename.
2. Tell Claude (or update yourself): set `"image": "images/01-shivolski.jpg"` on the matching entry in `data/creatives.json`.
3. Reload the site — the avatar updates.

The image-render integration in the HTML is **not yet wired** — when the first batch of images arrives, we wire it then.

---

## Image Specs (Recommended)

- **Size:** ~800×800 px (square)
- **Format:** JPG or PNG
- **Crop:** Centred portrait or product shot — the avatar circle will crop tightly to the centre
- **Background:** doesn't matter — but consistent backgrounds across the cohort look more polished

These are guidelines, not rules. The render will work with anything reasonable.

---

## Related

- `Cohort/` — the markdown files matching these images (`01-shivolski.md` ↔ `01-shivolski.jpg`)
- `data/creatives.json` — the schema that points to these files
- `index.html` — the render code that displays them

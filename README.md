# MamaMilkiva website

Promotional website for the MamaMilkiva Android app, built for GitHub Pages.
Plain HTML, CSS and JavaScript: no build step and no dependencies.

| Page | Slovenian | English | Other app languages |
|---|---|---|---|
| Landing page | `index.html` | `en/index.html` | – (links go to the English page) |
| Privacy policy | `zasebnost/index.html` | `en/privacy/index.html` | `<code>/privacy/index.html` for bg, bs, cs, de, el, es, fr, hr, hu, it, pl, pt-br, ro, sk, sr (Latin), tr |
| Not found | `404.html` (both languages) | | |

The app opens the privacy page in its own language (`privacy_policy_url` in each `values-*/strings.xml` of the app; Serbian Cyrillic uses the Latin `sr` page, every other language the English page).
Every privacy page lists all languages in its footer and carries the full set of `hreflang` alternates (`x-default` = English); `sitemap.xml` has the same set.

The look follows `../designs/` (direction "Objem"): Young Serif + Figtree, rose / sage / peach / plum palette, the app's own illustrations and screens.

## Preview locally

Open `index.html` in a browser, or serve the folder (closer to GitHub Pages):

```sh
cd MamaMilkiva-Website
python3 -m http.server 8000
# http://localhost:8000
```

## Publish on GitHub Pages

This folder is its own repository, `deniskorinsek/mamamilkiva`, served by GitHub Pages from the root of `main` at **https://deniskorinsek.github.io/mamamilkiva/**. The local folder keeps its old name, `MamaMilkiva-Website`. (The private app repository was renamed from `MamaMilkiva` to `MamaMilkiva-App` to free the name; GitHub repository names ignore case.)

Until 15 November 2026 the site lived at `deniskorinsek.github.io/MamaMilkiva-Website/`. That address is now a redirect-only repository (`deniskorinsek/MamaMilkiva-Website`) that forwards every path to the same path here; keep it, because app build 1.0 (versionCode 1) still links its privacy policy there.

Later changes: commit and push to `main`; Pages redeploys in a minute or two. `.nojekyll` makes Pages serve the files as they are.

## Site settings

| Setting | Value | Where it appears |
|---|---|---|
| Website repository | `deniskorinsek/mamamilkiva` | this folder (`MamaMilkiva-Website/`) |
| App repository | `deniskorinsek/MamaMilkiva-App` (private) | not linked from the site; downloads go through Google Play, questions to the contact email |
| Site URL | `https://deniskorinsek.github.io/mamamilkiva/` | canonical, hreflang, Open Graph, `sitemap.xml`, `robots.txt` |
| Publisher and contact | Denis Korinsek, korinsekdenis+mamamilkiva@gmail.com | privacy pages |
| Privacy policy effective date | 12 November 2026 | all 18 privacy pages (update it whenever the policy changes) |
| Base path of the 404 page | `<base href="/mamamilkiva/">` | `404.html` |

Moving to a custom domain: replace `https://deniskorinsek.github.io/mamamilkiva/` in all files (and in `privacy_policy_url` of every app language), add a `CNAME` file, and set the 404 base to `/`.

## Google Play

- Every store link points to `https://play.google.com/store/apps/details?id=si.mamamilkiva.app` (the `applicationId` from `app/build.gradle.kts`), with `&hl=sl` or `&hl=en`. The links work as soon as the store listing is live.
- The QR code in the download section (shown on wide screens) encodes the same URL without `hl`. Regenerate it if the package name changes, e.g. with `segno` and `error="q"`, and keep the small logo in the middle.
- The store button is text only. To use Google's official badge instead (a Slovenian version exists), download it from <https://play.google.com/intl/en_us/badges/>, follow its guidelines, and put the image inside the `.store-btn` links.
- In the Play Console, use the privacy page as the privacy policy URL: `https://deniskorinsek.github.io/mamamilkiva/zasebnost/` (English: `/en/privacy/`, other languages: `/<code>/privacy/`). Play has one privacy URL per app, so the Slovenian or English one is enough there; the app itself links the page in the user's language.

## Also needed

- **Review the privacy policy.** It is a draft based on the app's manifest, `MamaMilkiva-Android/docs/PRIVACY_POLICY.md` and GitHub Pages hosting. Update it whenever permissions, storage or distribution change. It is not legal advice.
- **Screenshots** in `assets/img/screens/<sl|en>/` (home, editor, history-stats, history-days, settings; WebP, 420 and 786 px wide) are made from the Play Store screenshots of the current app by `../designs/website/render_website_images.py`, which also renders the link previews `og-sl.png` / `og-en.png` from `../designs/website/og.html`. Re-run it after new store screenshots (`MamaMilkiva-Android/tools/store_screenshots.py`).

## Motion

All motion is progressive enhancement in `assets/js/main.js`:

| Attribute / class | Effect |
|---|---|
| `data-reveal` (`fade`, `scale`, `rise`) | fades and lifts an element in when it scrolls into view; `--delay` staggers it |
| `data-speed` | scroll parallax relative to the closest `[data-parallax]` section (positive = slower, further back) |
| `data-depth` | pointer parallax inside `[data-pointer]` (desktop only) |
| `.hero__sun` | the sun rises along the dotted arc, like the app's home screen |
| `.stage` + `.step` | sticky phone in "How it works" (desktop) that follows the active step |
| `[data-quotes]` | rotating phrases from the app (with a pause button) |
| `img[data-still]` | animated SVG illustration; switches to the still version when motion is off |

Motion stops completely when the system asks for reduced motion, or when a visitor presses **Ustavi animacije / Pause animations** in the footer (saved in that browser only). Without JavaScript every element is visible and static.

## Structure

```
MamaMilkiva-Website/
├── index.html, 404.html, site.webmanifest, sitemap.xml, robots.txt, .nojekyll
├── zasebnost/index.html
├── en/index.html, en/privacy/index.html
├── <code>/privacy/index.html  bg bs cs de el es fr hr hu it pl pt-br ro sk sr tr
└── assets/
    ├── css/styles.css         tokens, components, pages
    ├── js/main.js             motion and small enhancements
    ├── fonts/                 Figtree + Young Serif (WOFF2 subsets, SIL OFL 1.1; *-ext.ttf full fonts load only for Romanian ș ț)
    └── img/
        ├── screens/sl, en/    app screens of the current version (WebP)
        ├── illustrations/     animated/ and static/ SVGs from ../designs
        ├── icons/             app icon, favicons, manifest icons
        └── og-sl.png, og-en.png   link previews (1200×630)
```

Slovenian and English mirror each other; when you change one page, change its counterpart too. When the privacy policy changes, update all 18 privacy pages and their effective date (Greek and Cyrillic text uses the system fallback fonts, the brand fonts cover Latin only).

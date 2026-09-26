# VAStudio Pro — Learning Course Catalog

Static assets consumed by the **Learning** page of VAStudio Pro. Everything here
is served over the public GitHub raw CDN, so the desktop app can fetch and render
the catalog without bundling any content.

## Contents

| Path | Purpose |
| --- | --- |
| `courses.json` | Manifest the app fetches to render the course list. |
| `course-images/` | One `640×360` SVG thumbnail per course, named `<course-id>.svg`. |
| `logos/` | One `256×256` rounded-square SVG placeholder logo per provider. |
| `affiliate.json` | Manifest the app fetches to render the affiliate "Ads System". |
| `affiliate-images/` | One `640×360` SVG creative per ad, named `<ad-id>.svg`. |

## `courses.json` schema

```jsonc
{
  "version": 1,                       // schema/version integer
  "updated": "YYYY-MM-DD",            // last content update (ISO date)
  "courseImagesBase": "https://.../marketing/course-images/", // prefix for `image`
  "logosBase": "https://.../marketing/logos/",                // prefix for `logo`
  "categories": [                     // top-level category vocabulary
    { "id": "web-development", "label": "Web Development" },
    { "id": "ai-ml", "label": "AI & Machine Learning" }
  ],
  "courses": [
    {
      "id": "free-web-design",        // unique slug; also the thumbnail filename
      "title": "Responsive Web Design Certification",
      "description": "markdown text (2-4 sentences, may use **bold** and one bullet list)",
      "cost": "free",                 // "free" | "premium"
      "url": "https://...",           // where the course opens
      "provider": "freeCodeCamp",     // display name
      "image": "free-web-design.svg", // relative to courseImagesBase
      "logo": "freecodecamp.svg",     // relative to logosBase
      "tags": ["web development", "html", "css"],
      "categories": ["web-development", "design"] // ids from top-level `categories`
    }
  ]
}
```

- `id` must be **unique** and match the thumbnail filename exactly.
- `cost` drives the chip color in the thumbnail: `free` → green, `premium` → amber.
- `description` is rendered as Markdown. Keep it short (2–4 sentences) and use at
  most one bullet list.
- `image` / `logo` are resolved against `courseImagesBase` / `logosBase`, so the
  CDN host can change without touching every entry.

### Categories

- The top-level `categories` array is the **vocabulary** the app uses to build
  category filters/chips. Each entry is `{ "id": "<slug>", "label": "<display>" }`.
- Every course has a `categories` array of one or more category `id`s drawn from
  that vocabulary. At least one category is required.
- Category `id`s are lowercase kebab-case slugs. Add new categories to the
  top-level list **before** referencing them from a course; never inline a
  category that is not declared there.
- `label` is UI display text and may contain `&` and spaces (e.g.
  `"AI & Machine Learning"`).

## Adding or replacing a course

1. Add an entry to `courses.json#courses` with a new unique `id`.
2. Add `course-images/<id>.svg` (a `640×360` thumbnail).
3. If the provider is new, add `logos/<provider-slug>.svg` and set `logo` to it.
4. Assign `categories` using one or more `id`s already declared in the top-level
   `categories` array (add a new vocabulary entry first if needed).
5. Bump `updated` to today's date.
6. Commit and push to `main`.

## Thumbnail conventions (`course-images/`)

- Root: `width="640" height="360" viewBox="0 0 640 360"`.
- Two-color linear-gradient background + a dark overlay so white text stays legible.
- Provider name (small caps) top-left; `FREE`/`PREMIUM` chip top-right.
- Course title in white, bold ~34px, wrapped to at most 3 lines.
- Font stack: `Segoe UI, Arial, sans-serif`. **No external fonts/images.**
- XML-escape `&`, `<`, `>`, `'` in text.

## Logo conventions (`logos/`)

- Root: `width="256" height="256" viewBox="0 0 256 256"`.
- Rounded square (`rx="40"`) with a brand-ish solid background and a white
  wordmark centered and auto-fit.
- These are **placeholders only** — they do not reproduce trademarked artwork.
  Do not replace them with official brand assets unless licensing is cleared.

## `affiliate.json` schema

Manifest for the affiliate **Ads System**. Ads are sponsored product/travel
promotions. The primary network is **Involve Asia**, but the `url` field also
accepts a direct first-party partner URL or a short link (e.g. Bitly) because
clicks are counted on the destination.

```jsonc
{
  "version": 1,                       // schema/version integer
  "updated": "YYYY-MM-DD",            // last content update (ISO date)
  "imageBase": "https://.../marketing/affiliate-images/", // prefix for `image`
  "ads": [
    {
      "id": "shopee-payday-15",       // unique slug; also the creative filename
      "title": "Shopee Payday Sale: Extra 15% Off Everything",
      "description": "1-2 sentence marketing copy, plain text.",
      "url": "https://invol.co/cl_shopee-payday-15", // deeplink, first-party URL or short link
      "image": "shopee-payday-15.svg", // relative to imageBase
      "partner": "Shopee",            // display name
      "events": ["payday-15"],        // event tags; [] = always eligible
      "tags": ["shopping", "deals"],  // free-form discovery tags
      "accent": "#ee4d2d"             // hex fallback gradient color
    }
  ]
}
```

- `id` must be **unique**, kebab-case, and match the creative filename exactly
  (`<id>.svg`).
- `description` is plain text (no Markdown needed). Keep it to 1–2 sentences.
- `url` may be an Involve Asia deeplink (`https://invol.co/cl_<id>`), a direct
  first-party partner URL, or a short link. All forms are valid.
- `image` is resolved against `imageBase`, so the CDN host can change without
  touching every entry.
- `accent` is a hex color used as the app's fallback gradient when the creative
  fails to load.

### Event-tag vocabulary

`events` is an array of zero or more tags from this **closed** vocabulary:

| Tag | Meaning |
| --- | --- |
| `10-10` | October 10 mega sale |
| `11-11` | November 11 mega sale |
| `12-12` | December 12 mega sale |
| `christmas` | Christmas season |
| `new-year` | New Year season |
| `payday-15` | Mid-month payday (15th) |
| `payday-30` | End-of-month payday (30th) |

An empty array (`[]`) means the ad is **always eligible**. Never invent tags
outside this list; the app matches dates against these exact strings.

## Adding or replacing an ad

1. Add an entry to `affiliate.json#ads` with a new unique `id`.
2. Add `affiliate-images/<id>.svg` (a `640×360` creative).
3. Set `events` using tags from the vocabulary above (or `[]` for always-on).
4. Set `url` to the Involve Asia deeplink (or first-party/short link) and
   `accent` to a matching hex color.
5. Bump `updated` to today's date.
6. Commit and push to `main`.

## Affiliate image conventions (`affiliate-images/`)

- Root: `width="640" height="360" viewBox="0 0 640 360"`.
- Two-color linear-gradient background unique per ad (use the ad's `accent` as
  one stop) + a dark overlay so white text stays legible.
- Partner name (small caps) top-left; event/sale chip top-right (e.g.
  `10.10 SALE`, `PAYDAY`, `CHRISTMAS`, `NEW YEAR`, or `DEAL` for always-on ads).
- Ad title in white, bold ~34px, wrapped to at most 3 lines using `<tspan>`.
- Font stack: `Segoe UI, Arial, sans-serif`. **No external fonts/images.**
- XML-escape `&`, `<`, `>`, `'` in text.

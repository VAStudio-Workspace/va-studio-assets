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

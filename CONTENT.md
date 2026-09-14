# Writing & publishing content

This repo is the SharedCue website and blog, served at `https://sharedcue.github.io/`:

| Page | URL | File |
|---|---|---|
| Home | `/` | `src/pages/index.astro` |
| Blog post list | `/blog/` | `src/pages/blog/index.astro` |
| Blog posts | `/blog/<slug>/` | `src/content/blog/<slug>.md` or `.mdx` |
| Privacy policy | `/privacy/` | `src/pages/privacy.astro` |

## Publish flow (all content)

```
git add -A
git commit -m "post: <what changed>"
git push
```

Push to `main` → GitHub Actions builds the site and publishes it to GitHub Pages (~1–2 min).
Check progress: the repo's **Actions** tab → "Deploy to GitHub Pages".

## Preview locally before pushing

```
npm run dev        # http://localhost:4321/
```

`npm run build` runs the production build into `dist/`.

---

## New blog post

Create `src/content/blog/<url-slug>.md`. The filename becomes the URL:
`src/content/blog/my-post.md` → `/blog/my-post/`.

```markdown
---
title: 'Your title'
description: 'One or two sentences. Shows in previews, search results, and the RSS feed.'
pubDate: 'Sep 05 2026'
---

Write in Markdown. First paragraph, then

## A heading

More text.
```

Frontmatter fields:

| Field | Required | Notes |
|---|---|---|
| `title` | yes | |
| `description` | yes | plain text, no Markdown |
| `pubDate` | yes | `'Mon DD YYYY'` — controls sort order (newest first) |
| `updatedDate` | no | shows "Last updated on ..." |
| `heroImage` | no | `'../../assets/file.jpg'` — see Images below |

The three newest posts also appear under "From the blog" on the home page.

## Posts with app screenshots (MDX)

Use `.mdx` instead of `.md` to show phone screenshots side by side with captions —
see `src/content/blog/getting-started-with-sharedcue.mdx`:

```mdx
import PhoneShots from '../../components/PhoneShots.astro';
import shot from '../../assets/my-guide/01-home.png';

<PhoneShots shots={[{ src: shot, alt: 'Home screen', caption: 'Tap + Add Group' }]} />
```

Keep screenshot files small (resize to ~1000px tall, e.g. `sips -Z 1000 in.png --out out.png`).

## Edit an existing post

Edit the file in `src/content/blog/`. To change a post's URL, rename the file
(old URL will 404 — avoid once a post has traffic).

## Delete a post

`git rm src/content/blog/<slug>.md`

---

## Images

1. Put the file in `src/assets/` (e.g. `src/assets/kitchen.jpg`).
2. Reference it:
   - **Hero:** add `heroImage: '../../assets/kitchen.jpg'` to frontmatter.
   - **In body:** `![alt text](../../assets/kitchen.jpg)`

Astro optimises and resizes them at build time. Prefer `.jpg`/`.png`/`.webp`,
roughly 1600px wide or less.

---

## Static pages

These are `.astro` files in `src/pages/`, not Markdown. The file path is the URL:
`src/pages/support.astro` → `/support/`. Copy `privacy.astro` as a starting point.

To add it to the navigation, edit `src/components/Header.astro` (and `Footer.astro`).

---

## Site-wide settings

| What | Where |
|---|---|
| Site name / description / contact email | `src/consts.ts` |
| Header nav | `src/components/Header.astro` |
| Footer | `src/components/Footer.astro` |
| Favicon | `public/favicon.svg`, `public/favicon.ico` |
| Domain | `astro.config.mjs` (`site`) |
| Deploy workflow | `.github/workflows/deploy.yml` |

# mobinkaram-content

Git-backed content repository for **mobinkaram.ir**.

The website treats this repository as a small headless CMS: posts and media are committed here, while the Next.js application fetches and renders the content.

## Structure

```text
assets/
  backgrounds/        category fallbacks and decorative backgrounds
  icons/              category icons
  images/             post covers and inline media
authors/
  mobin.json          author metadata
categories/
  categories.json     bilingual category metadata
posts/
  en/<category>/*.mdx English posts
  fa/<category>/*.mdx Persian posts
schemas/
  post.schema.json    documented post frontmatter contract
templates/
  post.en.mdx
  post.fa.mdx
content.config.json   repository-level content settings
```

## Post frontmatter

Every MDX post should start with:

```yaml
---
title: "Post title"
description: "Short SEO and card description"
date: "2026-09-19"
slug: "stable-post-slug"
translationKey: "stable-shared-key"
tags: ["tag-one", "tag-two"]
category: "architecture"
cover: "/assets/images/posts/my-post/cover.webp"
readingTime: 8
published: true
author: "Mobin Karam"
---
```

### Rules

- `date` is always stored as Gregorian ISO `YYYY-MM-DD`.
- English public URLs use the Gregorian date.
- Persian public URLs convert the same date to the Persian (Shamsi) calendar.
- Use an English slug for English posts.
- Use a Persian slug for Persian posts.
- Translations should share the same `translationKey`.
- `category` should match a slug in `categories/categories.json`.
- Keep all post media under `assets/images/posts/<translationKey>/`.
- `published: false` keeps drafts out of the public blog.
- Prefer WebP/AVIF for photographs when practical; SVG is suitable for diagrams and illustrations.

## Public URL examples

Given a post stored with `date: "2026-09-19"`:

```text
/en/2026/09/19/first-post-of-my-website
/fa/1405/06/28/اولین-پست-وبسایت-مبین
```

The Shamsi date is derived by the website. Do not maintain a second date field manually.

## Adding a post

1. Copy the appropriate file from `templates/`.
2. Put it in `posts/en/<category>/` or `posts/fa/<category>/`.
3. If the article has a translation, use the same `translationKey` in both files.
4. Upload cover/inline media to `assets/images/posts/<translationKey>/`.
5. Set `published: true` when ready.
6. Commit and push. The website periodically revalidates content from this repository.

## Media URLs

MDX can reference repository media using repository-relative paths:

```md
![Architecture diagram](/assets/images/posts/example/diagram.svg)
```

The website resolves these to the raw GitHub content URL automatically.

## Existing-content compatibility

Older posts in this repository do not all contain `translationKey`, and some cover paths currently point to files that are not present. The website integration is intentionally tolerant:

- it falls back to the file stem when `translationKey` is absent;
- it generates a Persian-safe route slug from the Persian title if an older Persian post still has an English slug;
- it falls back to the category background when a referenced cover file is missing.

New posts should follow the schema above.

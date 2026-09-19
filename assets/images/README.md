# Blog media

Store post-specific media under:

```text
assets/images/posts/<translationKey>/
```

Recommended example:

```text
assets/images/posts/api-versioning-strategies/
  cover.webp
  version-flow.svg
  migration-example.webp
```

Both English and Persian translations may reference the same media directory.

Use repository-relative paths from MDX, for example:

```md
![Version migration flow](/assets/images/posts/api-versioning-strategies/version-flow.svg)
```

The website maps `/assets/...` paths to raw GitHub URLs.

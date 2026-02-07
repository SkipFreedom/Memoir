# MDX Content Architecture

## Proposed directory tree

```
content/
  books/
    high-mountains-deep-oceans/
      volume-1/
        essays/
          0001-crossing-the-pass.mdx
        excerpts/
          0001-opening-pages.mdx
        extras/
          0001-deleted-scene.mdx
      volume-2/
        essays/
        excerpts/
        extras/
  standalone/
    essays/
      0001-silent-coast.mdx
  lead-magnets/
    0001-welcome-excerpt.mdx
  members/
    letters/
      0001-members-letter.mdx
```

- `books/` isolates memoir volumes while allowing multiple content types per volume.
- `standalone/` holds essays not tied to a specific memoir volume.
- `lead-magnets/` is separated to support Phase 1 distribution.
- `extras/` and `members/` exist now for future content without refactors.

## MDX frontmatter conventions

```yaml
---
title: "Crossing the Pass"
slug: "crossing-the-pass"
book:
  series: "high-mountains-deep-oceans"
  volume: "volume-1"
accessTier: "PUBLIC" # AccessTier enum
status: "published" # draft | published
narrative:
  themes: ["grief", "altitude", "homecoming"]
  chronology:
    year: 1997
    season: "winter"
  location: "San Juan Mountains"
  sequence: 12
---
```

Required frontmatter fields:
- `title`
- `slug`
- `accessTier` (required; no default is assumed)
- `status`

Optional fields:
- `book.series`, `book.volume`
- `narrative.themes`, `narrative.chronology`, `narrative.location`, `narrative.sequence`

## Phase activation

- Only **PUBLIC** and **LEAD_MAGNET** access tiers are active in Phase 1.
- Content marked **PURCHASE** or **MEMBER** must not be rendered or linked until backend access controls are implemented.

## Access metadata rule

- `accessTier` is required for all content. No default is assumed to avoid accidental exposure or accidental gating.

## Access logic boundary

- `accessTier` is metadata only; it must never be used inside MDX content to conditionally render, hide, or alter sections of text.
- All access decisions occur outside the content layer.

## Explicit non-patterns

The content layer must not:
- Contain conditional rendering logic.
- Reference user state or session data.
- Include commerce or membership concepts beyond `accessTier` metadata.
- Depend on CMS-only features.

## Runtime content resolution (Phase 1)

- The content system reads MDX files, filters by `status: published`, and uses `accessTier` to determine visibility.
- **PUBLIC** content is always available to render.
- **LEAD_MAGNET** content is only returned to readers with a verified email subscription; the check happens server-side before delivery.
- Access checks are performed at content retrieval time, keeping the client as a rendering surface only.
- **PURCHASE** and **MEMBER** content are stored but inactive in Phase 1; future access rules plug into the same `accessTier` field without changing content structure.

## Preservation goals

- **Narrative continuity**: `narrative.sequence` and chronology fields keep longform entries in coherent order across volumes.
- **Maintainability**: content lives in stable, book-focused folders with explicit metadata and minimal coupling to application logic.
- **Clean separation**: MDX stays purely editorial; access policy lives in server-side checks, not in content files.

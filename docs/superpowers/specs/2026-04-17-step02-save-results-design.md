# Design: Step 02 — Loop and Save to knowledge/raw/

**Date:** 2026-04-17
**Scope:** Extend `scrape_pipeline.py` to write one markdown file per Firecrawl result

## What We're Building

Add a save block to `scrape_pipeline.py` immediately after the existing print loop. No new files, no new functions — inline code only.

## Behavior

- Creates `knowledge/raw/` if it does not exist
- Loops over `results` with a zero-padded index (01, 02, ...)
- Slugifies each result's title: lowercase, non-alphanumeric replaced with `-`, leading/trailing dashes stripped
- Filename format: `NN-title-slug.md`
- Each file begins with a header containing: title, source URL, and date scraped
- If `markdown` is `None` or empty, writes the file with the header and `No content scraped` as the body
- Prints a confirmation line per file written

## File Header Format

```
# {title}

Source: {url}
Date scraped: {YYYY-MM-DD}

---
```

## Implementation Location

Single inline block appended to `scrape_pipeline.py` after the existing print loop. Uses only stdlib (`re`, `time`, `pathlib.Path`) already imported in Step 01.

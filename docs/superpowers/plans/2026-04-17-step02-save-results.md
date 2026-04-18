# Step 02 Save Results Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extend `scrape_pipeline.py` to write one markdown file per Firecrawl result into `knowledge/raw/`.

**Architecture:** Single inline block appended to the existing script after the print loop. Uses only stdlib already imported (`re`, `pathlib.Path`). No new files or functions.

**Tech Stack:** Python stdlib (`re`, `pathlib`, `datetime`), existing `results` list from Step 01.

---

### Task 1: Add the save block to scrape_pipeline.py

**Files:**
- Modify: `scrape_pipeline.py` (append after the existing `for r in results` print loop)

- [ ] **Step 1: Add the save block**

Append the following block to the end of `scrape_pipeline.py`, after the existing print loop:

```python
# --- Step 02: Save results to knowledge/raw/ ---

from datetime import date

out_dir = Path("knowledge/raw")
out_dir.mkdir(parents=True, exist_ok=True)

for i, r in enumerate(results, start=1):
    slug = re.sub(r"[^a-z0-9]+", "-", (r.get("title") or "untitled").lower()).strip("-")
    filename = f"{i:02d}-{slug}.md"
    filepath = out_dir / filename

    title = r.get("title") or "Untitled"
    url = r.get("url") or "Unknown"
    content = r.get("markdown") or ""

    header = f"# {title}\n\nSource: {url}\nDate scraped: {date.today()}\n\n---\n\n"
    body = content if content else "No content scraped"

    filepath.write_text(header + body, encoding="utf-8")
    print(f"  Wrote {filename} ({len(content)} chars)")
```

- [ ] **Step 2: Run the script and verify output**

```bash
python scrape_pipeline.py
```

Expected terminal output (titles/lengths will vary):
```
Firecrawl returned 5 results
  - Chipotle Mexican Grill | Press Releases
    https://ir.chipotle.com/...
    markdown length: 4231 chars
  ...
  Wrote 01-chipotle-mexican-grill-press-releases.md (4231 chars)
  Wrote 02-...md (... chars)
  ...
```

- [ ] **Step 3: Verify files in knowledge/raw/**

Open `knowledge/raw/` in the file explorer. You should see up to 5 `.md` files named `NN-slug.md`. Open one and confirm it starts with:

```
# Chipotle Mexican Grill | Press Releases

Source: https://ir.chipotle.com/...
Date scraped: 2026-04-17

---

<scraped markdown content here>
```

- [ ] **Step 4: Commit**

```bash
git add scrape_pipeline.py knowledge/raw/
git commit -m "feat: add Step 02 save loop to write results to knowledge/raw/"
```

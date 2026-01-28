# Skill Creation Workflow

This document outlines the step-by-step process for creating, packaging, and publishing AI skills.

## Prerequisites

- [skill-seekers](https://github.com/yusufkaraaslan/Skill_Seekers) installed
  ```bash
  pip install skill-seekers
  ```
- Git configured with your identity
- GitHub CLI authenticated (for publishing)

---

## Workflow Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  1. Configure   │───▶│   2. Scrape     │───▶│  3. Enhance     │───▶│  4. Publish     │
│                 │    │                 │    │                 │    │                 │
│  Create JSON    │    │  Run scraper    │    │  Review/edit    │    │  Copy to skills │
│  config file    │    │  on docs site   │    │  SKILL.md       │    │  Git commit     │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
```

---

## Step 1: Create Configuration

Create a config file in `work/configs/<skill-name>.json`:

```json
{
  "name": "my-skill",
  "description": "Brief description of what this library/framework does",
  "base_url": "https://docs.example.com",
  "selectors": {
    "main_content": "article",
    "title": "h1",
    "code_blocks": "pre code"
  },
  "url_patterns": {
    "include": ["/docs"],
    "exclude": ["/blog", "/v1/", "/v2/"]
  },
  "categories": {
    "api": ["/api/"],
    "guides": ["/guide/", "/tutorial/"]
  },
  "max_pages": 200,
  "rate_limit": 0.3
}
```

### Configuration Options

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Skill identifier (lowercase, hyphens) | `"lightweight-charts"` |
| `description` | Brief description | `"Financial charting library"` |
| `base_url` | Documentation root URL | `"https://docs.example.com"` |
| `selectors.main_content` | CSS selector for main content | `"article"`, `"main"`, `".content"` |
| `selectors.title` | CSS selector for page title | `"h1"`, `".page-title"` |
| `selectors.code_blocks` | CSS selector for code | `"pre code"` |
| `url_patterns.include` | URL patterns to include | `["/docs", "/api"]` |
| `url_patterns.exclude` | URL patterns to skip | `["/blog", "/old-version"]` |
| `categories` | Group pages by URL pattern | `{"api": ["/api/"]}` |
| `max_pages` | Maximum pages to scrape | `200` |
| `rate_limit` | Seconds between requests | `0.3` |

### Common Selectors by Framework

| Docs Framework | main_content | title |
|----------------|--------------|-------|
| Docusaurus | `"article"` | `"h1"` |
| MkDocs | `".md-content"` | `"h1"` |
| GitBook | `".page-inner"` | `"h1"` |
| Sphinx | `".document"` | `"h1"` |
| VuePress | `".theme-default-content"` | `"h1"` |
| Nextra | `"article"` | `"h1"` |

---

## Step 2: Scrape Documentation

Run the scraper:

```bash
skill-seekers scrape --config work/configs/my-skill.json
```

### What happens:
1. Checks for `llms.txt` (faster if available)
2. Falls back to HTML scraping
3. Crawls pages following links
4. Extracts content, code blocks, headings
5. Saves to `work/output/my-skill_data/` (raw JSON)
6. Builds skill in `work/output/my-skill/`

### Output structure:
```
work/output/my-skill/
├── SKILL.md           # Quick reference (auto-generated)
└── references/
    ├── index.md       # Table of contents
    ├── api.md         # API documentation (if categorized)
    ├── guides.md      # Guides (if categorized)
    └── other.md       # Uncategorized pages
```

### Troubleshooting

| Issue | Solution |
|-------|----------|
| No content extracted | Check `main_content` selector matches the site |
| Missing pages | Increase `max_pages` or adjust `url_patterns` |
| Timeout errors | Increase `rate_limit` (e.g., `0.5` or `1.0`) |
| Duplicate content | Add old versions to `exclude` patterns |

---

## Step 3: Enhance the Skill

### Review SKILL.md

The auto-generated `SKILL.md` needs improvement. Edit to add:

- Better description and use cases
- Organized quick reference tables
- Clean, formatted code examples
- Installation instructions
- Common patterns section

### Quality Check

Run the packager to check quality:

```bash
skill-seekers package work/output/my-skill/ --no-open
```

Review the quality report and fix any warnings.

---

## Step 4: Publish to Repository

### Copy to skills folder

```bash
# Copy the skill folder (not the zip)
cp -r work/output/my-skill skills/

# Remove any generated zips (keep it clean)
rm -f skills/*.zip work/output/*.zip
```

### Update marketplace.json

Add the new skill:

```json
{
  "id": "my-skill",
  "name": "My Skill Display Name",
  "description": "What it does",
  "version": "1.0",
  "category": "frontend",
  "source": "https://docs.example.com",
  "path": "skills/my-skill"
}
```

### Update README.md

Add to the appropriate category table in the README.

### Commit and Push

```bash
git add skills/my-skill marketplace.json README.md
git commit -m "Add my-skill - description here"
git push
```

---

## Quick Reference Commands

```bash
# Create new skill
skill-seekers scrape --config work/configs/my-skill.json

# Check quality
skill-seekers package work/output/my-skill/ --no-open

# Estimate pages before scraping
skill-seekers estimate --config work/configs/my-skill.json

# Resume interrupted scrape
skill-seekers resume work/output/my-skill_data/

# Generate config interactively
skill-seekers config
```

---

## File Locations

| What | Where |
|------|-------|
| Scraper configs | `work/configs/*.json` |
| Raw scraped data | `work/output/*_data/` |
| Built skills (working) | `work/output/*/` |
| Published skills | `skills/*/` |
| Marketplace manifest | `marketplace.json` |
| Plugin manifest | `.claude-plugin/plugin.json` |

---

## Tips

1. **Start small** - Set `max_pages: 50` first to test selectors
2. **Exclude old versions** - Add `/v1/`, `/v2/` etc. to exclude patterns
3. **Check the site** - Inspect the docs site to find the right selectors
4. **Review output** - Always review and enhance the auto-generated SKILL.md
5. **Keep it clean** - Don't commit zip files, only the extracted skill folders

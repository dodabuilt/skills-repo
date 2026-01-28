# Skills Repo

AI Skills generated from documentation using [skill-seekers](https://github.com/yusufkaraaslan/Skill_Seekers). This repository serves as both a skill library and a Claude Code plugin marketplace.

## Repository Structure

```
skills-repo/
├── .claude-plugin/          # Claude Code plugin manifest
│   └── plugin.json
├── skills/                  # Packaged skills (ready to use)
│   ├── lightweight-charts/  # Skill folder with SKILL.md
│   │   ├── SKILL.md
│   │   └── references/
│   └── *.zip               # Packaged zip files for Claude.ai
├── work/                   # Working directory (raw scraping)
│   ├── configs/            # Scraper configurations
│   └── output/             # Raw scraper output
├── marketplace.json        # Plugin marketplace manifest
└── README.md
```

## Available Skills

### Lightweight Charts
TradingView Lightweight Charts - A free, lightweight (~45kb), and performant financial charting library for creating interactive stock/crypto charts.

- **Version:** 5.1
- **Pages scraped:** 248
- **API coverage:** Functions, Interfaces, Enumerations, Type Aliases, Variables

## Usage

### As a Claude Code Plugin

Install this repo as a Claude Code plugin:

```bash
claude --plugin-dir /path/to/skills-repo
```

Or add to your marketplace configuration.

### As Claude.ai Skills

Upload the zip files from `skills/` to [Claude.ai Skills](https://claude.ai/skills).

### Using Individual Skills

Copy the skill folder (e.g., `skills/lightweight-charts/`) to your project's `.claude/skills/` directory.

## Creating New Skills

1. Create a config in `work/configs/`:
```json
{
  "name": "my-skill",
  "description": "Description here",
  "base_url": "https://docs.example.com",
  "selectors": {
    "main_content": "article",
    "title": "h1",
    "code_blocks": "pre code"
  },
  "url_patterns": {
    "include": [],
    "exclude": []
  },
  "max_pages": 100
}
```

2. Run the scraper:
```bash
skill-seekers scrape --config work/configs/my-skill.json
```

3. Package the skill:
```bash
skill-seekers package work/output/my-skill/
```

4. Copy to skills folder:
```bash
cp work/output/my-skill.zip skills/
cp -r work/output/my-skill skills/
```

## License

MIT

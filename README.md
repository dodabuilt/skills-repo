# AI Skills Repository

A curated collection of AI skills that give language models deep knowledge of libraries, frameworks, and tools. Skills are generated from official documentation using [skill-seekers](https://github.com/yusufkaraaslan/Skill_Seekers).

## What are AI Skills?

AI Skills are structured knowledge packages that help AI assistants (Claude, Cursor, ChatGPT, etc.) understand and work with specific technologies. Each skill contains:

- **SKILL.md** - Quick reference with common patterns and examples
- **references/** - Comprehensive API documentation and guides
- **Packaged .zip** - Ready to upload to Claude.ai or other platforms

## Available Skills

| Skill | Description | Version | Category |
|-------|-------------|---------|----------|
| [Lightweight Charts](skills/lightweight-charts/) | TradingView financial charting library | 5.1 | Charting |

*More skills coming soon!*

## Repository Structure

```
ai-skills/
├── .claude-plugin/          # Claude Code plugin configuration
│   └── plugin.json
├── skills/                  # 📦 Ready-to-use skills
│   ├── <skill-name>/        # Each skill has its own folder
│   │   ├── SKILL.md         # Quick reference & examples
│   │   └── references/      # Full documentation
│   └── <skill-name>.zip     # Packaged for upload
├── work/                    # 🔧 Working directory
│   ├── configs/             # Scraper configurations
│   └── output/              # Raw scraper output
├── marketplace.json         # Plugin marketplace manifest
└── README.md
```

## Usage

### Option 1: Claude Code Plugin

Load as a Claude Code plugin to get skills automatically:

```bash
claude --plugin-dir /path/to/skills-repo
```

### Option 2: Upload to Claude.ai

1. Go to [Claude.ai Skills](https://claude.ai/skills)
2. Click "Upload Skill"
3. Select a `.zip` file from `skills/`

### Option 3: Copy to Project

Copy a skill folder to your project's `.claude/skills/` directory:

```bash
cp -r skills/lightweight-charts ~/.claude/skills/
```

### Option 4: Cursor AI

Add skills to your Cursor workspace:

```bash
cp -r skills/lightweight-charts /path/to/project/.cursor/skills/
```

## Creating New Skills

### 1. Create a Configuration

Create `work/configs/<skill-name>.json`:

```json
{
  "name": "my-skill",
  "description": "Description of the library/framework",
  "base_url": "https://docs.example.com",
  "selectors": {
    "main_content": "article",
    "title": "h1",
    "code_blocks": "pre code"
  },
  "url_patterns": {
    "include": ["/docs"],
    "exclude": ["/blog", "/old-version"]
  },
  "categories": {
    "api": ["/api/"],
    "guides": ["/guide/", "/tutorial/"]
  },
  "max_pages": 200,
  "rate_limit": 0.3
}
```

### 2. Scrape Documentation

```bash
skill-seekers scrape --config work/configs/my-skill.json
```

### 3. Package the Skill

```bash
skill-seekers package work/output/my-skill/ --no-open
```

### 4. Add to Skills Directory

```bash
cp work/output/my-skill.zip skills/
cp -r work/output/my-skill skills/
```

### 5. Update Marketplace

Add the new skill to `marketplace.json`:

```json
{
  "id": "my-skill",
  "name": "My Skill Name",
  "description": "What it does",
  "version": "1.0",
  "category": "category-name",
  "source": "https://docs.example.com",
  "path": "skills/my-skill"
}
```

## Skill Ideas

Want to contribute? Here are some libraries that would make great skills:

**Frontend**
- React, Vue, Svelte, Angular
- Tailwind CSS, Shadcn/ui
- Three.js, D3.js

**Backend**
- FastAPI, Express, NestJS
- Prisma, Drizzle, TypeORM
- tRPC, GraphQL

**DevOps**
- Docker, Kubernetes
- Terraform, Pulumi
- GitHub Actions

**AI/ML**
- LangChain, LlamaIndex
- Hugging Face Transformers
- PyTorch, TensorFlow

## Contributing

1. Fork this repository
2. Create a skill using the steps above
3. Submit a pull request

## License

MIT - Feel free to use, modify, and distribute.

---

Built with [skill-seekers](https://github.com/yusufkaraaslan/Skill_Seekers) 🔍

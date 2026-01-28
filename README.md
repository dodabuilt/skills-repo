# IBKR Engineering Skills

A curated skill set for AI-assisted IBKR frontend development with **Claude Code**. These skills constrain Claude to use **only** the approved design system, components, and patterns.

## What's Included

| Skill | Purpose |
|-------|---------|
| **ibkr-development** | Core skill - Ratio UI, semantic tokens, PrimeVue, Fluidity, IBFonts |
| **tanstack-query-vue** | Data fetching with TanStack Query (Vue 3 Composition API) |
| **lightweight-charts** | TradingView charting library integration |

## Installation

### Option 1: Global Installation (All Projects)

Install as a Claude Code plugin for all your projects:

```bash
# Clone to your Claude plugins directory
git clone https://github.com/dodabuilt/skills-repo.git ~/.claude/plugins/ibkr-skills
```

The plugin is now available globally. Claude Code will automatically discover and use these skills.

### Option 2: Per-Project Installation

Add skills to a specific project:

```bash
# From your project root
mkdir -p .claude/skills
git clone https://github.com/dodabuilt/skills-repo.git .claude/skills/ibkr
```

Or add as a git submodule:
```bash
git submodule add https://github.com/dodabuilt/skills-repo.git .claude/skills/ibkr
```

### Option 3: Symlink (Best for Development)

```bash
# Clone once
git clone https://github.com/dodabuilt/skills-repo.git ~/skills-repo

# Symlink to Claude plugins
ln -s ~/skills-repo ~/.claude/plugins/ibkr-skills

# Or symlink to a specific project
ln -s ~/skills-repo/.claude/skills/ibkr /path/to/project/.claude/skills/ibkr
```

## Verify Installation

After installing, run Claude Code and ask:
```
What skills do you have available for IBKR development?
```

Claude should list the installed skills.

## Usage

Once installed, Claude will automatically use these skills when you:

- Ask to build UI components
- Work with Ratio UI or PrimeVue
- Implement Figma designs
- Fetch data from APIs
- Create charts or data visualizations

### Trigger Keywords

The skills activate on mentions of:
- `Ratio`, `PrimeVue`, `IBKR`, `trading UI`
- `semantic tokens`, `design system`
- `Fluidity`, `microfrontend`
- `useQuery`, `useMutation`, `TanStack Query`
- `lightweight-charts`, `candlestick`

### Example Prompts

```
Build a positions table using Ratio UI that fetches data from /api/positions
```

```
Create a trading card component with buy/sell buttons following the IBKR design system
```

```
Implement this Figma design using our component library
```

## What the Skills Enforce

### Always Use
- Semantic color tokens (`bg-surface`, `text-on-surface`, `border-accent`)
- PrimeVue components with Ratio presets
- TanStack Query for all data fetching
- IBFonts for icons (`ibi-*` classes)
- Ratio UI composables (`useDevice`, `useLocale`, `useNum`, etc.)

### Never Use
- Arbitrary Tailwind values (`w-[200px]`, `text-[14px]`)
- Hex colors or standard Tailwind colors (`bg-blue-500`)
- Custom SVGs or third-party icon libraries
- Manual `fetch()` + `ref()` patterns
- Custom components when PrimeVue/Ratio has one

## Updating Skills

```bash
cd ~/.claude/plugins/ibkr-skills  # or wherever you cloned
git pull origin main
```

## Skill Structure

```
skills-repo/
├── .claude-plugin/
│   └── plugin.json                 # Plugin manifest
├── skills/
│   ├── ibkr-development/
│   │   ├── SKILL.md                # Main skill file
│   │   └── references/
│   │       ├── semantic-tokens.md  # All color tokens
│   │       ├── ratio-components.md # Component reference
│   │       ├── fluidity-patterns.md# Microfrontend patterns
│   │       └── ibfonts.md          # Icon library
│   ├── tanstack-query-vue/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── api.md              # Full API reference
│   └── lightweight-charts/
│       ├── SKILL.md
│       └── references/
│           └── ...
└── README.md
```

## Contributing

To add or update skills:

1. Edit the relevant `SKILL.md` or reference files
2. Test with Claude Code to verify the skill activates correctly
3. Submit a PR

## License

MIT

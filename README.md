# IBKR Engineering Skills Plugin

A Claude Code plugin with curated skills for IBKR frontend development. These skills constrain Claude to use **only** the approved design system, components, and patterns.

## Skills Included

| Skill | Triggers On |
|-------|-------------|
| **ibkr-development** | Ratio UI, PrimeVue, IBKR, trading UI, semantic tokens, Fluidity, microfrontends |
| **tanstack-query-vue** | useQuery, useMutation, TanStack Query, Vue Query, server state |
| **lightweight-charts** | Candlestick charts, TradingView, financial charts |

## Installation

### Option 1: Add as Marketplace (Recommended)

Add this repo as a marketplace, then install the plugin:

```bash
# In Claude Code, run:
/plugin marketplace add dodabuilt/skills-repo

# Then install the plugin:
/plugin install ibkr-engineering-skills@dodabuilt-skills-repo
```

### Option 2: Direct Install from GitHub

```bash
/plugin install dodabuilt/skills-repo
```

### Option 3: Local Development/Testing

Clone and load directly with `--plugin-dir`:

```bash
# Clone the repo
git clone https://github.com/dodabuilt/skills-repo.git ~/skills-repo

# Start Claude Code with the plugin loaded
claude --plugin-dir ~/skills-repo
```

### Option 4: Per-Project Installation

Add to a specific project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": ["dodabuilt/skills-repo"],
  "enabledPlugins": ["ibkr-engineering-skills@dodabuilt-skills-repo"]
}
```

## Verify Installation

After installing, run in Claude Code:

```
/plugin
```

Go to the **Installed** tab - you should see `ibkr-engineering-skills`.

Or test with:
```
What IBKR development skills do you have?
```

## Usage

Once installed, Claude automatically uses these skills when relevant. Skills are namespaced:

```bash
# Invoke directly (optional - Claude uses them automatically)
/ibkr-engineering-skills:ibkr-development
/ibkr-engineering-skills:tanstack-query-vue
/ibkr-engineering-skills:lightweight-charts
```

### Example Prompts

```
Build a positions table with buy/sell buttons using Ratio UI
```

```
Fetch portfolio data using TanStack Query and display in a DataTable
```

```
Create a candlestick chart for AAPL with volume histogram
```

## What the Skills Enforce

### Always Use
- Semantic color tokens (`bg-surface`, `text-on-surface`, `border-accent`)
- PrimeVue components with Ratio presets
- TanStack Query for all data fetching (`useQuery`, `useMutation`)
- IBFonts for icons (`ibi-*` classes)
- Ratio UI composables (`useDevice`, `useLocale`, `useNum`, etc.)

### Never Use
- Arbitrary Tailwind values (`w-[200px]`, `text-[14px]`)
- Hex colors or standard Tailwind colors (`bg-blue-500`)
- Custom SVGs or third-party icon libraries
- Manual `fetch()` + `ref()` patterns
- Custom components when PrimeVue/Ratio has one

## Updating

```bash
# If using marketplace:
/plugin marketplace update dodabuilt-skills-repo

# If using local clone:
cd ~/skills-repo && git pull
```

## Plugin Structure

```
skills-repo/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── skills/
│   ├── ibkr-development/
│   │   ├── SKILL.md             # Main skill
│   │   └── references/
│   │       ├── semantic-tokens.md
│   │       ├── ratio-components.md
│   │       ├── fluidity-patterns.md
│   │       └── ibfonts.md
│   ├── tanstack-query-vue/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── api.md
│   └── lightweight-charts/
│       ├── SKILL.md
│       └── references/
│           └── ...
└── README.md
```

## Contributing

1. Edit the relevant `SKILL.md` or reference files
2. Test locally with `claude --plugin-dir ./skills-repo`
3. Submit a PR

## License

MIT

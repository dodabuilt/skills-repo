# IBKR Engineering Skills

A Claude Code plugin marketplace with curated skills for IBKR frontend development. These skills constrain Claude to use **only** the approved design system, components, and patterns.

## Skills Included

| Skill | Triggers On |
|-------|-------------|
| **ibkr-development** | Ratio UI, PrimeVue, IBKR, trading UI, semantic tokens, Fluidity, microfrontends |
| **tanstack-query-vue** | useQuery, useMutation, TanStack Query, Vue Query, server state |
| **lightweight-charts** | Candlestick charts, TradingView, financial charts |

## Installation

### Option 1: Add as Marketplace (Recommended)

```bash
# Add the marketplace
/plugin marketplace add dodabuilt/skills-repo

# Install the plugin
/plugin install ibkr-engineering-skills@dodabuilt-skills-repo
```

### Option 2: Local Development/Testing

```bash
# Clone the repo
git clone https://github.com/dodabuilt/skills-repo.git ~/skills-repo

# Start Claude Code with the plugin loaded
claude --plugin-dir ~/skills-repo/plugins/ibkr-engineering-skills
```

### Option 3: Per-Project Configuration

Add to your project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": ["dodabuilt/skills-repo"],
  "enabledPlugins": ["ibkr-engineering-skills@dodabuilt-skills-repo"]
}
```

## Verify Installation

After installing, run:

```bash
/plugin
```

Go to the **Installed** tab - you should see `ibkr-engineering-skills`.

Or ask Claude:
```
What skills do you have for IBKR development?
```

## Usage

Once installed, Claude automatically uses these skills when relevant. Skills are namespaced by plugin:

```bash
# Direct invocation (optional - skills trigger automatically)
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
# Update marketplace catalog
/plugin marketplace update dodabuilt-skills-repo
```

## Repository Structure

```
skills-repo/
├── .claude-plugin/
│   └── marketplace.json              # Marketplace catalog
├── plugins/
│   └── ibkr-engineering-skills/      # Plugin directory
│       ├── .claude-plugin/
│       │   └── plugin.json           # Plugin metadata
│       └── skills/
│           ├── ibkr-development/
│           │   ├── SKILL.md
│           │   └── references/
│           ├── tanstack-query-vue/
│           │   ├── SKILL.md
│           │   └── references/
│           └── lightweight-charts/
│               ├── SKILL.md
│               └── references/
└── README.md
```

## Contributing

1. Edit files in `plugins/ibkr-engineering-skills/skills/`
2. Test locally with `claude --plugin-dir ./plugins/ibkr-engineering-skills`
3. Submit a PR

## License

MIT

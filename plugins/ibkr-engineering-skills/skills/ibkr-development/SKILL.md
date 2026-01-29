---
name: ibkr-development
description: Use when building UI for IBKR applications, working with Ratio UI components, implementing Figma designs, or creating Vue 3 components for trading platforms. Triggers on mentions of Ratio, PrimeVue, IBKR, trading UI, semantic tokens, Fluidity, or microfrontends.
---

# IBKR Development

Build production-ready UI using the IBKR design system. This skill constrains you to use ONLY what exists in Ratio UI - no improvisation, no arbitrary values, no custom components when standard ones exist.

## Critical Guardrails

**NEVER DO:**
- Arbitrary Tailwind values: `w-[200px]`, `h-[3rem]`, `text-[14px]`, `p-[20px]`
- Hex/RGB colors: `#4B5563`, `rgb(75, 85, 99)`, `text-gray-600`
- Custom components when Ratio/PrimeVue has one
- Inline styles for colors, spacing, or layout
- Standard Tailwind color classes: `bg-blue-500`, `text-red-600`, `border-gray-300`
- Custom SVGs or inline SVG icons - use IBFonts (`ibi-*`) classes only
- PrimeIcons (`pi pi-*`) - use IBFonts instead
- Manual `fetch()` + `ref()` patterns for server data - use TanStack Query

**ALWAYS DO:**
- Use semantic tokens: `bg-surface`, `text-on-surface`, `border-accent`
- Use Tailwind preset spacing: `p-2`, `gap-4`, `m-auto`, `px-4`
- Use PrimeVue components with Ratio presets
- Use composables from `@ibgroup-tws-webapps/ratio.ui`
- Wrap app in `<IbApplication>` or `<RatioDesignProvider>`
- Use TanStack Query (`@tanstack/vue-query`) for all data fetching and server state

## Semantic Color Tokens (Required)

Use these tokens instead of arbitrary colors. All support `bg-`, `text-`, `border-` prefixes.

### Surface Tokens (Backgrounds)
| Token | Use For |
|-------|---------|
| `surface` | Main content background |
| `surface-low` | Sunken/recessed areas |
| `surface-high` | Elevated areas |
| `surface-container` | Card/panel backgrounds |
| `surface-container-low` | Subtle containers |
| `surface-container-high` | Emphasized containers |
| `surface-inverse` | Dark on light / light on dark |

### Text Tokens
| Token | Use For |
|-------|---------|
| `on-surface` | Primary text |
| `on-surface-secondary` | Secondary/muted text |
| `on-surface-tertiary` | Subtle text, hints |
| `on-surface-disabled` | Disabled state text |
| `on-surface-inverse` | Text on inverse surfaces |

### Trading Tokens
| Token | Use For |
|-------|---------|
| `buy` / `on-buy` | Buy actions, long positions |
| `sell` / `on-sell` | Sell actions, short positions |
| `positive` / `on-positive` | Gains, up movements |
| `negative` / `on-negative` | Losses, down movements |

### Status Tokens
| Token | Use For |
|-------|---------|
| `accent` / `on-accent` | Primary actions, links |
| `success` / `on-success` | Success states |
| `error` / `on-error` | Error states |
| `warning` / `on-warning` | Warning states |
| `alert` / `on-alert` | Alert/danger states |
| `important` / `on-important` | Important highlights |
| `ai` / `on-ai` | AI-related features |

### Token Variants
All tokens support state variants:
- Base: `bg-accent`
- Hover: `bg-accent-hover`
- Pressed: `bg-accent-pressed`
- Subtle: `bg-accent-subtle`, `bg-accent-subtle-hover`
- Disabled: `bg-accent-disabled`
- Inverse: `bg-accent-inverse`

### Border Tokens
Use directly (no `border-` prefix needed):
- `border`, `border-subtle`, `border-strong`, `border-strongest`
- `border-accent`, `border-buy`, `border-sell`
- `border-positive`, `border-negative`
- `border-success`, `border-error`, `border-warning`

## Component Selection Guide

### Use PrimeVue Components (via Ratio presets)

| Need | Use | NOT |
|------|-----|-----|
| Button | `<Button>` | Custom `<button>` with classes |
| Dropdown | `<Select>` | Custom dropdown |
| Input | `<InputText>` | `<input>` |
| Table | `<DataTable>` | Custom `<table>` |
| Tabs | `<Tabs>` / `<IbButtonTabs>` | Custom tab implementation |
| Modal/Dialog | `<Dialog>` | Custom modal |
| Accordion | `<Accordion>` | Custom collapsible |
| Tooltip | `<Tooltip>` | Custom tooltip |
| Loading | `<ProgressSpinner>` | Custom spinner |
| Toast | `<Toast>` | Custom notification |

### Use Ratio Custom Components

| Component | Purpose |
|-----------|---------|
| `<IbApplication>` | Root app wrapper (required) |
| `<RatioDesignProvider>` | Design system context |
| `<IbGrid>` | Grid layout system |
| `<IbSurface>` | Semantic surface container |
| `<IbEmptyState>` | Empty state displays |
| `<IbErrorState>` | Error state displays |
| `<IbChartLoadingAndError>` | Chart loading/error states |
| `<IbFlag>` | Country flags |
| `<IbCompanyLogo>` | Company logos |
| `<IbInfoTooltip>` | Info icon with tooltip |
| `<IbButtonTabs>` | Button-style tabs |

### Charts (Use Ratio Wrappers)

| Chart Type | Component |
|------------|-----------|
| Candlestick/Line | `<IbLightweightCharts>` |
| Sparkline | `<IbSparkline>` |
| Pie/Donut | `<IbPieChart>` |
| Bar | `<IbBarChart>` |
| Heatmap | `<IbHeatmap>` |
| Radar | `<IbRadarChart>` |
| Bubble | `<IbBubbleChart>` |
| Geographic | `<IbGeomap>` |

## Composables Reference

```typescript
import {
  useApp,           // App-level state and configuration
  useDevice,        // Device detection (mobile, tablet, desktop)
  useLocale,        // Locale and i18n
  useNum,           // Number formatting
  useDate,          // Date formatting
  useRequester,     // HTTP requests with auth
  useIsDarkMode,    // Dark mode detection
  useColorMap,      // Color mapping utilities
  useVersion,       // App version info
  useSemanticBaseToken, // Dynamic semantic token access
  useRuntime,       // Runtime configuration
  useMedia,         // Media queries
  useUtil,          // General utilities
} from '@ibgroup-tws-webapps/ratio.ui';
```

## Data Fetching (TanStack Query)

Always use TanStack Query for API calls. Combine with `useRequester()` for authenticated requests.

```vue
<script setup lang="ts">
import { useQuery, useMutation, useQueryClient } from '@tanstack/vue-query'
import { useRequester } from '@ibgroup-tws-webapps/ratio.ui'

const { request } = useRequester()
const queryClient = useQueryClient()

// Fetch data
const { data, isPending, isError, error } = useQuery({
  queryKey: ['positions'],
  queryFn: () => request('/api/positions'),
  staleTime: 30 * 1000, // 30 seconds
})

// Mutations
const { mutate, isPending: isMutating } = useMutation({
  mutationFn: (order) => request('/api/orders', { method: 'POST', body: order }),
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['positions'] })
  },
})
</script>

<template>
  <IbChartLoadingAndError :loading="isPending" :error="error">
    <DataTable :value="data" />
  </IbChartLoadingAndError>
</template>
```

**Key patterns:**
- `useQuery` for GET requests (auto-caching, background refetch)
- `useMutation` for POST/PUT/DELETE
- `queryClient.invalidateQueries()` after mutations
- Combine `isPending` with `<IbChartLoadingAndError>` for consistent loading states

## App Structure Template

```vue
<template>
  <IbApplication>
    <!-- Your app content -->
    <div class="flex flex-col gap-4 p-4 bg-surface">
      <header class="text-on-surface">
        <!-- Header content -->
      </header>
      <main class="bg-surface-container rounded-lg p-4">
        <!-- Main content -->
      </main>
    </div>
  </IbApplication>
</template>

<script setup lang="ts">
import { IbApplication } from '@ibgroup-tws-webapps/ratio.ui';
</script>
```

## Fluidity Microfrontend Pattern

When building a module that will be served as a microfrontend:

```vue
<template>
  <div class="module-container">
    <!-- Module content -->
  </div>
</template>

<script setup lang="ts">
import { useFluidityProps, useFluidityEmit, useFluidityCriticalErrors } from '@ibgroup-tws-webapps/fluidity';
import { ref, computed } from 'vue';

// Base props with defaults
const baseProps = {
  title: 'My Module',
  showHeader: true,
};

// Merge with injected moduleProps
const props = useFluidityProps(baseProps);

// Emit events to parent shell
const emit = useFluidityEmit();

// Signal critical errors to hide module
const hasError = ref(false);
useFluidityCriticalErrors([hasError]);
</script>
```

## Common Patterns

### Card/Panel Layout
```vue
<div class="bg-surface-container rounded-lg p-4 border border-subtle">
  <h2 class="text-on-surface text-lg font-semibold mb-2">Title</h2>
  <p class="text-on-surface-secondary">Description</p>
</div>
```

### Trading Action Buttons
```vue
<div class="flex gap-2">
  <Button severity="info" class="bg-buy text-on-buy">Buy</Button>
  <Button severity="danger" class="bg-sell text-on-sell">Sell</Button>
</div>
```

### Price Change Display
```vue
<span :class="change >= 0 ? 'text-positive' : 'text-negative'">
  {{ change >= 0 ? '+' : '' }}{{ change }}%
</span>
```

### Loading State
```vue
<IbChartLoadingAndError :loading="isLoading" :error="error">
  <template #default>
    <!-- Content when loaded -->
  </template>
</IbChartLoadingAndError>
```

## Common Mistakes

### Styling Mistakes

| Mistake | Fix |
|---------|-----|
| `text-gray-600` | Use `text-on-surface-secondary` |
| `bg-blue-500` | Use `bg-accent` or `bg-buy` |
| `text-red-500` | Use `text-negative` or `text-sell` |
| `text-green-500` | Use `text-positive` or `text-buy` |
| `border-gray-200` | Use `border-subtle` |
| `w-[200px]` | Use `w-48` or `w-52` (preset values) |
| `p-[10px]` | Use `p-2` or `p-3` (preset values) |
| `text-[14px]` | Use `text-sm` or `text-base` |
| `#4B5563` or `rgb(...)` | Use semantic token |

### Component Mistakes

| Mistake | Fix |
|---------|-----|
| `<button class="...">` | Use `<Button>` from PrimeVue |
| `<select>...</select>` | Use `<Select>` from PrimeVue |
| `<input type="text">` | Use `<InputText>` from PrimeVue |
| `<table>...</table>` | Use `<DataTable>` from PrimeVue |
| Custom modal div | Use `<Dialog>` from PrimeVue |
| Custom spinner SVG | Use `<ProgressSpinner>` from PrimeVue |
| Custom tooltip | Use `v-tooltip` directive |
| Custom tabs | Use `<Tabs>` or `<IbButtonTabs>` |

### Architecture Mistakes

| Mistake | Fix |
|---------|-----|
| No wrapper component | Wrap in `<IbApplication>` |
| Direct `fetch()` calls | Use `useRequester()` with TanStack Query |
| Manual dark mode check | Use `useIsDarkMode()` composable |
| Hardcoded locale | Use `useLocale()` composable |
| Manual number formatting | Use `useNum()` composable |
| Not using Fluidity props | Use `useFluidityProps()` in microfrontends |

### Data Fetching Mistakes

| Mistake | Fix |
|---------|-----|
| `ref()` + `onMounted()` + `fetch()` | Use `useQuery()` from TanStack Query |
| Manual loading/error state | TanStack Query provides `isPending`, `isError` |
| Manual cache invalidation | Use `queryClient.invalidateQueries()` |
| `watch()` to refetch on param change | Use reactive `queryKey` in `useQuery()` |
| Manual polling with `setInterval` | Use `refetchInterval` option |
| `try/catch` for every fetch | TanStack Query handles errors automatically |
| Storing server data in Pinia/Vuex | Use TanStack Query for server state |

### Trading UI Mistakes

| Mistake | Fix |
|---------|-----|
| Green for buy | Use `bg-buy` (blue in IBKR design) |
| Using `success` for gains | Use `positive` for P&L gains |
| Using `error` for losses | Use `negative` for P&L losses |
| Generic button for buy/sell | Use semantic `bg-buy`/`bg-sell` tokens |

### Icon Mistakes

| Mistake | Fix |
|---------|-----|
| `<i class="pi pi-search">` | Use `<i class="ibi-search">` (IBFonts) |
| `<svg>...</svg>` inline icons | Use `<i class="ibi-{name}">` |
| Importing `.svg` files | Use IBFonts CSS classes |
| Creating custom icon components | Use IBFonts - request new icons if missing |
| `<img src="icon.svg">` | Use `<i class="ibi-{name}">` |
| Heroicons, FontAwesome, etc. | Use IBFonts only |
| Forgetting to await font config | Always `await configureIconsAndFonts()` |
| Using emoji for icons | Use IBFonts icon classes |

**If an icon doesn't exist in IBFonts**, request it be added to the library. Do NOT create custom SVGs.

### Red Flags - Stop and Reconsider

If you find yourself doing any of these, STOP:

- Writing a custom component that "looks like" a PrimeVue component
- Using `style="..."` for colors or spacing
- Creating CSS variables for colors
- Using Tailwind arbitrary values `[...]`
- Importing colors from a constants file
- Writing media queries manually (use `useDevice()` or `useMedia()`)
- Building a loading spinner from scratch
- Using PrimeIcons (`pi pi-*`) instead of IBFonts (`ibi-*`)
- Writing `<svg>` tags or importing SVG files for icons
- Installing third-party icon libraries (Heroicons, FontAwesome, Lucide, etc.)
- Creating a "custom icon component" with inline SVG
- Writing `const data = ref(); onMounted(async () => { data.value = await fetch(...) })` - use TanStack Query
- Creating a Pinia/Vuex store just to cache API responses - use TanStack Query

## Validation Checklist

Before submitting code, verify:

- [ ] No arbitrary Tailwind values (`w-[...]`, `h-[...]`, `text-[...]`)
- [ ] No hex colors or standard Tailwind colors
- [ ] Using semantic tokens for all colors
- [ ] Using PrimeVue/Ratio components (not custom HTML)
- [ ] App wrapped in `<IbApplication>` or `<RatioDesignProvider>`
- [ ] Composables imported from `@ibgroup-tws-webapps/ratio.ui`
- [ ] If microfrontend: using Fluidity composables
- [ ] All API data fetching uses TanStack Query (`useQuery`, `useMutation`)
- [ ] No manual `ref()` + `onMounted()` + `fetch()` patterns

## Reference Files

For detailed documentation:
- **Semantic Tokens**: See `references/semantic-tokens.md` for complete token list
- **Components**: See `references/ratio-components.md` for all components and props
- **Fluidity**: See `references/fluidity-patterns.md` for microfrontend patterns
- **Icons & Fonts**: See `references/ibfonts.md` for icon library and text fonts
- **Data Fetching**: See `@tanstack-query-vue` skill for complete TanStack Query API

## Icon Usage (IBFonts)

```typescript
import { configureIconsAndFonts, Icons, ICON_TYPES } from '@ibgroup-tws-webapps/ibfonts';

// Configure in app setup (await this!)
await configureIconsAndFonts(ICON_TYPES.GLYPH);  // Outlined icons
await configureIconsAndFonts(ICON_TYPES.MATERIAL); // Filled icons
```

Use icons via CSS classes with `ibi-` prefix:
```html
<i class="ibi-search"></i>
<i class="ibi-alert text-warning"></i>
<i class="ibi-check text-positive"></i>
```

Common icons: `ibi-search`, `ibi-home`, `ibi-menu`, `ibi-close`, `ibi-check`, `ibi-alert`, `ibi-info`, `ibi-plus`, `ibi-minus`, `ibi-edit`, `ibi-delete`, `ibi-filter`, `ibi-sort`, `ibi-chart`

**DO NOT** use PrimeIcons (`pi pi-*`) - use IBFonts (`ibi-*`) for design system consistency.

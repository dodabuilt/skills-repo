# IBFonts Reference

Icon and text font library for IBKR applications. Provides 600+ icons in two styles plus comprehensive text font support.

## Installation

```bash
npm install @ibgroup-tws-webapps/ibfonts
```

## Quick Setup

```typescript
import { configureIconsAndFonts, ICON_TYPES } from '@ibgroup-tws-webapps/ibfonts';

// In your app initialization (e.g., main.ts)
await configureIconsAndFonts(ICON_TYPES.GLYPH);
```

## Icon Types

| Type | Constant | Style | Use For |
|------|----------|-------|---------|
| Glyph | `ICON_TYPES.GLYPH` | Outlined | Default, most UI contexts |
| Material | `ICON_TYPES.MATERIAL` | Filled | Emphasis, active states |

```typescript
import { configureIcons, ICON_TYPES } from '@ibgroup-tws-webapps/ibfonts';

// Glyph icons (outlined)
await configureIcons(ICON_TYPES.GLYPH);

// Material icons (filled)
await configureIcons(ICON_TYPES.MATERIAL);
```

## Configuration Functions

| Function | Purpose |
|----------|---------|
| `configureIconsAndFonts(type?, url?)` | Load both icons and text fonts |
| `configureIcons(type?, url?)` | Load only icon fonts |
| `configureFonts(url?)` | Load only text fonts |

```typescript
// Default: glyph icons + all text fonts from IBKR CDN
await configureIconsAndFonts();

// Material icons only, no text fonts
await configureIcons(ICON_TYPES.MATERIAL);

// Text fonts only
await configureFonts();

// Custom CDN URL
await configureIconsAndFonts(ICON_TYPES.GLYPH, 'https://custom-cdn.com/ibfonts');
```

## Using Icons in HTML

After configuration, use icons via CSS classes with the `ibi-` prefix:

```html
<!-- Basic icon -->
<i class="ibi-search"></i>

<!-- Icon in button -->
<button>
  <i class="ibi-plus"></i>
  Add Item
</button>

<!-- Icon with text -->
<span>
  <i class="ibi-alert"></i>
  Warning message
</span>

<!-- Sizing with Tailwind -->
<i class="ibi-home text-lg"></i>
<i class="ibi-home text-2xl"></i>

<!-- Coloring with semantic tokens -->
<i class="ibi-check text-positive"></i>
<i class="ibi-close text-negative"></i>
<i class="ibi-info text-accent"></i>
```

## Using Icons Programmatically

```typescript
import { Icons, createIcon, getIconClass } from '@ibgroup-tws-webapps/ibfonts';

// Create an icon element
const searchIcon = createIcon(Icons.SEARCH);
document.querySelector('#btn').appendChild(searchIcon);

// Get the CSS class for an icon
const className = getIconClass(Icons.ALERT); // Returns 'ibi-alert'
```

## Common Icons Reference

### Navigation & Actions
| Icon | Constant | Class |
|------|----------|-------|
| Search | `Icons.SEARCH` | `ibi-search` |
| Home | `Icons.HOME` | `ibi-home` |
| Menu | `Icons.MENU` | `ibi-menu` |
| Close | `Icons.CLOSE` | `ibi-close` |
| Back | `Icons.BACK` | `ibi-back` |
| Settings | `Icons.SETTINGS` | `ibi-settings` |

### Status & Feedback
| Icon | Constant | Class |
|------|----------|-------|
| Check | `Icons.CHECK` | `ibi-check` |
| Alert | `Icons.ALERT` | `ibi-alert` |
| Info | `Icons.INFO` | `ibi-info` |
| Warning | `Icons.WARNING` | `ibi-warning` |
| Error | `Icons.ERROR` | `ibi-error` |

### Trading
| Icon | Constant | Class |
|------|----------|-------|
| Chart | `Icons.CHART` | `ibi-chart` |
| Portfolio | `Icons.PORTFOLIO` | `ibi-portfolio` |
| Trade | `Icons.TRADE` | `ibi-trade` |
| Order | `Icons.ORDER` | `ibi-order` |

### Data & Content
| Icon | Constant | Class |
|------|----------|-------|
| Plus | `Icons.PLUS` | `ibi-plus` |
| Minus | `Icons.MINUS` | `ibi-minus` |
| Edit | `Icons.EDIT` | `ibi-edit` |
| Delete | `Icons.DELETE` | `ibi-delete` |
| Download | `Icons.DOWNLOAD` | `ibi-download` |
| Upload | `Icons.UPLOAD` | `ibi-upload` |
| Filter | `Icons.FILTER` | `ibi-filter` |
| Sort | `Icons.SORT` | `ibi-sort` |

## Text Fonts by Platform

IBFonts includes text fonts for each platform:

| Platform | Font Family | Weights |
|----------|-------------|---------|
| iOS | `SourceSans3` | ExtraLight, Light, Regular, Semibold, Bold, Black |
| Android | `Roboto` | Thin, Light, Regular, Medium, Bold, Black |
| Web | `ProximaNova` | Regular, Medium, Semibold, Bold |
| Web (Alt) | `CBABeaconSans` | Regular, Bold |
| NTWS | `SourceSans3` | Light, Regular, Semibold, Bold, Black |
| GlobalTrader | `UniversNextforHSBC` | Regular, Medium |
| Impact | `Proximasoft` | Regular, Medium, Semibold, Bold |

Usage in CSS:
```css
/* Text fonts are automatically loaded */
.ios-text {
  font-family: 'SourceSans3', sans-serif;
}

.android-text {
  font-family: 'Roboto', sans-serif;
}

.web-text {
  font-family: 'ProximaNova', sans-serif;
}
```

## Configuration Options

```typescript
import { configure, setLogLevel, IBFontsConfig } from '@ibgroup-tws-webapps/ibfonts';

// Full configuration
configure({
  logLevel: 'silent',           // 'debug' | 'info' | 'warn' | 'error' | 'silent'
  requestTimeout: 5000,         // Request timeout in ms
  enableCacheBusting: false,    // Add timestamps to URLs
  defaultBaseUrl: 'https://custom-cdn.com',
});

// Quick log level change
setLogLevel('error');

// Access current config
console.log(IBFontsConfig.defaultBaseUrl);
```

## Vue Component Pattern

```vue
<template>
  <div class="flex items-center gap-2">
    <i :class="iconClass" class="text-accent"></i>
    <span>{{ label }}</span>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { Icons, getIconClass } from '@ibgroup-tws-webapps/ibfonts';

const props = defineProps<{
  icon: keyof typeof Icons;
  label: string;
}>();

const iconClass = computed(() => getIconClass(Icons[props.icon]));
</script>
```

## App Initialization Pattern

```typescript
// main.ts
import { createApp } from 'vue';
import { configureIconsAndFonts, ICON_TYPES } from '@ibgroup-tws-webapps/ibfonts';
import App from './App.vue';

async function bootstrap() {
  // Load fonts before mounting app
  await configureIconsAndFonts(ICON_TYPES.GLYPH);
  
  const app = createApp(App);
  app.mount('#app');
}

bootstrap();
```

## Cleanup (for testing or SPA unmount)

```typescript
import { cleanupAllStyles } from '@ibgroup-tws-webapps/ibfonts';

// Remove all injected styles
const removedCount = cleanupAllStyles();
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using PrimeIcons (`pi pi-*`) | Use IBFonts (`ibi-*`) for consistency |
| Inline `<svg>` icons | Use `<i class="ibi-{name}">` |
| Importing `.svg` files | Use IBFonts CSS classes |
| `<img src="icon.svg">` | Use `<i class="ibi-{name}">` |
| Third-party icons (Heroicons, etc.) | Use IBFonts only |
| Creating custom icon components | Request icon be added to IBFonts |
| Forgetting to await configuration | Always `await configureIconsAndFonts()` |
| Using deprecated `configureFontPath` | Use `configureIconsAndFonts()` |
| Hardcoding icon class strings | Use `Icons` enum for type safety |

## Critical Rule

**NEVER use custom SVGs or third-party icon libraries.** IBFonts is the ONLY icon source.

If an icon is missing from IBFonts:
1. Check if a similar icon exists (search the 600+ available icons)
2. If truly missing, request it be added to the `svg/glyph/` or `svg/material/` folder
3. Do NOT create a workaround with inline SVG or external libraries

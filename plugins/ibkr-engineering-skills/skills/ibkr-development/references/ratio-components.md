# Ratio UI Components Reference

Complete reference for all components available in the IBKR design system.

## Installation

```bash
npm install @ibgroup-tws-webapps/ratio.ui
```

## Required Wrapper Components

### IbApplication

Root application wrapper. **Required** for all IBKR applications.

```vue
<template>
  <IbApplication :setup-s-s-o="true" :footer-config="footerConfig">
    <slot />
  </IbApplication>
</template>

<script setup>
import { IbApplication } from '@ibgroup-tws-webapps/ratio.ui';

const footerConfig = {
  gfisConfig: {
    showFooter: true,
    useDarkModeLogo: false,
    onClick: () => console.log('GFIS clicked')
  },
  disclosureConfig: {
    showFooter: true,
    text: 'Important Disclosure',
    onClick: () => console.log('Disclosure clicked')
  }
};
</script>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `setupSSO` | `boolean` | Enable SSO authentication setup |
| `footerConfig` | `object` | Footer configuration (GFIS, disclosures) |
| `setViewportMeta` | `boolean` | Auto-set mobile viewport meta tag |

### RatioDesignProvider

Design system context provider. Wraps content with design tokens.

```vue
<template>
  <RatioDesignProvider dir="ltr">
    <slot />
  </RatioDesignProvider>
</template>
```

**Props:**
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dir` | `'ltr' \| 'rtl'` | `'ltr'` | Text direction |

### RatioDesignExclude

Excludes content from design system styling (escape hatch for legacy content).

```vue
<RatioDesignExclude>
  <!-- Legacy content here won't have Ratio styles -->
</RatioDesignExclude>
```

---

## Layout Components

### IbGrid

Responsive grid layout system.

```vue
<IbGrid :columns="3" :gap="4">
  <div>Cell 1</div>
  <div>Cell 2</div>
  <div>Cell 3</div>
</IbGrid>
```

### IbSurface

Semantic surface container with automatic styling.

```vue
<IbSurface is="section" :level="1">
  Content on elevated surface
</IbSurface>
```

**Props:**
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `is` | `string` | `'div'` | HTML element to render |
| `level` | `number` | `0` | Surface elevation level |

---

## Display Components

### IbEmptyState

Display empty state with icon, title, and description.

```vue
<IbEmptyState
  icon="pi pi-inbox"
  title="No Data"
  description="There's nothing to display yet"
/>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `icon` | `string` | PrimeIcons class |
| `title` | `string` | Main title text |
| `description` | `string` | Description text |

### IbErrorState

Display error state.

```vue
<IbErrorState
  title="Something went wrong"
  description="Please try again later"
  :retry="handleRetry"
/>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `title` | `string` | Error title |
| `description` | `string` | Error description |
| `retry` | `() => void` | Retry callback function |

### IbChartLoadingAndError

Wrapper for charts with loading and error states.

```vue
<IbChartLoadingAndError
  :loading="isLoading"
  :error="errorMessage"
  :height="300"
>
  <template #default>
    <IbSparkline :data="chartData" />
  </template>
</IbChartLoadingAndError>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `loading` | `boolean` | Show loading spinner |
| `error` | `string \| null` | Error message to display |
| `height` | `number` | Container height in pixels |

---

## Data Display Components

### IbFlag

Display country flags.

```vue
<IbFlag country="US" :size="24" />
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `country` | `string` | ISO country code |
| `size` | `number` | Size in pixels |

### IbCompanyLogo

Display company logos with fallback.

```vue
<IbCompanyLogo
  :symbol="'AAPL'"
  :size="LogoSize.MD"
  :color="LogoColor.DEFAULT"
/>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `symbol` | `string` | Stock symbol |
| `size` | `LogoSize` | `SM`, `MD`, `LG`, `XL` |
| `color` | `LogoColor` | `DEFAULT`, `LIGHT`, `DARK` |

### IbInfoTooltip

Info icon with tooltip on hover.

```vue
<IbInfoTooltip content="Helpful information here" />
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `content` | `string` | Tooltip content |
| `position` | `string` | `'top'`, `'bottom'`, `'left'`, `'right'` |

### IbExternalPositionHolderBadge

Badge for external position holders.

```vue
<IbExternalPositionHolderBadge :count="3" />
```

---

## Navigation Components

### IbButtonTabs

Button-style tab navigation.

```vue
<IbButtonTabs
  v-model="activeTab"
  :tabs="tabs"
  active-tab-class="bg-accent-subtle text-accent"
/>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `modelValue` | `string` | Active tab key (v-model) |
| `tabs` | `IbButtonTabsTab[]` | Tab definitions |
| `activeTabClass` | `string` | CSS class for active tab |

**Tab Interface:**
```typescript
interface IbButtonTabsTab {
  key: string;
  label: string;
  disabled?: boolean;
}
```

---

## Chart Components

### IbLightweightCharts

TradingView Lightweight Charts wrapper.

```vue
<IbLightweightCharts
  :options="chartOptions"
  :series="seriesData"
  :height="400"
/>
```

**Note:** For detailed Lightweight Charts usage, see the `lightweight-charts` skill.

### IbSparkline

Mini inline chart.

```vue
<IbSparkline
  :data="[10, 15, 8, 20, 12]"
  :width="100"
  :height="30"
  :positive="true"
/>
```

**Props:**
| Prop | Type | Description |
|------|------|-------------|
| `data` | `number[]` | Data points |
| `width` | `number` | Width in pixels |
| `height` | `number` | Height in pixels |
| `positive` | `boolean` | Use positive (green) or negative (red) color |

### IbPieChart

Pie/donut chart.

```vue
<IbPieChart
  :data="pieData"
  :options="{ cutout: '50%' }"
/>
```

### IbBarChart

Bar chart.

```vue
<IbBarChart
  :data="barData"
  :options="chartOptions"
/>
```

### IbHeatmap

Heatmap visualization.

```vue
<IbHeatmap
  :data="heatmapData"
  :rows="rows"
  :columns="columns"
/>
```

### IbRadarChart

Radar/spider chart.

```vue
<IbRadarChart
  :data="radarData"
  :options="radarOptions"
/>
```

### IbBubbleChart

Bubble chart.

```vue
<IbBubbleChart
  :data="bubbleData"
  :options="bubbleOptions"
/>
```

### IbGeomap

Geographic map visualization.

```vue
<IbGeomap
  :data="geoData"
  :options="mapOptions"
/>
```

---

## PrimeVue Components (With Ratio Presets)

These PrimeVue components are pre-configured with IBKR design system presets.

### Button

```vue
<Button label="Click Me" severity="primary" />
<Button label="Secondary" severity="secondary" outlined />
<Button icon="pi pi-check" rounded />
```

**Severities:** `primary`, `secondary`, `success`, `info`, `warn`, `danger`, `contrast`

### Select (Dropdown)

```vue
<Select
  v-model="selected"
  :options="options"
  option-label="name"
  option-value="id"
  placeholder="Select an option"
/>
```

### InputText

```vue
<InputText v-model="value" placeholder="Enter text" />
```

### DataTable

```vue
<DataTable :value="data" :paginator="true" :rows="10">
  <Column field="name" header="Name" sortable />
  <Column field="value" header="Value" sortable />
</DataTable>
```

### Tabs

```vue
<Tabs v-model:value="activeIndex">
  <TabList>
    <Tab value="0">Tab 1</Tab>
    <Tab value="1">Tab 2</Tab>
  </TabList>
  <TabPanels>
    <TabPanel value="0">Content 1</TabPanel>
    <TabPanel value="1">Content 2</TabPanel>
  </TabPanels>
</Tabs>
```

### Dialog

```vue
<Dialog v-model:visible="visible" header="Dialog Title" :modal="true">
  <p>Dialog content</p>
</Dialog>
```

### Accordion

```vue
<Accordion>
  <AccordionPanel value="0">
    <AccordionHeader>Section 1</AccordionHeader>
    <AccordionContent>Content 1</AccordionContent>
  </AccordionPanel>
</Accordion>
```

### Toast

```vue
<script setup>
import { useToast } from 'primevue/usetoast';
const toast = useToast();

const showSuccess = () => {
  toast.add({ severity: 'success', summary: 'Success', detail: 'Operation completed' });
};
</script>

<template>
  <Toast />
  <Button label="Show Toast" @click="showSuccess" />
</template>
```

### ProgressSpinner

```vue
<ProgressSpinner />
```

### Tooltip

```vue
<Button v-tooltip="'Tooltip text'" label="Hover me" />
```

### Tag

```vue
<Tag value="Active" severity="success" />
<Tag value="Pending" severity="warn" />
<Tag value="Error" severity="danger" />
```

**Severities:** `primary`, `success`, `info`, `warn`, `danger`, `secondary`

### Chip

```vue
<Chip label="Apple" removable />
```

### Avatar

```vue
<Avatar label="JD" size="large" shape="circle" />
<Avatar image="/path/to/image.jpg" />
```

### Skeleton

```vue
<Skeleton width="100%" height="2rem" />
<Skeleton shape="circle" size="4rem" />
```

### Slider

```vue
<Slider v-model="value" :min="0" :max="100" />
```

### Timeline

```vue
<Timeline :value="events" align="alternate">
  <template #content="{ item }">
    {{ item.status }}
  </template>
</Timeline>
```

### Panel

```vue
<Panel header="Panel Title" toggleable>
  <p>Panel content</p>
</Panel>
```

### Drawer (Sidebar)

```vue
<Drawer v-model:visible="visible" header="Drawer Title">
  <p>Drawer content</p>
</Drawer>
```

---

## Component Import Pattern

```typescript
// Ratio UI Components
import {
  IbApplication,
  RatioDesignProvider,
  IbGrid,
  IbSurface,
  IbEmptyState,
  IbErrorState,
  IbChartLoadingAndError,
  IbFlag,
  IbCompanyLogo,
  IbInfoTooltip,
  IbButtonTabs,
  IbSparkline,
  IbPieChart,
  IbBarChart,
  IbHeatmap,
  IbRadarChart,
  IbBubbleChart,
  IbLightweightCharts,
  IbGeomap,
} from '@ibgroup-tws-webapps/ratio.ui';

// PrimeVue components are auto-imported when using unplugin-vue-components
// with @primevue/auto-import-resolver
```

---

## Best Practices

1. **Always wrap your app** in `<IbApplication>` or at minimum `<RatioDesignProvider>`
2. **Use semantic tokens** for all colors - never hardcode hex values
3. **Prefer Ratio components** over building custom ones
4. **Use PrimeVue components** for standard UI patterns (buttons, inputs, tables)
5. **Use composables** from ratio.ui for app-level concerns (locale, device, etc.)

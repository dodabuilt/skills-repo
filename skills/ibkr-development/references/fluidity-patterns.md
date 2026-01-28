# Fluidity Microfrontend Patterns

Guide for building Vue modules that will be served as microfrontends via Fluidity.

## Overview

Fluidity is IBKR's microfrontend architecture that allows Vue modules to be dynamically loaded into a shell application. Modules are self-contained and communicate with the parent shell via props injection and event emission.

## Installation

```bash
npm install @ibgroup-tws-webapps/fluidity
```

## Core Concepts

### Module Architecture

```
Shell Application (Host)
└── Fluidity Container
    ├── Module A (Remote)
    ├── Module B (Remote)
    └── Module C (Remote)
```

- **Shell**: The host application that loads and orchestrates modules
- **Module**: A self-contained Vue application served as a UMD or ES module
- **Fluidity Container**: Component that handles module loading and lifecycle

### Communication Flow

```
Shell App
    │
    ├── Injects: moduleProps (configuration)
    │
    └── Receives: moduleEmit (events)
```

## Composables

### useFluidityProps

Merge base props with injected `moduleProps` from the shell.

```typescript
import { useFluidityProps } from '@ibgroup-tws-webapps/fluidity';

// Define your module's base/default props
const baseProps = {
  title: 'My Module',
  showHeader: true,
  theme: 'light',
  apiEndpoint: '/api/default',
};

// Get merged props (moduleProps overrides baseProps)
const props = useFluidityProps(baseProps);

// Access in template
// {{ props.value.title }}
```

**How it works:**
1. Shell injects `moduleProps` via Vue's provide/inject
2. `useFluidityProps` merges `moduleProps` with your `baseProps`
3. `baseProps` values take precedence (allows local override)
4. Returns a computed ref that updates reactively

### useFluidityEmit

Emit events to the parent shell application.

```typescript
import { useFluidityEmit } from '@ibgroup-tws-webapps/fluidity';

const emit = useFluidityEmit();

// Emit custom events to shell
emit('onDataLoaded', { items: 42 });
emit('onUserAction', { action: 'click', target: 'button' });
emit('onError', { message: 'Something went wrong' });
```

**Common events:**
| Event | Payload | Purpose |
|-------|---------|---------|
| `onDataLoaded` | `{ count: number }` | Signal data is ready |
| `onUserAction` | `{ action: string, ... }` | User interaction events |
| `onError` | `{ message: string }` | Non-critical errors |
| `onNavigate` | `{ route: string }` | Request navigation |
| `onResize` | `{ height: number }` | Request container resize |

### useFluidityCriticalErrors

Signal critical errors that should hide the module entirely.

```typescript
import { useFluidityCriticalErrors } from '@ibgroup-tws-webapps/fluidity';
import { ref } from 'vue';

const authError = ref(false);
const dataError = ref(false);
const networkError = ref(false);

// Pass all error refs - if ANY is true, module hides
useFluidityCriticalErrors([authError, dataError, networkError]);

// Later, when a critical error occurs:
const handleAuthFailure = () => {
  authError.value = true; // Module will hide
};
```

**When to use critical errors:**
- Authentication failures
- Required data unavailable
- Unrecoverable API errors
- Permission denied

**NOT for:**
- Transient loading states
- User-recoverable errors (show `<IbErrorState>` instead)
- Validation errors

## Complete Module Template

```vue
<template>
  <div class="module-root bg-surface">
    <!-- Loading State -->
    <div v-if="isLoading" class="flex items-center justify-center p-8">
      <ProgressSpinner />
    </div>

    <!-- Error State -->
    <IbErrorState
      v-else-if="error"
      :title="error.title"
      :description="error.message"
      :retry="handleRetry"
    />

    <!-- Main Content -->
    <template v-else>
      <header v-if="props.showHeader" class="p-4 border-b border-subtle">
        <h1 class="text-on-surface text-lg font-semibold">
          {{ props.title }}
        </h1>
      </header>

      <main class="p-4">
        <!-- Your module content -->
        <slot />
      </main>
    </template>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';
import { 
  useFluidityProps, 
  useFluidityEmit, 
  useFluidityCriticalErrors 
} from '@ibgroup-tws-webapps/fluidity';
import { IbErrorState } from '@ibgroup-tws-webapps/ratio.ui';
import ProgressSpinner from 'primevue/progressspinner';

// Props with defaults
interface ModuleProps {
  title: string;
  showHeader: boolean;
  apiEndpoint: string;
}

const baseProps: ModuleProps = {
  title: 'My Module',
  showHeader: true,
  apiEndpoint: '/api/v1/data',
};

const props = useFluidityProps(baseProps);
const emit = useFluidityEmit();

// State
const isLoading = ref(true);
const error = ref<{ title: string; message: string } | null>(null);
const criticalError = ref(false);

// Critical error handling
useFluidityCriticalErrors([criticalError]);

// Data fetching
const fetchData = async () => {
  isLoading.value = true;
  error.value = null;

  try {
    const response = await fetch(props.value.apiEndpoint);
    
    if (!response.ok) {
      if (response.status === 401) {
        criticalError.value = true; // Hide module
        return;
      }
      throw new Error(`HTTP ${response.status}`);
    }

    const data = await response.json();
    emit('onDataLoaded', { count: data.length });
    
  } catch (e) {
    error.value = {
      title: 'Failed to load data',
      message: e instanceof Error ? e.message : 'Unknown error',
    };
    emit('onError', { message: error.value.message });
  } finally {
    isLoading.value = false;
  }
};

const handleRetry = () => {
  fetchData();
};

onMounted(() => {
  fetchData();
});
</script>

<style scoped>
.module-root {
  min-height: 200px;
}
</style>
```

## Module Entry Point

Your module's entry point should export the component for UMD loading:

```typescript
// src/index.ts
import { createApp, type App } from 'vue';
import ModuleRoot from './ModuleRoot.vue';
import PrimeVue from 'primevue/config';
import { ibThemes } from '@ibgroup-tws-webapps/ratio.ui';

// For ES module usage
export { ModuleRoot as default };

// For UMD mount function (used by Fluidity)
export function mount(container: HTMLElement, props?: Record<string, unknown>): App {
  const app = createApp(ModuleRoot, props);
  
  app.use(PrimeVue, {
    theme: {
      preset: ibThemes.web,
      options: {
        darkModeSelector: '.ratio-dark',
      },
    },
  });
  
  app.mount(container);
  return app;
}

export function unmount(app: App): void {
  app.unmount();
}
```

## Vite Configuration for Module Build

```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import dts from 'vite-plugin-dts';

export default defineConfig({
  plugins: [
    vue(),
    dts({ insertTypesEntry: true }),
  ],
  build: {
    lib: {
      entry: 'src/index.ts',
      name: 'MyModule',
      formats: ['es', 'umd'],
      fileName: (format) => `my-module.${format}.js`,
    },
    rollupOptions: {
      external: ['vue', 'primevue'],
      output: {
        globals: {
          vue: 'Vue',
          primevue: 'PrimeVue',
        },
      },
    },
  },
});
```

## Shell Integration

How the shell loads your module:

```vue
<!-- In the shell application -->
<template>
  <FluidityModule
    :url="moduleUrl"
    :props="moduleConfig"
    @on-data-loaded="handleDataLoaded"
    @on-error="handleError"
  >
    <template #loading>
      <FluidityDefaultLoader />
    </template>
  </FluidityModule>
</template>

<script setup>
import { FluidityModule, FluidityDefaultLoader } from '@ibgroup-tws-webapps/fluidity';

const moduleUrl = 'https://cdn.example.com/my-module.umd.js';
const moduleConfig = {
  title: 'Custom Title',
  apiEndpoint: '/api/v2/data',
};

const handleDataLoaded = (payload) => {
  console.log('Module loaded data:', payload.count);
};

const handleError = (payload) => {
  console.error('Module error:', payload.message);
};
</script>
```

## Testing Modules Locally

Run your module standalone during development:

```vue
<!-- src/DevApp.vue -->
<template>
  <IbApplication>
    <!-- Simulate shell props injection -->
    <div :key="propsKey">
      <ModuleRoot />
    </div>
  </IbApplication>
</template>

<script setup>
import { provide, ref } from 'vue';
import { IbApplication } from '@ibgroup-tws-webapps/ratio.ui';
import ModuleRoot from './ModuleRoot.vue';

// Simulate moduleProps from shell
const mockModuleProps = {
  title: 'Dev Mode',
  showHeader: true,
  apiEndpoint: '/api/mock/data',
};

provide('moduleProps', mockModuleProps);
provide('moduleEmit', (event: string, payload: unknown) => {
  console.log('[DEV] Module emit:', event, payload);
});

const propsKey = ref(0);
</script>
```

## Best Practices

### DO:
- Keep modules focused on a single feature/concern
- Handle all error states gracefully
- Use `useFluidityCriticalErrors` for unrecoverable failures
- Emit meaningful events for shell integration
- Test with simulated shell props locally

### DON'T:
- Access window/document directly (breaks SSR)
- Assume specific shell behavior
- Store global state that persists across mounts
- Import large dependencies (they won't be shared)
- Use route params directly (pass via props)

### Error Handling Strategy

```
Error Type              → Action
─────────────────────────────────────────
Authentication (401)    → criticalError = true (hide module)
Permission (403)        → criticalError = true (hide module)
Not Found (404)         → Show <IbErrorState> with retry
Server Error (5xx)      → Show <IbErrorState> with retry
Network Error           → Show <IbErrorState> with retry
Validation Error        → Show inline error, don't hide
```

## TypeScript Support

Define your module's prop types:

```typescript
// types.ts
export interface MyModuleProps {
  title: string;
  showHeader?: boolean;
  apiEndpoint?: string;
  theme?: 'light' | 'dark';
}

// Usage
const baseProps: MyModuleProps = {
  title: 'Default Title',
  showHeader: true,
};

const props = useFluidityProps(baseProps);
// props.value is typed as Partial<MyModuleProps> & MyModuleProps
```

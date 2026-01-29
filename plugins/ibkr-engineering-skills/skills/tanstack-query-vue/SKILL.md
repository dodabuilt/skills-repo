---
name: tanstack-query-vue
description: Use when fetching data in Vue 3 applications, managing server state, caching API responses, handling mutations, or implementing infinite scroll. Triggers on useQuery, useMutation, TanStack Query, Vue Query, or server state management.
---

# TanStack Query for Vue 3

Server state management library for Vue 3 Composition API. Handles data fetching, caching, synchronization, and background updates.

## Installation

```bash
npm install @tanstack/vue-query
```

## Setup (main.ts)

```typescript
import { createApp } from 'vue'
import { VueQueryPlugin } from '@tanstack/vue-query'
import App from './App.vue'

const app = createApp(App)

app.use(VueQueryPlugin, {
  queryClientConfig: {
    defaultOptions: {
      queries: {
        staleTime: 5 * 60 * 1000,  // 5 minutes
        gcTime: 10 * 60 * 1000,    // 10 minutes (formerly cacheTime)
        retry: 3,
        refetchOnWindowFocus: false,
      },
    },
  },
})

app.mount('#app')
```

## Core Hooks

| Hook | Purpose |
|------|---------|
| `useQuery` | Fetch and cache data |
| `useMutation` | Create, update, delete data |
| `useInfiniteQuery` | Paginated/infinite scroll data |
| `useQueryClient` | Access query client for invalidation |

## useQuery - Fetching Data

```vue
<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query'

interface Todo {
  id: number
  title: string
  completed: boolean
}

const fetchTodos = async (): Promise<Todo[]> => {
  const response = await fetch('/api/todos')
  if (!response.ok) throw new Error('Failed to fetch')
  return response.json()
}

const { 
  data,           // Reactive data
  isPending,      // Initial loading (no cached data)
  isError,        // Error occurred
  error,          // Error object
  isFetching,     // Background refetch in progress
  isSuccess,      // Query succeeded
  refetch,        // Manual refetch function
} = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
})
</script>

<template>
  <div v-if="isPending">Loading...</div>
  <div v-else-if="isError">Error: {{ error?.message }}</div>
  <ul v-else>
    <li v-for="todo in data" :key="todo.id">{{ todo.title }}</li>
  </ul>
</template>
```

## Query Keys

Query keys identify cached data. Use arrays with descriptive segments:

```typescript
// Simple key
useQuery({ queryKey: ['todos'], queryFn: fetchTodos })

// Key with parameters
useQuery({ queryKey: ['todo', todoId], queryFn: () => fetchTodo(todoId) })

// Key with filters
useQuery({ 
  queryKey: ['todos', { status: 'completed', page: 1 }], 
  queryFn: () => fetchTodos({ status: 'completed', page: 1 })
})
```

## useMutation - Modifying Data

```vue
<script setup lang="ts">
import { useMutation, useQueryClient } from '@tanstack/vue-query'

interface NewTodo {
  title: string
}

const queryClient = useQueryClient()

const createTodo = async (newTodo: NewTodo) => {
  const response = await fetch('/api/todos', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newTodo),
  })
  if (!response.ok) throw new Error('Failed to create')
  return response.json()
}

const { 
  mutate,         // Trigger mutation
  mutateAsync,    // Trigger mutation (returns Promise)
  isPending,      // Mutation in progress
  isError,        // Mutation failed
  error,          // Error object
  isSuccess,      // Mutation succeeded
  reset,          // Reset mutation state
} = useMutation({
  mutationFn: createTodo,
  onSuccess: () => {
    // Invalidate and refetch todos after successful mutation
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
  onError: (error) => {
    console.error('Mutation failed:', error)
  },
})

const handleSubmit = (title: string) => {
  mutate({ title })
}
</script>
```

## useInfiniteQuery - Infinite Scroll

```vue
<script setup lang="ts">
import { useInfiniteQuery } from '@tanstack/vue-query'

interface Page {
  data: Todo[]
  nextCursor?: number
}

const fetchTodosPage = async ({ pageParam = 0 }): Promise<Page> => {
  const response = await fetch(`/api/todos?cursor=${pageParam}&limit=10`)
  return response.json()
}

const {
  data,
  fetchNextPage,
  hasNextPage,
  isFetchingNextPage,
  isPending,
} = useInfiniteQuery({
  queryKey: ['todos', 'infinite'],
  queryFn: fetchTodosPage,
  initialPageParam: 0,
  getNextPageParam: (lastPage) => lastPage.nextCursor,
})
</script>

<template>
  <div v-if="isPending">Loading...</div>
  <template v-else>
    <div v-for="page in data?.pages" :key="page.nextCursor">
      <div v-for="todo in page.data" :key="todo.id">
        {{ todo.title }}
      </div>
    </div>
    <button 
      @click="fetchNextPage()" 
      :disabled="!hasNextPage || isFetchingNextPage"
    >
      {{ isFetchingNextPage ? 'Loading...' : hasNextPage ? 'Load More' : 'No more' }}
    </button>
  </template>
</template>
```

## Query Invalidation

```typescript
import { useQueryClient } from '@tanstack/vue-query'

const queryClient = useQueryClient()

// Invalidate specific query
queryClient.invalidateQueries({ queryKey: ['todos'] })

// Invalidate all queries starting with 'todos'
queryClient.invalidateQueries({ queryKey: ['todos'], exact: false })

// Invalidate and refetch immediately
queryClient.refetchQueries({ queryKey: ['todos'] })

// Set query data directly (optimistic update)
queryClient.setQueryData(['todos'], (old) => [...old, newTodo])
```

## Dependent Queries

```typescript
// Second query depends on first query's data
const { data: user } = useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
})

const { data: projects } = useQuery({
  queryKey: ['projects', user.value?.id],
  queryFn: () => fetchProjects(user.value!.id),
  enabled: computed(() => !!user.value?.id),  // Only run when user is loaded
})
```

## Query Options

| Option | Type | Description |
|--------|------|-------------|
| `queryKey` | `array` | Unique key for caching |
| `queryFn` | `function` | Function that fetches data |
| `enabled` | `boolean \| Ref<boolean>` | Enable/disable query |
| `staleTime` | `number` | Time until data is considered stale (ms) |
| `gcTime` | `number` | Time to keep unused data in cache (ms) |
| `retry` | `number \| boolean` | Retry failed queries |
| `retryDelay` | `number \| function` | Delay between retries |
| `refetchOnMount` | `boolean` | Refetch when component mounts |
| `refetchOnWindowFocus` | `boolean` | Refetch when window regains focus |
| `refetchInterval` | `number` | Poll interval (ms) |
| `select` | `function` | Transform/select data |
| `placeholderData` | `any` | Placeholder while loading |

## Optimistic Updates

```typescript
const { mutate } = useMutation({
  mutationFn: updateTodo,
  onMutate: async (newTodo) => {
    // Cancel outgoing refetches
    await queryClient.cancelQueries({ queryKey: ['todos'] })
    
    // Snapshot previous value
    const previousTodos = queryClient.getQueryData(['todos'])
    
    // Optimistically update
    queryClient.setQueryData(['todos'], (old) => 
      old.map(t => t.id === newTodo.id ? newTodo : t)
    )
    
    return { previousTodos }
  },
  onError: (err, newTodo, context) => {
    // Rollback on error
    queryClient.setQueryData(['todos'], context?.previousTodos)
  },
  onSettled: () => {
    // Always refetch after error or success
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
})
```

## Common Patterns

### Loading States
```vue
<template>
  <div v-if="isPending" class="flex justify-center p-4">
    <ProgressSpinner />
  </div>
  <div v-else-if="isError" class="text-error">
    {{ error?.message }}
  </div>
  <div v-else>
    <!-- Content -->
  </div>
</template>
```

### Refetch on Action
```vue
<script setup>
const { data, refetch, isFetching } = useQuery({
  queryKey: ['data'],
  queryFn: fetchData,
})
</script>

<template>
  <button @click="refetch()" :disabled="isFetching">
    {{ isFetching ? 'Refreshing...' : 'Refresh' }}
  </button>
</template>
```

### Polling
```typescript
const { data } = useQuery({
  queryKey: ['live-data'],
  queryFn: fetchLiveData,
  refetchInterval: 5000,  // Poll every 5 seconds
})
```

## Migration Notes (v4 → v5)

| v4 | v5 |
|----|-----|
| `cacheTime` | `gcTime` |
| `isLoading` | `isPending` |
| `useQuery({ onSuccess })` | Use `useEffect` or watch |
| `useQuery({ onError })` | Use error boundary or check `isError` |

## Reference Files

For detailed documentation:
- **API Reference**: See `references/api.md` for complete hook options
- **Patterns**: See `references/patterns.md` for advanced patterns

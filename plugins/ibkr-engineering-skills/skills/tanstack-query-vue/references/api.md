# TanStack Query Vue API Reference

## VueQueryPlugin Setup

### Basic Setup

```typescript
import { createApp } from 'vue'
import { VueQueryPlugin } from '@tanstack/vue-query'

const app = createApp(App)
app.use(VueQueryPlugin)
app.mount('#app')
```

### With Configuration

```typescript
import { VueQueryPlugin, type VueQueryPluginOptions } from '@tanstack/vue-query'

const vueQueryPluginOptions: VueQueryPluginOptions = {
  queryClientConfig: {
    defaultOptions: {
      queries: {
        staleTime: 5 * 60 * 1000,
        gcTime: 10 * 60 * 1000,
        retry: 3,
        retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000),
        refetchOnWindowFocus: false,
        refetchOnReconnect: true,
      },
      mutations: {
        retry: 1,
      },
    },
  },
}

app.use(VueQueryPlugin, vueQueryPluginOptions)
```

### With Custom QueryClient

```typescript
import { QueryClient, VueQueryPlugin } from '@tanstack/vue-query'

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: Infinity,
    },
  },
})

app.use(VueQueryPlugin, { queryClient })
```

## useQuery

### Full Options

```typescript
const result = useQuery({
  // Required
  queryKey: ['todos', filters],
  queryFn: ({ queryKey, signal }) => fetchTodos(queryKey[1], signal),
  
  // Optional
  enabled: true,                    // Enable/disable query
  staleTime: 0,                     // Time until data is stale (ms)
  gcTime: 5 * 60 * 1000,           // Garbage collection time (ms)
  retry: 3,                         // Number of retries
  retryDelay: 1000,                 // Delay between retries
  retryOnMount: true,               // Retry on mount if failed
  refetchOnMount: true,             // Refetch when component mounts
  refetchOnWindowFocus: true,       // Refetch on window focus
  refetchOnReconnect: true,         // Refetch on network reconnect
  refetchInterval: false,           // Polling interval (ms) or false
  refetchIntervalInBackground: false,
  networkMode: 'online',            // 'online' | 'always' | 'offlineFirst'
  
  // Data transformation
  select: (data) => data.filter(t => !t.completed),
  placeholderData: [],
  initialData: undefined,
  initialDataUpdatedAt: undefined,
  
  // Structural sharing
  structuralSharing: true,
  
  // Meta
  meta: { customKey: 'value' },
})
```

### Return Value

```typescript
const {
  // Data
  data,                 // TData | undefined
  dataUpdatedAt,        // number
  
  // Status
  status,               // 'pending' | 'error' | 'success'
  isPending,            // boolean - no data yet
  isError,              // boolean
  isSuccess,            // boolean
  
  // Fetch status
  fetchStatus,          // 'fetching' | 'paused' | 'idle'
  isFetching,           // boolean - fetching (initial or background)
  isPaused,             // boolean - query is paused (offline)
  isRefetching,         // boolean - background refetch
  
  // Error
  error,                // TError | null
  errorUpdatedAt,       // number
  failureCount,         // number
  failureReason,        // TError | null
  
  // Actions
  refetch,              // (options?) => Promise<QueryObserverResult>
  
  // Stale status
  isStale,              // boolean
} = useQuery(options)
```

## useMutation

### Full Options

```typescript
const mutation = useMutation({
  // Required
  mutationFn: (variables) => createTodo(variables),
  
  // Optional
  mutationKey: ['createTodo'],
  retry: 0,
  retryDelay: 1000,
  networkMode: 'online',
  gcTime: 5 * 60 * 1000,
  
  // Callbacks
  onMutate: async (variables) => {
    // Called before mutation
    // Return context for rollback
    return { previousData }
  },
  onSuccess: (data, variables, context) => {
    // Called on success
  },
  onError: (error, variables, context) => {
    // Called on error
  },
  onSettled: (data, error, variables, context) => {
    // Called on success or error
  },
  
  // Meta
  meta: { customKey: 'value' },
})
```

### Return Value

```typescript
const {
  // Actions
  mutate,               // (variables, options?) => void
  mutateAsync,          // (variables, options?) => Promise<TData>
  reset,                // () => void
  
  // Status
  status,               // 'idle' | 'pending' | 'error' | 'success'
  isPending,            // boolean
  isError,              // boolean
  isSuccess,            // boolean
  isIdle,               // boolean
  
  // Data
  data,                 // TData | undefined
  error,                // TError | null
  variables,            // TVariables | undefined
  
  // Counts
  failureCount,         // number
  failureReason,        // TError | null
  submittedAt,          // number
} = useMutation(options)
```

## useInfiniteQuery

### Full Options

```typescript
const result = useInfiniteQuery({
  queryKey: ['posts'],
  queryFn: ({ pageParam }) => fetchPosts(pageParam),
  initialPageParam: 0,
  
  // Page params
  getNextPageParam: (lastPage, allPages, lastPageParam) => {
    return lastPage.nextCursor ?? undefined
  },
  getPreviousPageParam: (firstPage, allPages, firstPageParam) => {
    return firstPage.prevCursor ?? undefined
  },
  
  // Max pages in cache
  maxPages: undefined,
  
  // All useQuery options also apply
  staleTime: 0,
  gcTime: 5 * 60 * 1000,
  // ...
})
```

### Return Value

```typescript
const {
  // All useQuery returns plus:
  data,                   // { pages: TData[], pageParams: unknown[] }
  fetchNextPage,          // (options?) => Promise
  fetchPreviousPage,      // (options?) => Promise
  hasNextPage,            // boolean
  hasPreviousPage,        // boolean
  isFetchingNextPage,     // boolean
  isFetchingPreviousPage, // boolean
} = useInfiniteQuery(options)
```

## useQueryClient

```typescript
import { useQueryClient } from '@tanstack/vue-query'

const queryClient = useQueryClient()

// Invalidate queries
queryClient.invalidateQueries({ queryKey: ['todos'] })
queryClient.invalidateQueries({ queryKey: ['todos'], exact: true })

// Refetch queries
queryClient.refetchQueries({ queryKey: ['todos'] })

// Cancel queries
await queryClient.cancelQueries({ queryKey: ['todos'] })

// Get cached data
const data = queryClient.getQueryData(['todos'])

// Set cached data
queryClient.setQueryData(['todos'], (old) => [...old, newTodo])

// Remove queries from cache
queryClient.removeQueries({ queryKey: ['todos'] })

// Reset queries to initial state
queryClient.resetQueries({ queryKey: ['todos'] })

// Prefetch queries
await queryClient.prefetchQuery({
  queryKey: ['todo', id],
  queryFn: () => fetchTodo(id),
})

// Get query state
const state = queryClient.getQueryState(['todos'])
```

## queryOptions Helper

Type-safe query options factory:

```typescript
import { queryOptions } from '@tanstack/vue-query'

const todosQueryOptions = (filters: Filters) => queryOptions({
  queryKey: ['todos', filters],
  queryFn: () => fetchTodos(filters),
  staleTime: 5 * 60 * 1000,
})

// Use in component
const { data } = useQuery(todosQueryOptions({ status: 'active' }))

// Use for prefetching
await queryClient.prefetchQuery(todosQueryOptions({ status: 'active' }))

// Use for invalidation
queryClient.invalidateQueries(todosQueryOptions({ status: 'active' }))
```

## TypeScript Types

```typescript
import type {
  QueryClient,
  QueryKey,
  QueryFunction,
  UseQueryOptions,
  UseQueryReturnType,
  UseMutationOptions,
  UseMutationReturnType,
  UseInfiniteQueryOptions,
  UseInfiniteQueryReturnType,
  QueryObserverResult,
  MutationObserverResult,
} from '@tanstack/vue-query'
```

## Nuxt 3 Setup

```typescript
// plugins/vue-query.ts
import type { VueQueryPluginOptions } from '@tanstack/vue-query'
import { 
  VueQueryPlugin, 
  QueryClient, 
  hydrate, 
  dehydrate 
} from '@tanstack/vue-query'
import { defineNuxtPlugin, useState } from '#imports'

export default defineNuxtPlugin((nuxt) => {
  const vueQueryState = useState<DehydratedState | null>('vue-query')
  
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { staleTime: 5000 },
    },
  })
  
  nuxt.vueApp.use(VueQueryPlugin, { queryClient })
  
  if (import.meta.server) {
    nuxt.hooks.hook('app:rendered', () => {
      vueQueryState.value = dehydrate(queryClient)
    })
  }
  
  if (import.meta.client) {
    nuxt.hooks.hook('app:created', () => {
      hydrate(queryClient, vueQueryState.value)
    })
  }
})
```

---
title: Stream Slow Data with `defer`
impact: HIGH
impactDescription: reduces time to first byte/paint
tags: server, loader, defer, streaming, suspense
---

## Stream Slow Data with `defer`

Don't let slow data requirements block the entire route transition. Use `defer` to stream non-critical data.

**Incorrect (blocks entire page until sidebar is ready):**

```tsx
export async function loader() {
  const header = await fetchHeader() // Fast (50ms)
  const sidebar = await fetchSidebarItems() // Slow (500ms)
  
  return { header, sidebar }
}

export default function Page({ loaderData }: Route.ComponentProps) {
  return (
    <div>
      <Header data={loaderData.header} />
      <Sidebar data={loaderData.sidebar} />
    </div>
  )
}
```

**Correct (page renders immediately, sidebar streams in):**

```tsx
import { defer } from 'react-router'

export async function loader() {
  const header = await fetchHeader() // Critical: await it
  const sidebarPromise = fetchSidebarItems() // Non-critical: promise
  
  return defer({
    header,
    sidebar: sidebarPromise,
  })
}

export default function Page({ loaderData }: Route.ComponentProps) {
  return (
    <div>
      <Header data={loaderData.header} />
      <Suspense fallback={<SidebarSkeleton />}>
        <Await resolve={loaderData.sidebar}>
          {(sidebar) => <Sidebar data={sidebar} />}
        </Await>
      </Suspense>
    </div>
  )
}
```

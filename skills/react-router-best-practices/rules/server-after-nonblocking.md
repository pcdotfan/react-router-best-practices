---
title: Non-Blocking Side Effects
impact: MEDIUM
impactDescription: faster response times
tags: server, async, logging, analytics, side-effects
---

## Non-Blocking Side Effects

Schedule work that should execute after a response is sent. This prevents logging, analytics, and other side effects from blocking the response.

**Incorrect (blocks response):**

```tsx
import { logUserAction } from './utils'

export async function action({ request }: Route.ActionArgs) {
  // Perform mutation
  await updateDatabase(request)
  
  // Logging blocks the response
  await logUserAction(request)
  
  return { status: 'success' }
}
```

**Correct (non-blocking):**

```tsx
import { logUserAction } from './utils'

export async function action({ request, context }: Route.ActionArgs) {
  // Perform mutation
  await updateDatabase(request)
  
  // Log in background
  // Note: specific implementation depends on your deployment target.
  // In Node.js, you can just not await the promise (handling errors).
  // In Cloudflare Workers or Vercel Edge, use context.waitUntil().
  
  const logPromise = logUserAction(request).catch(console.error)
  
  if (context?.waitUntil) {
    context.waitUntil(logPromise)
  }
  
  return { status: 'success' }
}
```

The response is sent immediately while logging happens in the background.

**Common use cases:**

- Analytics tracking
- Audit logging
- Sending notifications
- Cache invalidation
- Cleanup tasks

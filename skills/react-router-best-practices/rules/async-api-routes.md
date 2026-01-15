---
title: Prevent Waterfall Chains in Loaders/Actions
impact: CRITICAL
impactDescription: 2-10× improvement
tags: loaders, actions, waterfalls, parallelization
---

## Prevent Waterfall Chains in Loaders/Actions

In Loaders and Actions, start independent operations immediately, even if you don't await them yet.

**Incorrect (config waits for auth, data waits for both):**

```typescript
export async function loader({ request, params }) {
  const session = await auth()
  const config = await fetchConfig()
  const data = await fetchData(session.user.id)
  return { data, config }
}
```

**Correct (auth and config start immediately):**

```typescript
export async function loader({ request, params }) {
  const sessionPromise = auth()
  const configPromise = fetchConfig()
  const session = await sessionPromise
  const [config, data] = await Promise.all([
    configPromise,
    fetchData(session.user.id)
  ])
  return { data, config }
}
```

For operations with more complex dependency chains, use `better-all` to automatically maximize parallelism (see Dependency-Based Parallelization).

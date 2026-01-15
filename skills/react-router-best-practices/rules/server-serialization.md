---
title: Minimize Data Serialization
impact: HIGH
impactDescription: reduces data transfer size
tags: server, loader, serialization, props
---

## Minimize Data Serialization

Data passed from the server (Loaders/Actions) to the client is serialized into JSON and embedded in the HTML response or network payload. This serialized data directly impacts page weight and load time, so **size matters a lot**. Only pass fields that the client actually uses.

**Incorrect (serializes all 50 fields):**

```tsx
export async function loader() {
  const user = await fetchUser() // 50 fields
  return { user }
}

export default function Profile({ loaderData }: Route.ComponentProps) {
  return <div>{loaderData.user.name}</div> // uses 1 field
}
```

**Correct (serializes only 1 field):**

```tsx
export async function loader() {
  const user = await fetchUser()
  return { name: user.name }
}

export default function Profile({ loaderData }: Route.ComponentProps) {
  return <div>{loaderData.name}</div>
}
```

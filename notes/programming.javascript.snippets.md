---
id: mjfdkodzhces8aac8yp2gze
title: Snippets
desc: ""
updated: 1701479032044
created: 1701478525202
---

# `sleep`

```tsx
const sleep = (ms: number) => new Promise((r) => setTimeout(r, ms));
```

- Usage

```tsx
await sleep(10_000);
```

---

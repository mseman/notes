---
id: opkm5byl5ypjprkcz8e89a6
title: Components
desc: ""
updated: 1709775683282
created: 1701743936650
---

# `<Form />` component

See [[programming.remix.routing#form--component]]

---

# `<Links />` component

<https://remix.run/docs/en/main/components/links>

```tsx
import { Links } from "@remix-run/react";

export default function Root() {
  return (
    <html>
      <head>
        <Links />
      </head>
      <body></body>
    </html>
  );
}
```

- Renders all of the `<link>` tags created by your route module links export

- Usually placed within the `<head>` of the `<Root />` component

---

# `<Link />` component

See [[programming.remix.routing#link--component]]

---

# `<LiveReload />` component

<https://remix.run/docs/en/main/components/live-reload>

```tsx
import { Links, LiveReload, Meta, Scripts } from "@remix-run/react";

// ...

export default function Root() {
  return (
    <html lang="en">
      <head>
        <Meta />
        <Links />
      </head>
      <body>
        {/* ... */}
        <Scripts />
        <LiveReload />
      </body>
    </html>
  );
}
```

- TODO: Description

---

# `<Outline />` component

See [[programming.remix.routing#outline--component]]

---

# `<Root />` component

See [[programming.remix.routing#root--component]]

---

# `<Scripts />` component

<https://remix.run/docs/en/main/components/scripts>

```tsx
import { Links, Meta, Scripts } from "@remix-run/react";

// ...

export default function Root() {
  return (
    <html lang="en">
      <head>
        <Meta />
        <Links />
      </head>
      <body>
        {/* ... */}
        <Scripts />
      </body>
    </html>
  );
}
```

- TODO: Description

---

# `<ScrollRestoration />` component

See [[programming.remix.routing#scrollrestoration--component]]

---

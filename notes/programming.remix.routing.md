---
id: iy51hc8k5ixw0dmpvdvrod7
title: Routing
desc: ""
updated: 1709775573525
created: 1700195725980
---

- Remix's router supports what's called "nested routing." Often, the UI of an
  application is nested in a similar fashion to the URL. For example, let's take
  a URL like this: `https://example.com/sales/invoices/:invoiceId`. The "root"
  of the application (the part of the app that's on all pages) is considered the
  "parent route" of all the routes on the page. The `/sales` portion is a child
  of the root, the `/invoices` is a child of the `/sales` route, and the
  `/:invoiceId` is a child of the `/invoices` route. The UI could resemble
  something like this: A left navigation section (root), then a top navigation
  (sales), then a list (invoices), and finally a details portion of the page
  (invoice ID).

- `npx remix routes` - Terminal command to print out all the routes in an
  application

---

# Route components

---

## `<Outline />` component

<https://remix.run/docs/en/main/components/outlet>

```tsx
import { Outlet } from "@remix-run/react";

export function SomeParent() {
  return (
    <div>
      <h1>Parent Content</h1>

      <Outlet />
    </div>
  );
}
```

- Renders the matching child route of a parent route

---

## `<Root />` component

<https://remix.run/docs/en/main/file-conventions/routes#root-route>

```tsx
import { Links, Outlet } from "@remix-run/react";

export default function Root() {
  return (
    <html lang="en">
      <head>
        <Links />
      </head>
      <body>
        <Outlet />
      </body>
    </html>
  );
}
```

- Serves as the root layout of the entire app

  - All other routes will render inside the `<Outlet />` component

- Always located in `app/root.tsx`

- Works just like all other route modules, so you can export a loader, action, etc

---

# Managing routes, the `routes/` directory, and `remix-flat-routes`

---

## `remix-flat-routes`

<https://github.com/kiliman/remix-flat-routes>

- Implements an improved version of remix's v2 routing convention

---

### `index.tsx`

```tsx
<Routes>
  <Route file="root.tsx">
    <Route path="users/" index file="routes/users+/index.tsx" />
  </Route>
</Routes>
```

- aka the `index` route

- Because the filename is `index.tsx`, the path to render the component exported
  by `routes/index.tsx` will be `/`

- The component exported by `index.tsx` is nested 1 level deep

  - The `/` route is the root route itself, so the component exported by
    `index.tsx` will be rendered inside the `<Root />` component

---

#### Nested `index.tsx` routes

```tsx
<Routes>
  <Route file="root.tsx">
    <Route index file="routes/index.tsx" />
    <Route path="users/" index file="routes/users+/index.tsx" />
  </Route>
</Routes>
```

```tsx
<Routes>
  <Route file="root.tsx">
    <Route index file="routes/index.tsx" />
    <Route path="users" file="routes/users.tsx">
      <Route index file="routes/users+/index.tsx" />
    </Route>
  </Route>
</Routes>
```

---

### Route examples - `routes/users.kody.tsx`

```tsx
<Routes>
  <Route file="root.tsx">
    <Route path="users" file="routes/users.tsx">
      <Route path="users/kody" file="routes/users.kody.tsx" />
    </Route>
  </Route>
</Routes>
```

- Equivalent to `routes/users+/kody.tsx`

- The `.` in `users.kody` tells remix-flat-routes to separate `users` and `kody`
  by a `/`. So the path to render the component exported by `users.kody.tsx`
  will be `/users/kody`

- The component exported by `users.kody.tsx` is nested 2 levels deep

  - The parent of the `kody` route is the `users` route, and thus the component
    exported by `users.kody.tsx` will be rendered inside the component exported
    by `users.tsx`

  - The parent of the `users` route is the `root` route, and thus the component
    exported by `users.tsx` (and thereby the component exported by
    `users.kody.tsx`) will be rendered inside the `<Root />` component

---

### Route examples - `routes/users+/kody.tsx`

```tsx
<Routes>
  <Route file="root.tsx">
    <Route path="users" file="routes/users.tsx">
      <Route path="kody" file="routes/users+/kody.tsx" />
    </Route>
  </Route>
</Routes>
```

- Equivalent to `routes/users.kody.tsx`

- The `+` at the end of the `users+` directory name tells remix-flat-routes to
  separate `users` and `kody` by a `/`. So the path to render the component
  exported by `users+/kody.tsx` will be `/users/kody`

- The component exported by `users+/kody.tsx` is nested 2 levels deep

  - The parent of the `kody` route is the `users` route, and thus the component
    exported by `users+/kody.tsx` will be rendered inside the component exported
    by `users.tsx`

    - This would not be the case if `kody.tsx` was renamed to `_kody.tsx`

  - The parent of the `users` route is the `root` route, and thus the component
    exported by `users.tsx` (and thereby the component exported by
    `users+/kody.tsx`) will be rendered inside the `<Root />` component

---

### Route examples - `routes/users+/kody_+/notes.tsx`

```tsx
<Routes>
  <Route file="root.tsx">
    <Route path="users" file="routes/users.tsx">
      <Route path="kody" file="routes/users+/kody.tsx" />
      <Route path="kody/notes" file="routes/users+/kody_+/notes.tsx" />
    </Route>
  </Route>
</Routes>
```

- The path to render the component exported by `users+/kody_+/notes.tsx` will be
  `/users/kody/notes`

- The `_` in the `kody_+` directory name tells remix-flat-routes that the
  components inside of the `users+/kody_+` directory do NOT have the
  `users+/kody.tsx` component as a parent, and instead have the component
  rendered by `users.tsx` as a parent

- The component exported by `users+/kody_+/notes.tsx` is nested 2 levels deep

  - Because the `kody_+` directory contains an `_`, the parent of the `notes` route
    is the `users` route, and thus the component exported by
    `users+/kody_+/notes.tsx` will be rendered inside the component exported by
    `users.tsx`

  - The parent of the `users` route is the `root` route, and thus the component
    exported by `users.tsx` (and thereby the component exported by
    `users+/kody.tsx`) will be rendered inside the `<Root />` component

---

### Route examples - `routes/users+/$username.tsx`

```tsx
<Routes>
  <Route file="root.tsx">
    <Route path="users/:username" file="routes/users+/$username.tsx" />
  </Route>
</Routes>
```

- The path to render the component exported by `users+/$username.tsx` will be
  `/users/:username`

- The `$` in the `$username.tsx` filename tells remix-flat-routes that the route
  requires a route param, called `username`

- Any route params can be accessed within the component with the `useParams`
  hook

  ```tsx
  import { useParams } from "@remix-run/react";

  export default function UserRoute() {
    const params = useParams();

    return (
      <div>
        <h1>{params.username}</h1>
      </div>
    );
  }
  ```

- The component exported by `users+/$username.tsx` is nested 1 level deep

  - Because there is no `routes/users.tsx` file, the parent of the `:username`
    route is the `root` route, and thus the component exported by
    `users+/$username.tsx` will be rendered inside the `<Root />` component

---

## `loader` function

See [[programming.remix.data#loader-functions]]

---

## Route matching and the `action` function

> In Remix, when a POST is made to a URL, multiple routes in your route
> hierarchy will match the URL. Unlike a GET to loaders, where all of them are
> called to build the UI, _only one action is called_.
>
> The route called will be the deepest matching route, unless the deepest
> matching route is an "index route". In this case, it will post to the parent
> route of the index (because they share the same URL, the parent wins).
>
> If you want to post to an index route use `?index` in the action:
> `<Form action="/accounts?index" method="post" />`

- <https://remix.run/docs/en/main/route/action>

---

## Resource routes

<https://remix.run/docs/en/main/guides/resource-routes>

```tsx
import { json, type DataFunctionArgs } from "@remix-run/node";

export async function loader({ request, params }: DataFunctionArgs) {
  return json({ hello: "world" });
}
```

- Like a regular route, but exports a `loader` function and doesn't have a default
  export. When the associated route is requested, the `loader` function will be
  run on the server and its output returned to the caller

- Route nesting does not affect Resource Routes. Only the `loader` of the route
  that matches the request is called. The `loader` function in the
  `app/root.tsx` module, for example, will not be run

- WARNING: Doing a client-side navigation to a resource route breaks the app,
  because that route doesn't exist in a UI context. So you need to make sure if
  you link to a resource route, you either use a regular `<a>` or add the
  `reloadDocument` prop to the `<Link />`

---

# Navigation

---

## `<Form />` component

<https://remix.run/docs/en/main/components/form>

```tsx
import { Form } from "@remix-run/react";

function NewEvent() {
  return (
    <Form action="/events" method="post">
      <input name="title" type="text" />
      <input name="description" type="text" />
    </Form>
  );
}
```

- A wrapper around the `<form>` tag that enables client-side routing

- Forms without an action prop (`<Form method="post">`) will automatically POST
  to the same route within which they are rendered

- NOTE: For forms that shouldn't manipulate the browser history stack, use
  `<fetcher.Form>` <https://remix.run/docs/en/main/hooks/use-fetcher#fetcherform>

---

### `preventScrollReset` prop

<https://remix.run/docs/en/main/components/form#preventscrollreset>
<https://reactrouter.com/en/main/components/link#preventscrollreset>

```tsx
<Link preventScrollReset />;
<Form preventScrollReset />;
```

- Prevents the user's scroll position from being reset after navigating

- Usable on both the `<Link />` component and the `<Form />` component

---

## `<Link />` component

<https://remix.run/docs/en/main/components/link>

```tsx
import { Link } from "@remix-run/react";

function NavBar() {
  return (
    <div>
      <Link to="/dashboard">Dashboard</Link>
    </div>
  );
}
```

- A wrapper around the `<a>` tag that enables client-side routing

- See also [[programming.remix.routing#preventscrollreset-prop]]

---

### `prefetch` prop

<https://remix.run/docs/en/main/components/link#prefetch>

```tsx
<>
  <Link /> {/* defaults to "none" */}
  <Link prefetch="none" />
  <Link prefetch="intent" />
  <Link prefetch="render" />
  <Link prefetch="viewport" />
</>
```

- Instructs Remix to prefetch data and modules based on how the user interacts
  with the associated `<a>` element

- `none` - default value, does not perform any prefetching

- `intent` - prefetches when the user hovers or focuses the `<a>` element

- `render` - prefetches when the `<a>` element is rendered

- `viewport` - prefetches when the `<a>` element is in the viewport

- NOTE: Prefetching will insert `<link rel="prefetch" ... />` elements after the
  `<a>` element. Because of this, if you are using the `nav :last-child` css
  selector you will need to use the `nav :last-of-type` css selector so the
  styles don't conditionally fall off your last `<a>` element (and any other
  similar selectors)

---

## `<ScrollRestoration />` component

<https://remix.run/docs/en/main/components/scroll-restoration>

```tsx
import { Links, Meta, Scripts, ScrollRestoration } from "@remix-run/react";

export default function Root() {
  return (
    <html lang="en">
      <head>
        <Meta />
        <Links />
      </head>
      <body>
        {/* ... */}
        <ScrollRestoration />
        <Scripts />
      </body>
    </html>
  );
}
```

- TODO: Description

- See also [[programming.remix.routing#preventscrollreset-prop]]

---

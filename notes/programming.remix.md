---
id: s8ddacyvhofpccy4lv49keb
title: Remix
desc: ""
updated: 1701831263376
created: 1699932032176
---

# Bundling and entry files

- Remix has 2 entry files that are compiled into 2 corresponding JavaScript
  "bundle" files

---

## `entry.client.tsx`

<https://remix.run/docs/en/main/file-conventions/entry.client>

```tsx
import { RemixBrowser } from "@remix-run/react";
import { startTransition } from "react";
import { hydrateRoot } from "react-dom/client";

startTransition(() => {
  hydrateRoot(document, <RemixBrowser />);
});
```

- The entry file for client-side code

  - The bundled output is usually placed in the `public/build/` directory,
    usually named `entry.client-<SOME_HASH>.js` where `<SOME_HASH>` is a random,
    remix-generated fingerprint

- Responsible for hydrating the application

- Optional - By default, Remix will handle bundling your client-side JavaScript
  for you. This file only needs to exist if you want to customize this behavior

- Wrapping the `hydrateRoot` function inside the `startTransition` function
  ensures that hydration doesn't block the main thread, and there is no
  interruption in the event that the user is already interacting with the page
  during hydration

- The `hydrateRoot` function is being called on the `document`, instead of the
  body or an element inside the body, to allow Remix to control all of the
  contents between the `<html>` and `</html>` tags

---

## `entry.server.tsx`

<https://remix.run/docs/en/main/file-conventions/entry.server>

```tsx
import { type HandleDocumentRequestFunction } from "@remix-run/node";
import { RemixServer } from "@remix-run/react";
import { renderToString } from "react-dom/server";

type DocRequestArgs = Parameters<HandleDocumentRequestFunction>;

export default async function handleRequest(...args: DocRequestArgs) {
  const [request, responseStatusCode, responseHeaders, remixContext] = args;
  const markup = renderToString(
    <RemixServer context={remixContext} url={request.url} />
  );

  responseHeaders.set("Content-Type", "text/html");

  return new Response("<!DOCTYPE html>" + markup, {
    status: responseStatusCode,
    headers: responseHeaders,
  });
}
```

- The entry file for server-side code

  - The bundled output is usually placed in the `build/` directory, usually
    named `index.js`

- Responsible for generating the markup for a given http request, and returning
  an http response containing that markup

- Optional - By default, Remix will handle generating the http response for you.
  This file only needs to exist if you want to customize this behavior

---

# Module evaluation order

<https://www.epicweb.dev/tips/javascript-module-evaluation-order-on-the-web>

---

# Hydration

- Client-side hydration usually occurs in the `entry.client.tsx` file - see
  [[programming.remix#entryclienttsx]]

---

# `links` function

<https://remix.run/docs/en/main/route/links>

```tsx
import { type LinksFunction } from "@remix-run/node";

export const links: LinksFunction = () => {
  return [
    {
      rel: "stylesheet",
      href: "/my-stylesheet.css",
    },
  ];
};
```

- Export from a route module to render all the `<link>` tags returned by the
  function on the route and any child routes

- Returned `<link>` tags will be rendered by the `<Links />` component (usually
  located in the `<head>` of the `<Root />` component)

- See also [[programming.remix.components#links--component]]

---

# Static assets

---

## Asset imports

<https://remix.run/docs/en/main/file-conventions/asset-imports>

```tsx
import banner from "./images/banner.jpg";

export default function Page() {
  return (
    <div>
      <h1>Some Page</h1>
      <img src={banner} />
    </div>
  );
}
```

- Provides a url when imported and will "compile" the asset - place it in the
  project's build directory and add a fingerprint to the filename

  - Build directory can be changed via the `assetsBuildDirectory` property in
    the `remix.config.js` file

    - <https://remix.run/docs/en/main/file-conventions/remix-config#assetsbuilddirectory>

---

## `public/` directory

- All files in the `public/` directory are served at the root of the site

---

---
id: guu4gjv2ag9lkkbmv5sg7zj
title: CSS
desc: ""
updated: 1701821248324
created: 1700195857761
---

# Best Practices

- The general best practice for bundled vs separate css files is to have some
  separate css files for things that don't frequently change (e.g. fonts, library
  stylesheets), and bundle per-page stylesheets together

  ```tsx
  import { cssBundleHref } from "@remix-run/css-bundle";
  import { type LinksFunction } from "@remix-run/node";
  import faviconAssetUrl from "./assets/favicon.svg";
  import fontStylesheetUrl from "./styles/font.css";
  import tailwindStylesheetUrl from "./styles/tailwind.css";

  export const links: LinksFunction = () => {
    return [
      { rel: "icon", type: "image/svg+xml", href: faviconAssetUrl },
      { rel: "stylesheet", href: fontStylesheetUrl },
      { rel: "stylesheet", href: tailwindStylesheetUrl },
      cssBundleHref ? { rel: "stylesheet", href: cssBundleHref } : null,
    ].filter(Boolean);
  };
  ```

---

# Bundling stylesheets

<https://remix.run/docs/en/main/styling/bundling#css-bundling>

- Any css file imported without an "import clause" will be bundled

  ```tsx
  import stylesheetUrl from "./styles1.css"; // <-- will NOT be bundled
  import "./styles2.css"; // <-- this will be bundled
  ```

- Must load the bundle file in a `<link>` tag, usually in the `links` function
  in the `<Root />` component

  ```tsx
  import { cssBundleHref } from "@remix-run/css-bundle";
  import { type LinksFunction } from "@remix-run/node";

  export const links: LinksFunction = () => {
    return [
      cssBundleHref ? { rel: "stylesheet", href: cssBundleHref } : null,
    ].filter(Boolean);
  };
  ```

  - NOTE: `cssBundleHref` will be `undefined` if there are no side-effect css
    imports on a page, so the `undefined` case must be handled via `filter`ing

---

# Global stylesheets

<https://remix.run/docs/en/main/styling/css#regular-css>

```tsx
import type { LinksFunction } from "@remix-run/node";

import stylesheetUrl from "~/styles/global.css";

export const links: LinksFunction = () => [
  { rel: "stylesheet", href: stylesheetUrl },
];
```

- Can be added to the `<Root />` component's `links` function to load the
  stylesheet on every page

- Can be added to any route component's `links` function to only load the
  stylesheet on that specific page (and all of its child pages)

---

# PostCSS

<https://postcss.org/>

<https://remix.run/docs/en/main/styling/postcss>

- Support is built-in to Remix. If there is a `postcss.config.js` file in the
  project's root directory, Remix will automatically use PostCSS to compile the
  css files you import

- Configured via the `postcss.config.js` file in the project's root directory
  ```tsx
  export default {
    plugins: {
      "tailwindcss/nesting": {},
      tailwindcss: {},
      autoprefixer: {},
    },
  };
  ```

---

# Tailwind

<https://tailwindcss.com/>

<https://remix.run/docs/en/main/styling/tailwind>

- Must be included as a plugin for PostCSS via the `postcss.config.js` file

  ```tsx
  export default {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    },
  };
  ```

- Configured via the `tailwind.config.ts` file in the project's root directory

  ```tsx
  import { type Config } from "tailwindcss";
  import defaultTheme from "tailwindcss/defaultTheme.js";

  export default {
    content: ["./app/**/*.{ts,tsx,jsx,js}"],
    theme: {
      extend: {
        fontFamily: {
          sans: [
            "Nunito Sans",
            "Nunito Sans Fallback",
            ...defaultTheme.fontFamily.sans,
          ],
        },
      },
    },
  } satisfies Config;
  ```

- Place the Tailwind directives in a stylesheet

  ```css
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
  ```

- Read more about configuring Tailwind with PostCSS: <https://tailwindcss.com/docs/using-with-preprocessors#using-post-css-as-your-preprocessor>

---

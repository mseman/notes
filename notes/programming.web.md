---
id: 5ng1cdtwko958vjwo73m5al
title: Web
desc: ""
updated: 1701566476291
created: 1699403411112
---

# Favicon

- For the best user experience, support all formats of favicon. Browsers that
  support svg favicons will override the first link element declaration and
  honor the second instead. Browsers that do not support svg favicons but do
  support web app manifests will use the higher-resolution images. All other
  browsers fall back to using the favicon.ico

```html
<link rel="icon" href="/favicon.ico" sizes="any" />
<link rel="icon" href="/favicon.svg" type="image/svg+xml" />
<link rel="manifest" href="/manifest.webmanifest" />
```

---

## `.ico` format

Loaded onto a page using the `<link>` tag

```html
<link rel="icon" type="image/x-icon" href="/images/favicon.ico" />
```

---

## `.png` format

Add an entry to your site's `manifest.json`

```json
{
  "icons": [
    { "src": "/icon-192.png", "type": "image/png", "sizes": "192x192" },
    { "src": "/icon-512.png", "type": "image/png", "sizes": "512x512" }
  ]
}
```

---

## `.svg` format

Loaded onto a page using the `<link>` tag

```html
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
```

- <https://css-tricks.com/svg-favicons-and-all-the-fun-things-we-can-do-with-them/>

- NOTE 2023-11-14: Not supported in safari <https://caniuse.com/link-icon-svg>

---

# URLs

<https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL>

---

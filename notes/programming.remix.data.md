---
id: 520wfn9jtcjnc5w6tnw9x66
title: Data
desc: ""
updated: 1709774836591
created: 1700689884570
---

# Data loading

---

## `loader` function

<https://remix.run/docs/en/main/route/loader>

```tsx
import { type DataFunctionArgs } from "@remix-run/node";
import { useLoaderData } from "@remix-run/react";

export async function loader({ request, params }: DataFunctionArgs) {
  const dataString = JSON.stringify({ hello: "world" });
  return new Response(dataString, {
    headers: { "content-type": "application/json" },
  });
}

export default function MyRoute() {
  const data = useLoaderData<typeof loader>();
  return (
    <div>
      <h1>{data.hello}</h1>
    </div>
  );
}
```

- Export from a route module to provide data to the route component

- The route component can use the `useLoaderData` hook to access the data
  returned by the `loader` function

- Run only on the server and therefore has access to your database, APIs,
  private environment variables, etc

- Because most loaders will return a JSON response, Remix provides a helper
  function, the `json` function

  - See [[programming.remix.data#json-helper]]

---

# Data mutation

---

## `action` function

<https://remix.run/docs/en/main/route/action>

```tsx
import { Form } from "@remix-run/react";
import { type DataFunctionArgs, redirect } from "@remix-run/node";

export async function action({ request, params }: DataFunctionArgs) {
  const formData = await request.formData();
  const sandwichType = formData.get("sandwichType");

  // do something with the sandwichType

  return redirect("/sandwiches");
}

export default function SandwichChooser() {
  return (
    <Form method="POST">
      <label>
        Type: <input name="sandwichType" />
      </label>
      <button type="submit">Create Sandwich</button>
    </Form>
  );
}
```

- Export from a route module to do stuff when a non-GET request is made to that
  route

- As a best practice, a successful `action` function should return a redirect if
  the `action` function is being called from a `<Form />`

  - See [[programming.web.html#forms-and-the-back-button]]

  - Remix provides a helper function for this, the `redirect` function. See
    [[programming.remix.data#redirect-helper]]

- Run only on the server and therefore has access to your database, APIs,
  private environment variables, etc

- See also [[programming.remix.routing#route-matching-and-the-action-function]]

---

## `<Form />` component

See [[programming.remix.components#form--component]]

---

# `json` helper

<https://remix.run/docs/en/main/utils/json>

```tsx
import { json, type DataFunctionArgs } from "@remix-run/node";

export async function loader({ request, params }: DataFunctionArgs) {
  return json({ hello: "world" });
}
```

---

# `redirect` helper

<https://remix.run/docs/en/main/utils/redirect>

```tsx
import { type DataFunctionArgs, redirect } from "@remix-run/node";

export async function action({ request, params }: DataFunctionArgs) {
  const formData = await request.formData();
  const sandwichType = formData.get("sandwichType");

  // do something with the sandwichType

  return redirect("/sandwiches");
}
```

- The behavior of redirects with relative URLs is weird (because of the web as a
  platform, not specific to Remix), so it is best to always use absolute paths
  when redirecting

---

# `throw`

```tsx
export async function loader({ params }) {
  const sandwich = await db.sandwich.findFirst({
    where: { id: params.id },
  });

  if (!sandwich) {
    throw new Response("Sandwich not found", { status: 404 });
  }

  return json({ sandwich });
}
```

- In a `loader` or `action` function, you can `throw new Response()` to short
  circuit the code and have Remix catch the response and send it to the client

---

# Resource Routes

See [[programming.remix.routing#resource-routes]]

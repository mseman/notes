---
id: hfqfiy8dpabn4d92gsveeht
title: UI
desc: ""
updated: 1701482023509
created: 1701481576442
---

# Forms

---

## Button loading states

- When a form is in the process of submitting, Remix provides the
  `useNavigation` hook, which exposes the submitted form's `intent` via the
  `formData` method

- See also <https://www.epicweb.dev/tips/automatic-browser-request-cancellation>

```tsx
import { redirect, type DataFunctionArgs } from "@remix-run/node";
import { Form, useNavigation } from "@remix-run/react";
import { Button } from "#app/components/ui/button.tsx";
import { db } from "#app/utils/db.server.ts";

export async function action({ params, request }: DataFunctionArgs) {
  const formData = await request.formData();
  const intent = formData.get("intent");

  switch (intent) {
    case "delete": {
      db.note.delete({ where: { id: { equals: params.noteId } } });
      return redirect(`/users/${params.username}/notes`);
    }
    case "favorite": {
      db.note.favorite({ where: { id: { equals: params.noteId } } });
      return redirect(`/users/${params.username}/notes/${params.noteId}`);
    }
    default: {
      throw new Response(`Invalid intent: ${intent}`, { status: 400 });
    }
  }
}

export default function NoteRoute() {
  const navigation = useNavigation();
  const intent = navigation.formData?.get("intent");

  return (
    <div>
      <Form method="POST">
        <Button
          disabled={Boolean(intent)}
          loading={intent === "delete"}
          name="intent"
          type="submit"
          value="delete"
        >
          Delete
        </Button>

        <Button
          disabled={Boolean(intent)}
          loading={intent === "favorite"}
          name="intent"
          type="submit"
          value="favorite"
        >
          Favorite
        </Button>
      </Form>
    </div>
  );
}
```

---

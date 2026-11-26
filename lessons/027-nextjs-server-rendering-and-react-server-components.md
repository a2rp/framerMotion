# Next.js, server rendering, and React Server Components

Next.js App Router pages and layouts are Server Components by default. This lets the server fetch data and render non-interactive parts of a route without sending their component JavaScript to the browser.

Motion can be used in both server and client parts of this architecture. Use the `motion/react-client` entry point for Motion components imported from a Server Component. Use a narrow Client Component boundary when animation depends on React state, event handlers, effects, browser APIs, or client-only hooks.

## Learning outcomes

By the end of this lesson, you should be able to:

- Distinguish Server Components from Client Components in the Next.js App Router.
- Choose the correct Motion import for each component boundary.
- Keep server data fetching and static content on the server.
- Put stateful animation and browser-dependent behavior behind a small `"use client"` boundary.
- Pass serializable values from a Server Component to a Client Component.
- Avoid common server rendering and hydration mistakes.

## Server Components and Client Components

A Server Component runs as part of server rendering. It can fetch data near its source and keep server-only values out of the client module graph. It cannot use browser APIs, React state, event handlers, or client-only hooks.

A Client Component is part of the client module graph. It can use state, event handlers, effects, browser APIs, and client-side hooks. In the App Router, a file becomes a client entry point when `"use client"` appears at the very beginning, before imports.

A Client Component may still be pre-rendered to HTML on the server for the initial page load. The browser later hydrates it to attach event handling and make it interactive. "Client Component" describes the module boundary and capabilities; it does not mean that no HTML is produced on the server.

## Use Motion from a Server Component

For a Motion component imported into a Server Component, use the React Server Components entry point:

~~~tsx
import * as motion from "motion/react-client";

export default function Intro() {
  return (
    <motion.section
      initial={false}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.25 }}
    >
      <h1>Motion can render with server content</h1>
      <p>The visible content is available in the rendered page.</p>
    </motion.section>
  );
}
~~~

Motion supports server rendering, and its initial state is reflected in generated output. In the example, `initial={false}` makes the target state the rendered state, avoiding an initially invisible heading if animation JavaScript is delayed or unavailable.

The `motion/react-client` entry point is not a replacement for a Client Component when your own component needs React state, event handlers, effects, or browser APIs. It provides Motion components that can be used in an RSC-aware module graph.

## Keep stateful motion inside a client boundary

Use `motion/react` in a Client Component when the component manages interactive state:

~~~tsx
"use client";

import { useState } from "react";
import { motion } from "motion/react";

export function ExpandableTip({ title, body }: {
  title: string;
  body: string;
}) {
  const [open, setOpen] = useState(false);

  return (
    <section>
      <button
        type="button"
        aria-expanded={open}
        onClick={() => setOpen((current) => !current)}
      >
        {open ? "Hide" : "Show"} {title}
      </button>

      {open && (
        <motion.p
          initial={{ opacity: 0, y: 8 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.2 }}
        >
          {body}
        </motion.p>
      )}
    </section>
  );
}
~~~

The directive is at the top of the file because it declares a client boundary. The component owns its button handler and state. The animation props remain local to this interactive client component.

Props crossing from a Server Component into a Client Component must be serializable. Strings, numbers, booleans, arrays, and plain serializable records are common choices. Do not pass a function created in a Server Component as an ordinary callback prop.

## Compose server and client UI

A route can remain a Server Component and render a small animated Client Component:

~~~tsx
// app/page.tsx - Server Component
import { ExpandableTip } from "./expandable-tip";

const tip = {
  pageTitle: "Motion on the server",
  summary: "Static page content can stay in a Server Component.",
  title: "Client-side animation",
  body: "This small interactive tip is rendered by a Client Component.",
};

export default function Page() {
  return (
    <main>
      <h1>{tip.pageTitle}</h1>
      <p>{tip.summary}</p>
      <ExpandableTip title={tip.title} body={tip.body} />
    </main>
  );
}
~~~

~~~tsx
// app/expandable-tip.tsx - Client Component
"use client";

import { useState } from "react";
import { motion } from "motion/react";

export function ExpandableTip({ title, body }: {
  title: string;
  body: string;
}) {
  const [open, setOpen] = useState(false);

  return (
    <section>
      <button
        type="button"
        aria-expanded={open}
        onClick={() => setOpen((current) => !current)}
      >
        {open ? "Hide" : "Show"} {title}
      </button>
      {open && <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }}>{body}</motion.p>}
    </section>
  );
}
~~~

The Server Component performs the data work and provides already-rendered content. The Client Component adds only the state and animation interaction that needs browser-side behavior.

If a Server Component needs to appear inside a Client Component, render it in the Server Component parent and pass it through a slot such as `children`. Do not import a server-only module from the client module graph.

## Initial state, hydration, and animation

Server rendering gives the browser useful HTML before client JavaScript finishes loading. Make the initial visual state deterministic so the server output and the first client render agree.

Use these practices:

- Give animation props stable values during server rendering and the first client render.
- Avoid reading `window`, viewport dimensions, local storage, or other browser-only state during render.
- Avoid generating random IDs or time-dependent initial values independently on the server and client.
- Use `initial={false}` when the intended server-rendered appearance should match the animation target.
- If a client-only preference changes animation, read it in an appropriate client-side hook and preserve a usable initial state.
- Keep meaningful content visible without depending on an animation finishing.

Motion components support server rendering, but a browser interaction still needs client-side code. Server rendering does not make click handlers or state updates run on the server.

## Keep the client boundary small

Every module imported into a Client Component entry point becomes part of its client module graph. Mark only the interactive part as a client entry point when possible.

For a page with mostly static content:

1. Keep the route and data access in a Server Component.
2. Identify the exact control that needs state or browser behavior.
3. Place `"use client"` at the top of that interactive component.
4. Pass it the small set of serializable data it needs.
5. Keep unrelated page content outside the client boundary.

This composition can reduce client JavaScript while preserving rich interactions where they are needed.

## Common mistakes

- Importing `motion` from `motion/react` in a Server Component that is intended to use the RSC entry point.
- Expecting `motion/react-client` to make your own event handlers or state work in a Server Component.
- Placing `"use client"` after imports or below other executable code.
- Marking a whole route client-side when only one control needs interaction.
- Passing a function or other unsupported value from a Server Component as a normal prop.
- Reading browser-only values during render and producing different initial output on the server and client.
- Assuming a Client Component is never server-rendered to HTML on the first load.
- Making essential content invisible until a client animation runs.

## Practice

Build a server-rendered page with a heading and a short data summary. Add one expandable Motion client component. Confirm that the page content is visible in the initial HTML, the button works after hydration, and the initial markup does not produce a hydration mismatch.

Then move the animation component boundary inward or outward and compare the client JavaScript included by the production build. Keep the smallest boundary that preserves the required behavior.

Continue with [React Router transitions and navigation](028-react-router-transitions-and-navigation.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion</a></td><td><a href="../qnas/027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components Q&amp;A</a></td><td><a href="../examples/027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components example</a></td><td><a href="028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation</a></td></tr></tbody>
</table>

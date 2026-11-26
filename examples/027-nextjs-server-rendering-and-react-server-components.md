# Next.js, server rendering, and React Server Components example

This App Router example keeps the page content in a Server Component and puts only the interactive reveal control in a Client Component. The server page imports Motion from `motion/react-client`. The reveal control uses state and event handlers, so it starts with `"use client"` and imports from `motion/react`.

## What this example demonstrates

- A server page can render Motion components through the RSC entry point.
- Server-rendered content begins in a visible, deterministic state.
- A stateful animated control has a narrow Client Component boundary.
- The Server Component passes only serializable string props.
- The interactive panel has a native button, a stable controls target, and reduced-motion handling.

## Setup

Use a Next.js App Router project with Motion installed:

~~~bash
npm install motion
~~~

Create the files below under the `app` directory. If your project uses a `src/app` directory, put them there instead.

## `app/page.tsx`

~~~tsx
import * as motion from "motion/react-client";
import ExpandableTip from "./ExpandableTip";

const tip = {
  title: "Keep the boundary small",
  body: "The page content can stay server-rendered while this button owns its interactive state.",
};

export default function Page() {
  return (
    <main className="page-shell">
      <motion.header
        className="page-heading"
        initial={false}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.2 }}
      >
        <p className="eyebrow">Motion with the App Router</p>
        <h1>Server content, focused interaction</h1>
        <p>
          This heading and explanation are rendered by the Server Component.
          They remain visible in the initial HTML.
        </p>
      </motion.header>

      <ExpandableTip title={tip.title} body={tip.body} />
    </main>
  );
}
~~~

The page has no `"use client"` directive. It imports Motion from `motion/react-client` for the server module graph. It can also fetch server data in an actual application, then pass serializable fields to the interactive component.

## `app/ExpandableTip.tsx`

~~~tsx
"use client";

import { useState } from "react";
import {
  AnimatePresence,
  motion,
  useReducedMotion,
} from "motion/react";

type ExpandableTipProps = {
  title: string;
  body: string;
};

export default function ExpandableTip({
  title,
  body,
}: ExpandableTipProps) {
  const [open, setOpen] = useState(false);
  const shouldReduceMotion = useReducedMotion();
  const distance = shouldReduceMotion ? 0 : 10;
  const duration = shouldReduceMotion ? 0 : 0.18;

  return (
    <section className="tip">
      <button
        className="tip-toggle"
        type="button"
        aria-expanded={open}
        aria-controls="tip-panel-container"
        onClick={() => setOpen((current) => !current)}
      >
        {open ? "Hide tip" : "Show tip"}: {title}
      </button>

      <div id="tip-panel-container">
        <AnimatePresence initial={false}>
          {open && (
            <motion.div
              key="tip-panel"
              className="tip-panel"
              initial={{ opacity: 0, y: distance }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: shouldReduceMotion ? 0 : -distance }}
              transition={{ duration, ease: "easeOut" }}
              role="region"
              aria-label={title}
            >
              <p>{body}</p>
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </section>
  );
}
~~~

The directive appears before imports. This component owns the event handler, state, and reduced-motion hook. The `title` and `body` values arrive as serializable strings from the Server Component.

## `app/globals.css`

~~~css
:root {
  color-scheme: light;
  font-family: system-ui, sans-serif;
  color: #172033;
  background: #f3f6fa;
}

* {
  box-sizing: border-box;
}

body {
  min-width: 20rem;
  min-height: 100vh;
  margin: 0;
}

.page-shell {
  width: min(100% - 2rem, 46rem);
  margin: 0 auto;
  padding: 4rem 0;
}

.page-heading {
  max-width: 42rem;
}

.eyebrow {
  color: #475569;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

h1 {
  line-height: 1.15;
}

.page-heading p:last-child {
  max-width: 38rem;
  line-height: 1.6;
}

.tip {
  margin-top: 2rem;
}

.tip-toggle {
  min-height: 2.75rem;
  padding: 0.7rem 1rem;
  border: 2px solid #174ea6;
  border-radius: 0.5rem;
  color: #fff;
  background: #174ea6;
  font: inherit;
  font-weight: 700;
  cursor: pointer;
}

.tip-toggle:hover {
  background: #123b7d;
}

.tip-toggle:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.tip-panel {
  max-width: 40rem;
  margin-top: 1rem;
  padding: 1.25rem;
  border: 1px solid #b8c5d6;
  border-radius: 0.75rem;
  background: #fff;
  line-height: 1.6;
}
~~~

Import `globals.css` from the root layout if the project does not already do so.

## How the boundary works

1. Next.js treats `app/page.tsx` as a Server Component by default.
2. The page imports a Motion component from `motion/react-client`, the RSC-aware entry point.
3. The heading uses `initial={false}` so its server-rendered state is visible and matches its target.
4. The page imports `ExpandableTip` as a Client Component. The page itself remains server-side.
5. `ExpandableTip` owns state and the button handler, so it declares `"use client"` at the top of its module.
6. The server page passes `title` and `body` as serializable strings. It does not pass an ordinary server-created callback.
7. The Client Component observes reduced-motion preference and changes the panel's travel distance and duration.

## Verify the behavior

- Load the route and confirm its heading and explanatory text are visible before interacting.
- View the initial page output and confirm that the server section is not emitted hidden.
- Wait for hydration, then operate the button with Tab, Enter, and Space.
- Confirm `aria-expanded` updates and `aria-controls` points to a wrapper that remains in the document.
- Open and close the panel, checking both its entrance and exit.
- Enable the operating system's reduced-motion preference and confirm the panel does not travel.
- Check the browser console for hydration warnings.
- In a production build, inspect which code enters the client bundle. Keep server data access in server modules and avoid marking the entire page as a Client Component.

## Common issues

- **The page imports `motion` from `motion/react` as a Server Component:** Use the RSC entry point `motion/react-client` for Motion components imported in the server module graph.
- **A state or event-handler error occurs:** Put that behavior in a Client Component with `"use client"` before imports.
- **A prop crossing the boundary is rejected:** Pass serializable data, and define event handlers within the client module.
- **The content appears hidden without JavaScript:** Keep the server-rendered initial state visible. Use `initial={false}` when the target is the intended initial appearance.
- **The client bundle grows unexpectedly:** Move the client boundary closer to the component that truly needs state, browser APIs, or event handlers.
- **Reduced motion has no effect:** Confirm the preference is being read from the Client Component and the animation values use the result.

Continue with [React Router transitions and navigation](028-react-router-transitions-and-navigation.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion example</a></td><td><a href="../lessons/027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components lesson</a></td><td><a href="../qnas/027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components Q&amp;A</a></td><td><a href="028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation example</a></td></tr></tbody>
</table>

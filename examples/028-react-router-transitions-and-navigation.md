# React Router transitions and navigation example

This example shows two alternative route shells:

1. A current React Router layout that wraps a persistent `Outlet` in React `ViewTransition`.
2. A Motion-specific layout that wraps a keyed route child in `AnimatePresence`.

Use one route-level transition owner at a time. The first option needs React and React DOM 19.3 or later. The Motion alternative is useful when custom Motion enter and exit values are required.

## Setup

Use a React project with React Router and Motion installed:

~~~bash
npm install react@^19.3.0 react-dom@^19.3.0 react-router motion
~~~

## Option A: React ViewTransition with React Router

### `src/main.jsx`

~~~jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import {
  createBrowserRouter,
  RouterProvider,
} from "react-router";
import { MotionConfig } from "motion/react";
import AppLayout from "./AppLayout";
import HomePage from "./HomePage";
import AboutPage from "./AboutPage";
import "./styles.css";

const router = createBrowserRouter([
  {
    element: <AppLayout />,
    children: [
      { path: "/", element: <HomePage /> },
      { path: "/about", element: <AboutPage /> },
    ],
  },
]);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <MotionConfig reducedMotion="user">
      <RouterProvider router={router} />
    </MotionConfig>
  </StrictMode>,
);
~~~

### `src/AppLayout.jsx`

~~~jsx
import { ViewTransition } from "react";
import { Link, Outlet } from "react-router";

export default function AppLayout() {
  return (
    <div className="app-shell">
      <header className="site-header">
        <p className="site-name">Route transitions</p>
        <nav aria-label="Main navigation">
          <Link to="/">Home</Link>
          <Link to="/about">About</Link>
        </nav>
      </header>

      <ViewTransition default="route-fade">
        <main className="route-main">
          <Outlet />
        </main>
      </ViewTransition>
    </div>
  );
}
~~~

The header stays outside the animated region. The `ViewTransition` boundary remains mounted while the router changes the outlet content.

### `src/HomePage.jsx`

~~~jsx
import { motion } from "motion/react";

export default function HomePage() {
  return (
    <section>
      <p className="eyebrow">Home route</p>
      <h1>Navigation owns the route</h1>
      <p>
        Use the links above to change routes. React Router updates the Outlet,
        and the persistent ViewTransition boundary animates that change.
      </p>

      <motion.article
        className="info-card"
        whileHover={{ y: -3 }}
        transition={{ duration: 0.16 }}
      >
        <h2>Motion can handle local interactions</h2>
        <p>
          This hover effect is separate from the route-level ViewTransition.
        </p>
      </motion.article>
    </section>
  );
}
~~~

### `src/AboutPage.jsx`

~~~jsx
export default function AboutPage() {
  return (
    <section>
      <p className="eyebrow">About route</p>
      <h1>A new route, the same layout</h1>
      <p>
        The URL and browser history belong to React Router. The view transition
        only changes how this route content appears.
      </p>
    </section>
  );
}
~~~

### `src/styles.css`

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

.app-shell {
  width: min(100% - 2rem, 52rem);
  margin: 0 auto;
  padding: 1rem 0 4rem;
}

.site-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 1rem 0;
  border-bottom: 1px solid #cbd5e1;
}

.site-name {
  margin: 0;
  font-weight: 700;
}

nav {
  display: flex;
  gap: 1rem;
}

a {
  color: #174ea6;
  text-underline-offset: 0.18em;
}

a:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.route-main {
  padding-top: 2rem;
}

.eyebrow {
  color: #475569;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.info-card {
  max-width: 34rem;
  margin-top: 2rem;
  padding: 1.25rem;
  border: 1px solid #b8c5d6;
  border-radius: 0.75rem;
  background: #fff;
  box-shadow: 0 0.5rem 1.5rem rgb(24 34 48 / 8%);
}

.info-card p {
  line-height: 1.6;
}

::view-transition-old(.route-fade) {
  animation: route-out 140ms ease-in both;
}

::view-transition-new(.route-fade) {
  animation: route-in 200ms ease-out both;
}

@keyframes route-out {
  to {
    opacity: 0;
  }
}

@keyframes route-in {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(.route-fade),
  ::view-transition-new(.route-fade) {
    animation: none !important;
  }
}
~~~

`MotionConfig reducedMotion="user"` applies the user's reduced-motion preference to Motion interactions in the tree. The CSS media query separately handles the browser View Transition animation.

## Option B: Motion AnimatePresence route shell

Use this alternative instead of `AppLayout` when the route itself needs Motion enter and exit targets. Do not leave Option A's full-page `ViewTransition` enabled around the same route content.

### `src/MotionAppLayout.jsx`

~~~jsx
import { AnimatePresence, motion } from "motion/react";
import { Link, useLocation, useOutlet } from "react-router";

export default function MotionAppLayout() {
  const location = useLocation();
  const outlet = useOutlet();

  return (
    <div className="app-shell">
      <header className="site-header">
        <p className="site-name">Motion route transitions</p>
        <nav aria-label="Main navigation">
          <Link to="/">Home</Link>
          <Link to="/about">About</Link>
        </nav>
      </header>

      <AnimatePresence mode="wait" initial={false}>
        <motion.main
          key={location.pathname}
          className="route-main"
          initial={{ opacity: 0, x: 16 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -16 }}
          transition={{ duration: 0.2, ease: "easeInOut" }}
        >
          {outlet}
        </motion.main>
      </AnimatePresence>
    </div>
  );
}
~~~

To use this option, replace `AppLayout` with `MotionAppLayout` in the router configuration. The presence boundary stays mounted, and its single direct Motion child receives a key that changes when the path changes.

This version uses `location.pathname`, so query-only navigation does not remount the route frame. If query and history-entry changes should also transition, choose a different key such as `location.key` and verify that behavior against the application's navigation rules.

`mode="wait"` supports one child at a time and delays entry until exit completes. Keep the exit short so navigation remains responsive. The `MotionConfig` in `main.jsx` reduces Motion transform animations according to the user's preference.

## Why the examples are alternatives

React `ViewTransition` animates a browser snapshot as React Router commits a navigation. Motion `AnimatePresence` keeps a removed React child mounted to complete its Motion `exit` target. They have different lifecycles and controls.

If both systems animate the same route change, the result can be a double fade or competing movement. Use the browser transition for a simple route snapshot effect, or the Motion route shell for custom Motion choreography.

## Verify both options

- Navigate with Home and About links and confirm the URL updates.
- Refresh each route directly and confirm it still renders.
- Use browser Back and Forward. A View Transition may not run for every popstate update, but navigation must still succeed.
- Check that the shared header remains mounted.
- Confirm the Motion option exits before entering and does not render two route pages at once.
- Try query-only navigation and confirm the selected route key matches the desired behavior.
- Enable reduced motion and check both Motion interactions and the CSS view-transition animation.
- Use the keyboard to reach and activate each navigation link.
- Check console output and verify route loading, errors, and not-found states remain understandable.

## Troubleshooting

- **React reports that ViewTransition is unavailable:** Check that both React and React DOM are at least 19.3 and that the project uses compatible versions.
- **The route changes without a cross-fade:** Confirm the persistent boundary wraps the outlet, the router's Transition integration is enabled, and the browser supports view transitions.
- **A Motion exit does not play:** Keep AnimatePresence mounted and give its direct child a stable key that changes for the intended route changes.
- **Every query update animates unexpectedly:** Use pathname as the key if only path changes should transition.
- **The transition feels slow:** Shorten the exit and entry. In wait mode the new page intentionally waits for exit to finish.
- **Reduced motion works for the card but not the route:** Keep the CSS reduced-motion rule because MotionConfig does not control browser View Transition pseudo-elements.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components example</a></td><td><a href="../lessons/028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation lesson</a></td><td><a href="../qnas/028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation Q&amp;A</a></td><td><a href="029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion example</a></td></tr></tbody>
</table>

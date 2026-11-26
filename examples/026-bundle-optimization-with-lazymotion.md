# Bundle optimization with LazyMotion example

This small React example uses the lightweight `m` components and the synchronous `domAnimation` feature bundle. A button reveals an animated information panel. The button keeps a stable `aria-controls` target, and the example shortens the animation when the user prefers reduced motion.

## What this example demonstrates

- A single `LazyMotion` boundary supplies the selected feature bundle.
- Animated elements use `m` rather than `motion`.
- Strict mode catches an accidental regular `motion` component inside the boundary.
- The native button remains keyboard-operable and exposes its expanded state.
- The content remains directly available after the user activates the button.
- The example measures the built app instead of claiming a guaranteed bundle saving.

## Setup

Use an existing React application, then install Motion:

~~~bash
npm install motion
~~~

Save the following files under `src`. Import the stylesheet from the component file.

## `src/LazyFeatureDemo.jsx`

~~~jsx
import { useState } from "react";
import {
  AnimatePresence,
  LazyMotion,
  domAnimation,
  useReducedMotion,
} from "motion/react";
import * as m from "motion/react-m";
import "./lazy-feature-demo.css";

function FeaturePanel({ reduceMotion }) {
  const travel = reduceMotion ? 0 : 12;
  const duration = reduceMotion ? 0 : 0.18;

  return (
    <m.section
      className="feature-panel"
      initial={{ opacity: 0, y: travel }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: reduceMotion ? 0 : -8 }}
      transition={{ duration, ease: "easeOut" }}
      aria-labelledby="feature-panel-title"
    >
      <p className="feature-label">Loaded feature set</p>
      <h2 id="feature-panel-title">A smaller feature choice</h2>
      <p>
        This panel uses animation and presence features from domAnimation.
        It does not need drag or layout animation.
      </p>
      <ul>
        <li>Use m components inside the LazyMotion boundary.</li>
        <li>Choose a bundle that supports every feature the interface uses.</li>
        <li>Compare production builds before and after the change.</li>
      </ul>
    </m.section>
  );
}

function LazyFeatureDemo() {
  const [open, setOpen] = useState(false);
  const reduceMotion = useReducedMotion();

  return (
    <main className="demo-shell">
      <p className="demo-label">Motion bundle learning example</p>
      <h1>Load the features you need</h1>
      <p className="demo-intro">
        Activate the button to reveal details. The control works without
        hover, and its expanded state is available to assistive technology.
      </p>

      <button
        className="feature-toggle"
        type="button"
        aria-expanded={open}
        aria-controls="feature-panel-container"
        onClick={() => setOpen((current) => !current)}
      >
        {open ? "Hide bundle details" : "Show bundle details"}
      </button>

      <div id="feature-panel-container">
        <AnimatePresence initial={false}>
          {open && (
            <FeaturePanel key="feature-panel" reduceMotion={reduceMotion} />
          )}
        </AnimatePresence>
      </div>
    </main>
  );
}

export default function App() {
  return (
    <LazyMotion features={domAnimation} strict>
      <LazyFeatureDemo />
    </LazyMotion>
  );
}
~~~

## `src/lazy-feature-demo.css`

~~~css
:root {
  color-scheme: light;
  font-family: system-ui, sans-serif;
  color: #182230;
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

.demo-shell {
  width: min(100% - 2rem, 42rem);
  margin: 0 auto;
  padding: 4rem 0;
}

.demo-label,
.feature-label {
  color: #475569;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

h1,
h2 {
  line-height: 1.2;
}

.demo-intro {
  max-width: 38rem;
  line-height: 1.6;
}

.feature-toggle {
  min-height: 2.75rem;
  margin: 1rem 0;
  padding: 0.7rem 1rem;
  border: 2px solid #174ea6;
  border-radius: 0.5rem;
  color: #fff;
  background: #174ea6;
  font: inherit;
  font-weight: 700;
  cursor: pointer;
}

.feature-toggle:hover {
  background: #123b7d;
}

.feature-toggle:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.feature-panel {
  margin-top: 1rem;
  padding: 1.25rem;
  border: 1px solid #b8c5d6;
  border-radius: 0.75rem;
  background: #fff;
  box-shadow: 0 0.5rem 1.5rem rgb(24 34 48 / 8%);
}

.feature-panel p,
.feature-panel li {
  line-height: 1.6;
}

.feature-panel ul {
  padding-left: 1.25rem;
}
~~~

## How the example works

1. `App` places the animated interface inside `LazyMotion` and supplies `domAnimation` synchronously.
2. `strict` helps catch a regular `motion` component accidentally rendered inside the boundary.
3. The animated panel is rendered with `m.section`. `AnimatePresence` keeps the exiting element mounted long enough to run its exit animation.
4. The native button updates `aria-expanded` and toggles the panel. Its `aria-controls` points to a wrapper that remains in the document.
5. `useReducedMotion` is used to remove the panel's travel distance and make its transition immediate when reduced motion is preferred.
6. The example does not request drag or layout features, so it does not need `domMax`.

## Try it

- Use Tab and Enter or Space to operate the button.
- Check that `aria-expanded` changes between `false` and `true`.
- Open and close the panel to see its enter and exit states.
- Enable the operating system's reduced-motion setting and confirm the panel no longer travels.
- Temporarily replace `m.section` with `motion.section` and confirm strict mode catches the unsupported component choice.
- Make a production build before and after the feature-boundary change. Compare the same build outputs and note the bundler and settings.
- If you add drag or layout animation, change the selected feature bundle and repeat the checks.

## Troubleshooting

- **The animation does not run:** Check that the element uses `m` and is rendered below the `LazyMotion` boundary.
- **Strict mode throws:** Find and replace a regular `motion` component inside the boundary, or remove it only after confirming that the full component is intentional.
- **A gesture or layout prop does not work:** Confirm the feature bundle contains that capability. `domAnimation` does not include pan, drag, or layout animation.
- **The measured bundle is unchanged:** Inspect the production chunks and imports. The result depends on the bundler and the features already included in the application.
- **Content is hard to find without animation:** Keep the control and its accessible state clear. Animation should not be required to understand what the button does.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="025-responsive-motion-patterns.md">025 - Responsive motion patterns example</a></td><td><a href="../lessons/026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion lesson</a></td><td><a href="../qnas/026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion Q&amp;A</a></td><td><a href="027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components example</a></td></tr></tbody>
</table>

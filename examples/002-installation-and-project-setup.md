# Installation and project setup example

This example creates a small Vite React app, installs Motion with npm, and renders one animated component. It assumes the current Motion for React setup, which uses the `motion` package and imports React APIs from `motion/react`.

## Prerequisites

- Node.js and npm installed.
- A terminal that can create a project and install npm packages.
- React familiarity sufficient to edit a component.

## Create the project

Run these commands in the parent directory where the demo should live:

~~~bash
npm create vite@latest motion-setup -- --template react
cd motion-setup
npm install
npm install motion
npm run dev
~~~

The development server prints a local address in the terminal. Keep that process running while you edit the source files.

If a React Vite project already exists, skip project creation and run `npm install motion` from its root. Use the package manager that matches the existing lockfile.

## File structure

Replace or create the following files in the generated project:

~~~text
motion-setup/
  package.json
  package-lock.json
  index.html
  src/
    main.jsx
    App.jsx
    App.css
~~~

The package manager updates `package.json` and `package-lock.json` when Motion is installed. Keep both changes.

## `src/main.jsx`

~~~jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App.jsx";
import "./App.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <App />
  </StrictMode>,
);
~~~

Vite's generated `index.html` provides the `root` element. Keep the generated HTML entry file unless your project has a deliberate reason to change it.

## `src/App.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

export default function App() {
  const [open, setOpen] = useState(false);

  return (
    <main className="setup-page">
      <p className="setup-page__eyebrow">Chapter 002</p>
      <h1>Motion is connected</h1>
      <p className="setup-page__intro">
        The app renders a React component and animates its current target.
      </p>

      <button
        className="setup-page__toggle"
        type="button"
        aria-expanded={open}
        aria-controls="setup-details"
        onClick={() => setOpen((current) => !current)}
      >
        {open ? "Hide details" : "Show details"}
      </button>

      <motion.section
        id="setup-details"
        className="setup-card"
        initial={{ opacity: 0, y: 12 }}
        animate={{
          opacity: open ? 1 : 0.82,
          y: open ? 0 : 5,
          scale: open ? 1 : 0.99,
        }}
        transition={{ duration: 0.28, ease: "easeOut" }}
        aria-label="Motion setup details"
      >
        <h2>{open ? "Ready to learn" : "Setup complete"}</h2>
        <p>
          The app imports motion from motion/react and uses React state to
          choose the card's visual target.
        </p>
        <code>npm ls motion react</code>
      </motion.section>
    </main>
  );
}
~~~

The component's boolean state controls the card's target. The `initial` object describes its first visual state. Motion then interpolates toward the values in `animate`. The button is a regular HTML button and exposes whether its controlled region is expanded.

This example keeps the section mounted while its opacity changes. That is intentional for this setup lesson. If content should leave the DOM after an exit animation, use `AnimatePresence` as explained in a later chapter.

## `src/App.css`

~~~css
:root {
  font-family: Inter, ui-sans-serif, system-ui, sans-serif;
  color: #182238;
  background: #eff4fb;
  font-synthesis: none;
  text-rendering: optimizeLegibility;
}

* {
  box-sizing: border-box;
}

body {
  min-width: 320px;
  min-height: 100vh;
  margin: 0;
}

button {
  font: inherit;
}

.setup-page {
  width: min(100% - 2rem, 720px);
  margin: 0 auto;
  padding: 5rem 0;
}

.setup-page__eyebrow {
  color: #315f9c;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.setup-page h1 {
  margin-bottom: 0.5rem;
  font-size: clamp(2.2rem, 7vw, 4rem);
}

.setup-page__intro {
  max-width: 38rem;
  color: #53627a;
  line-height: 1.6;
}

.setup-page__toggle {
  min-height: 2.75rem;
  margin: 1rem 0;
  padding: 0.65rem 1rem;
  border: 0;
  border-radius: 0.6rem;
  color: white;
  background: #245da8;
  cursor: pointer;
}

.setup-page__toggle:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.setup-card {
  padding: 1.5rem;
  border: 1px solid #d2dbea;
  border-radius: 1rem;
  background: white;
  box-shadow: 0 12px 32px rgb(24 34 56 / 10%);
}

.setup-card p {
  color: #53627a;
  line-height: 1.6;
}

.setup-card code {
  display: inline-block;
  padding: 0.4rem 0.55rem;
  border-radius: 0.35rem;
  background: #eef2f7;
}
~~~

## Verify the setup

From the project root:

~~~bash
npm ls motion react
~~~

Confirm that both packages appear in the dependency tree. Then check that `npm run dev` is still running and the browser shows the heading, toggle button, and card. Click the button and confirm the card changes to its open target. This repository does not include a Vite project, so the commands are instructions for running the example in a local React app.

## Expected behavior

- The card starts at its compact target with slightly lower opacity.
- Selecting the button changes the target and animates the card.
- The button's label changes between “Show details” and “Hide details”.
- The `aria-expanded` attribute tracks the selected state.
- The browser console has no module-resolution error for `motion/react`.

## Troubleshooting

- **The page is blank.** Check the browser console, the Vite terminal, the root element ID, and the import paths.
- **Motion cannot be resolved.** Run the package installation from the directory containing the intended `package.json`.
- **The app uses pnpm or Yarn.** Install Motion with that tool and preserve its lockfile instead of adding an npm lockfile.
- **The card changes instantly.** Confirm the `motion` import and that the element is rendered as `motion.section`, not a regular `section`.
- **The button does not work.** Confirm that the click handler changes `open` and that the app renders this `App` component.
- **The card disappears unexpectedly.** This component does not conditionally render the card. Check for other state or CSS rules affecting the section.

## Practice

1. Change the transition duration, reload, and compare the timing.
2. Change the card background through a CSS class instead of animating color. Observe which part is a normal React class update.
3. Add an `aria-live` status paragraph that reports whether the card is open.
4. Keep the component in place and create a separate project branch before experimenting with dependency upgrades.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals</a></td><td><a href="../lessons/002-installation-and-project-setup.md">002 - Installation and project setup lesson</a></td><td><a href="../qnas/002-installation-and-project-setup.md">002 - Installation and project setup Q&amp;A</a></td><td><a href="003-motion-components-and-props.md">003 - Motion components and props example</a></td></tr>
  </tbody>
</table>

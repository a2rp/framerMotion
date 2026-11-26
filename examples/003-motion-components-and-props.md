# Motion components and props example

This example uses several Motion components while keeping their underlying HTML elements meaningful. The heading is a heading, the status panel is a section, and the action is a native button. Standard React props and Motion props are shown together.

## Prerequisites

- A React application with Motion installed.
- A bundler such as Vite.
- Familiarity with components, props, and React state.

If needed, install Motion from the project root:

~~~bash
npm install motion
~~~

## File structure

~~~text
src/
  App.jsx
  App.css
~~~

## `src/App.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";
import "./App.css";

export default function App() {
  const [detailsOpen, setDetailsOpen] = useState(false);

  return (
    <main className="component-demo">
      <motion.header
        className="component-demo__header"
        initial={{ opacity: 0, y: 12 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.35, ease: "easeOut" }}
      >
        <p className="component-demo__eyebrow">Chapter 003</p>
        <h1>Motion components keep HTML meaning</h1>
        <p>
          The following elements use Motion props and remain normal semantic
          elements in the rendered page.
        </p>
      </motion.header>

      <motion.section
        className="component-demo__panel"
        aria-labelledby="panel-title"
        animate={{
          opacity: detailsOpen ? 1 : 0.9,
          x: detailsOpen ? 0 : 4,
        }}
        transition={{ duration: 0.22 }}
      >
        <h2 id="panel-title">Status panel</h2>
        <p>
          This is a section with a class, an accessible heading relationship,
          and animated visual values.
        </p>

        <motion.button
          className="component-demo__button"
          type="button"
          aria-expanded={detailsOpen}
          aria-controls="component-details"
          whileHover={{ scale: 1.03 }}
          whileTap={{ scale: 0.98 }}
          transition={{ duration: 0.15 }}
          onClick={() => setDetailsOpen((open) => !open)}
        >
          {detailsOpen ? "Hide details" : "Show details"}
        </motion.button>

        <motion.div
          id="component-details"
          className="component-demo__details"
          animate={{
            opacity: detailsOpen ? 1 : 0.55,
            y: detailsOpen ? 0 : 5,
          }}
        >
          The button is a real button. Its accessible expanded state is separate
          from the animated opacity and position.
        </motion.div>
      </motion.section>
    </main>
  );
}
~~~

The `className`, `aria-labelledby`, `type`, `aria-expanded`, `aria-controls`, and `onClick` props are ordinary React or HTML props. Motion handles `initial`, `animate`, `transition`, `whileHover`, and `whileTap`. The section stays mounted in this example, so its closed visual state does not mean the content has been removed.

## `src/App.css`

~~~css
:root {
  font-family: Inter, ui-sans-serif, system-ui, sans-serif;
  color: #172033;
  background: #f1f5fa;
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

.component-demo {
  width: min(100% - 2rem, 760px);
  margin: 0 auto;
  padding: 4rem 0;
}

.component-demo__header {
  margin-bottom: 2rem;
}

.component-demo__eyebrow {
  color: #315f9c;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.component-demo__header h1 {
  max-width: 16ch;
  margin: 0.4rem 0 0.8rem;
  font-size: clamp(2rem, 6vw, 3.5rem);
  line-height: 1.08;
}

.component-demo__header p:last-child,
.component-demo__panel p {
  color: #526079;
  line-height: 1.6;
}

.component-demo__panel {
  padding: 1.5rem;
  border: 1px solid #d1dbea;
  border-radius: 1rem;
  background: white;
  box-shadow: 0 12px 32px rgb(23 32 51 / 9%);
}

.component-demo__panel h2 {
  margin-top: 0;
}

.component-demo__button {
  min-height: 2.75rem;
  padding: 0.65rem 1rem;
  border: 0;
  border-radius: 0.55rem;
  color: white;
  background: #245da8;
  cursor: pointer;
}

.component-demo__button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.component-demo__details {
  margin-top: 1rem;
  padding: 1rem;
  border-left: 4px solid #6c9bd7;
  background: #eef4fc;
  line-height: 1.55;
}
~~~

## Run and inspect

Start the development server with the app's existing command, commonly `npm run dev`. Confirm the page shows the heading and status panel. Hover or press the button to see its temporary targets. Click the button to update React state and observe the panel values change.

The expected result is a rendered page with a fading heading, a state-driven status section, and a button that responds to hover, press, and click. The HTML remains semantic in the browser's accessibility tree.

## Variations

- Replace `motion.header` with a regular `header`, since it has no specific interaction after its entrance. Compare the result.
- Change `motion.section` to a normal `section` and note that its `animate` prop no longer has an effect.
- Add an accessible label to the button and inspect the rendered HTML.
- Move the details text into a conditional render. Then decide whether an exit animation is needed.

## Common mistakes

- A Motion prop such as `animate` has no animation behavior on a regular HTML element.
- A Motion component does not automatically make an element interactive or accessible.
- Hiding content with opacity alone does not remove its keyboard or accessibility behavior.
- Defining every node as a Motion component adds unnecessary abstraction.
- CSS and Motion can conflict when both try to control the same transform.
- A custom React component needs additional ref and style forwarding; wrapping it is not shown here.

## Practice

1. Add `whileFocus` to the button and verify keyboard focus visibly changes it.
2. Add an `aria-live` status message that reports the current state.
3. Use a property-specific transition so opacity and x do not share the same timing.
4. Identify the element type rendered by each Motion component in the example.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="002-installation-and-project-setup.md">002 - Installation and project setup</a></td><td><a href="../lessons/003-motion-components-and-props.md">003 - Motion components and props lesson</a></td><td><a href="../qnas/003-motion-components-and-props.md">003 - Motion components and props Q&amp;A</a></td><td><a href="004-initial-and-animate-states.md">004 - Initial and animate states example</a></td></tr>
  </tbody>
</table>

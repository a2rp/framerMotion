# Initial and animate states example

This example shows the difference between a component's first target and the current target. A button changes React state, which selects a new `animate` object. A second button toggles whether the card should start from a separate entrance state on the next page load.

## Prerequisites and setup

Use a React project with Motion installed:

~~~bash
npm install motion
~~~

Place the files under the project's `src/` directory. Render `<InitialAndAnimate />` from the app component.

## File structure

~~~text
src/
  InitialAndAnimate.jsx
  InitialAndAnimate.css
~~~

## `src/InitialAndAnimate.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";
import "./InitialAndAnimate.css";

export function InitialAndAnimate() {
  const [active, setActive] = useState(false);
  const [skipEntrance, setSkipEntrance] = useState(false);

  return (
    <main className="initial-demo">
      <p className="initial-demo__eyebrow">Chapter 004</p>
      <h1>Choose an animation target</h1>

      <div className="initial-demo__controls">
        <button
          type="button"
          aria-pressed={active}
          onClick={() => setActive((value) => !value)}
        >
          {active ? "Set inactive" : "Set active"}
        </button>
        <button
          type="button"
          aria-pressed={skipEntrance}
          onClick={() => setSkipEntrance((value) => !value)}
        >
          {skipEntrance ? "Use entrance" : "Skip entrance"}
        </button>
      </div>

      <motion.section
        className="initial-demo__card"
        initial={skipEntrance ? false : { opacity: 0, y: 18 }}
        animate={
          active
            ? { opacity: 1, y: 0, scale: 1 }
            : { opacity: 0.72, y: 6, scale: 0.98 }
        }
        transition={{ duration: 0.3, ease: "easeOut" }}
        aria-labelledby="initial-card-title"
      >
        <h2 id="initial-card-title">{active ? "Active" : "Inactive"}</h2>
        <p>
          React state chooses the destination. Motion transitions the visual
          values from the current frame.
        </p>
      </motion.section>

      <p className="initial-demo__note" aria-live="polite">
        Entrance setting: {skipEntrance ? "skipped on first render" : "enabled"}.
      </p>
    </main>
  );
}
~~~

Changing `active` selects an `animate` target. Changing `skipEntrance` changes the `initial` prop. To compare the mount behavior, toggle the entrance setting and reload the page so the component mounts with the new value. Changing `initial` after mount does not restart the initial lifecycle.

## `src/InitialAndAnimate.css`

~~~css
:root {
  font-family: Inter, ui-sans-serif, system-ui, sans-serif;
  color: #182238;
  background: #f1f5fa;
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

.initial-demo {
  width: min(100% - 2rem, 720px);
  margin: 0 auto;
  padding: 4rem 0;
}

.initial-demo__eyebrow {
  color: #315f9c;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.initial-demo__controls {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  margin: 1.5rem 0;
}

.initial-demo__controls button {
  min-height: 2.7rem;
  padding: 0.6rem 0.9rem;
  border: 0;
  border-radius: 0.55rem;
  color: white;
  background: #245da8;
  cursor: pointer;
}

.initial-demo__controls button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.initial-demo__card {
  padding: 1.5rem;
  border: 1px solid #d2dbea;
  border-radius: 1rem;
  background: white;
  box-shadow: 0 12px 32px rgb(24 34 56 / 9%);
}

.initial-demo__card p,
.initial-demo__note {
  color: #53627a;
  line-height: 1.6;
}
~~~

## Run and observe

Render the component from the app and start its normal development command, commonly `npm run dev`.

1. Reload with “Use entrance” selected. The card begins transparent and slightly lower, then reaches its inactive target.
2. Reload with “Skip entrance” selected. The card renders at its `animate` target without the entrance.
3. Select “Set active”. Motion transitions to the active target.
4. Select “Set inactive” again. Motion transitions to the inactive target.
5. Use keyboard navigation and confirm both buttons have a visible focus outline.

The card remains mounted in both visual states. The buttons communicate their own toggle state with `aria-pressed`.

## Variations and common mistakes

- Replace the conditional `initial` prop with `initial={false}` directly. Confirm that later changes to `active` still animate.
- Remove `transition` and observe the installed version's default behavior.
- Set opacity to zero and inspect whether the content is still focusable. Opacity alone does not remove content from interaction.
- Do not expect `exit` to run when React removes a component unless an `AnimatePresence` boundary is used.
- Do not change the React key on every render; that remounts the card and repeats its initial behavior.
- Keep the server and client initial output consistent in a server-rendered application.

## Practice

Add an `initiallyActive` prop and use it to initialize React state. Decide whether the first paint should animate from inactive or start directly at the active target. Explain why the answer depends on the intended initial state.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="003-motion-components-and-props.md">003 - Motion components and props</a></td><td><a href="../lessons/004-initial-and-animate-states.md">004 - Initial and animate states lesson</a></td><td><a href="../qnas/004-initial-and-animate-states.md">004 - Initial and animate states Q&amp;A</a></td><td><a href="005-transitions-and-springs.md">005 - Transitions and springs example</a></td></tr>
  </tbody>
</table>

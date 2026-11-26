# Migrating from framer-motion to Motion example

This example migrates a regular React application from the framer-motion package to the current motion package. It changes the dependency and React import path while preserving the same animation behavior. The sample includes an accessible card switcher so enter, exit, and reduced-motion behavior can be reviewed after the migration.

This workflow is for a regular React application. Framer Code Components and Overrides use the Framer-specific integration and should not be changed mechanically.

## 1. Record the current version

From the project root, record the installed package and save the current production build output:

~~~bash
npm ls framer-motion
npm run build
~~~

Use the project's actual production build script if it is named differently. Keep the build settings constant for the later comparison.

## 2. Replace the dependency

Remove the old package and add the current package:

~~~bash
npm uninstall framer-motion
npm install motion
~~~

Commit the updated package manifest and lockfile together.

## 3. Change the React import

The old component can import React APIs from framer-motion:

~~~jsx
import { AnimatePresence, motion } from "framer-motion";
~~~

Change the import path to motion/react:

~~~jsx
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
~~~

The component continues to use the same React Motion API. The path change alone does not require rewriting its animation targets.

## 4. Migrated component

Save as src/CardSwitcher.jsx:

~~~jsx
import { useState } from "react";
import {
  AnimatePresence,
  motion,
  useReducedMotion,
} from "motion/react";
import "./card-switcher.css";

const cards = [
  {
    id: "entrance",
    title: "Keep the behavior",
    body: "A package migration should preserve the existing interaction.",
  },
  {
    id: "verify",
    title: "Check the exit",
    body: "AnimatePresence retains the outgoing card until its exit finishes.",
  },
  {
    id: "measure",
    title: "Compare the production build",
    body: "Use the same command and settings before and after the package change.",
  },
];

export default function CardSwitcher() {
  const [index, setIndex] = useState(0);
  const shouldReduceMotion = useReducedMotion();
  const card = cards[index];
  const distance = shouldReduceMotion ? 0 : 16;
  const duration = shouldReduceMotion ? 0 : 0.18;

  function showPrevious() {
    setIndex((current) => (current - 1 + cards.length) % cards.length);
  }

  function showNext() {
    setIndex((current) => (current + 1) % cards.length);
  }

  return (
    <main className="switcher">
      <p className="switcher-label">Motion migration example</p>
      <h1>Keep the interface behavior</h1>
      <p className="switcher-intro">
        Move between cards with the buttons. The current card is identified
        by its stable ID, so presence tracking can distinguish it from the
        next card.
      </p>

      <div className="switcher-controls">
        <button type="button" onClick={showPrevious}>
          Previous card
        </button>
        <button type="button" onClick={showNext}>
          Next card
        </button>
      </div>

      <div className="card-stage">
        <AnimatePresence mode="wait" initial={false}>
          <motion.article
            key={card.id}
            className="switcher-card"
            initial={{ opacity: 0, x: distance }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: shouldReduceMotion ? 0 : -distance }}
            transition={{ duration, ease: "easeInOut" }}
            aria-labelledby="switcher-card-title"
          >
            <p className="card-count">
              Card {index + 1} of {cards.length}
            </p>
            <h2 id="switcher-card-title">{card.title}</h2>
            <p>{card.body}</p>
          </motion.article>
        </AnimatePresence>
      </div>
    </main>
  );
}
~~~

## 5. Styles

Save as src/card-switcher.css:

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

.switcher {
  width: min(100% - 2rem, 42rem);
  margin: 0 auto;
  padding: 4rem 0;
}

.switcher-label,
.card-count {
  color: #475569;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.switcher-intro,
.switcher-card p {
  line-height: 1.6;
}

.switcher-controls {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  margin: 1.5rem 0;
}

.switcher-controls button {
  min-height: 2.75rem;
  padding: 0.65rem 0.9rem;
  border: 2px solid #174ea6;
  border-radius: 0.5rem;
  color: #fff;
  background: #174ea6;
  font: inherit;
  font-weight: 700;
  cursor: pointer;
}

.switcher-controls button:hover {
  background: #123b7d;
}

.switcher-controls button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.card-stage {
  min-height: 13rem;
}

.switcher-card {
  min-height: 12rem;
  padding: 1.25rem;
  border: 1px solid #b8c5d6;
  border-radius: 0.75rem;
  background: #fff;
  box-shadow: 0 0.5rem 1.5rem rgb(24 34 48 / 8%);
}
~~~

## 6. Check for older APIs

The component above uses current API props. If the existing application uses older APIs, update only the ones present and follow the relevant major-version notes. For example, an old exitBeforeEnter prop may need mode="wait", and older layout-transition props may need the layout prop and regular transition configuration.

Do not change every historical API listed in an upgrade guide if the project does not use it.

## 7. Find remaining imports

Search application source, tests, stories, examples, and workspace packages for framer-motion. Review every result. Keep the package import only where the code is intentionally using Framer's own Code Components or Overrides integration.

Check the dependency tree:

~~~bash
npm ls motion framer-motion
~~~

For a regular application migration, confirm that motion is installed and no unintended framer-motion copy remains.

## 8. Verify the migration

- Start the application with its normal development command.
- Use Tab, Enter, and Space to operate both buttons.
- Move through several cards and confirm each outgoing card exits before the next enters.
- Enable reduced motion and confirm the card does not travel.
- Run the same production build command recorded before migration.
- Compare the relevant chunks and inspect whether both package names remain in the dependency graph.
- Check console warnings, type errors, hydration behavior, and styled component prop forwarding where relevant.

A successful import and build confirm only that the code compiles. Check the interactions and production output as well.

## Troubleshooting

- **Cannot resolve motion/react:** Confirm the motion package is installed in this workspace and that the import is in a React project.
- **The old import still works in one file:** Search by package name across the complete repository and classify the remaining use.
- **A Framer Code Component breaks:** Restore the integration import required by the Framer platform for that file.
- **The exit does not run:** Keep AnimatePresence mounted and ensure its direct child has a stable key that changes when the card changes.
- **A TypeScript or styling error appears:** Check the major-version notes and styling-library prop forwarding for the versions being used.
- **Two package copies appear:** Inspect workspace dependency declarations and regenerate the lockfile with the project's chosen package manager.
- **The output size is not smaller:** Compare production chunks with the same build settings. The package rename is not itself a bundle-size optimization.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation example</a></td><td><a href="../lessons/029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion lesson</a></td><td><a href="../qnas/029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion Q&amp;A</a></td><td><a href="030-performance-and-debugging.md">030 - Performance and debugging example</a></td></tr></tbody>
</table>

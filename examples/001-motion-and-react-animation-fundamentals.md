# Motion and React animation fundamentals example

This example turns the lesson's state-to-target idea into a small React interface. A native button selects one card. React owns the selected ID; Motion animates the card's opacity, vertical position, background color, and hover scale.

## Prerequisites

- Node.js and npm installed.
- Basic familiarity with React components and JSX.
- A terminal opened in the folder where you want the demo.

If you already have a Vite React project, install Motion in that project:

~~~bash
npm install motion
~~~

To make a new Vite React project, run:

~~~bash
npm create vite@latest motion-fundamentals -- --template react
cd motion-fundamentals
npm install
npm install motion
npm run dev
~~~

Replace or add the files below inside the project's `src/` directory. In an existing project, render `<MotionFundamentals />` from its app component.

## File structure

~~~text
src/
  MotionFundamentals.jsx
  MotionFundamentals.css
  App.jsx
~~~

## `src/MotionFundamentals.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";
import "./MotionFundamentals.css";

const concepts = [
  {
    id: "react-state",
    title: "React state",
    description: "Stores which card is selected and why the interface changed.",
  },
  {
    id: "motion-target",
    title: "Motion target",
    description: "Describes the visual values the selected card should reach.",
  },
  {
    id: "interpolation",
    title: "Interpolation",
    description: "Moves supported visual values smoothly between their targets.",
  },
];

export function MotionFundamentals() {
  const [selectedId, setSelectedId] = useState(null);

  function toggleSelection(id) {
    setSelectedId((currentId) => (currentId === id ? null : id));
  }

  return (
    <main className="motion-demo">
      <header className="motion-demo__header">
        <p className="motion-demo__eyebrow">Chapter 001</p>
        <h1>Motion and React animation fundamentals</h1>
        <p>
          Select a card to change React state. Motion animates the card to the
          target chosen by that state.
        </p>
      </header>

      <ul className="motion-demo__grid">
        {concepts.map((concept, index) => {
          const selected = selectedId === concept.id;

          return (
            <li key={concept.id}>
              <motion.article
                className="motion-card"
                aria-labelledby={concept.id + "-title"}
                initial={{ opacity: 0, y: 14 }}
                animate={{
                  opacity: selected ? 1 : 0.88,
                  y: selected ? 0 : 4,
                  backgroundColor: selected ? "#e9f3ff" : "#ffffff",
                }}
                whileHover={{ scale: 1.02 }}
                transition={{
                  duration: 0.28,
                  delay: index * 0.06,
                  ease: "easeOut",
                }}
              >
                <h2 id={concept.id + "-title"}>{concept.title}</h2>
                <p>{concept.description}</p>
                <button
                  type="button"
                  aria-pressed={selected}
                  onClick={() => toggleSelection(concept.id)}
                >
                  {selected ? "Unselect" : "Select"}
                </button>
              </motion.article>
            </li>
          );
        })}
      </ul>

      <p className="motion-demo__status" aria-live="polite">
        {selectedId
          ? "Selected: " + concepts.find((item) => item.id === selectedId).title
          : "No card is selected."}
      </p>
    </main>
  );
}
~~~

The `concepts` array provides stable IDs and content. The `selectedId` state stores the selected card, not its animation frame. Each render computes a boolean and passes a target object to `animate`. Motion interpolates the change. The functional state update avoids relying on an old value if React batches updates.

The native button exposes its selected state through `aria-pressed`. The live status makes the current selection available to assistive technology. Hover only changes scale; clicking the button remains the way to select a card.

## `src/MotionFundamentals.css`

~~~css
:root {
  font-family: Inter, ui-sans-serif, system-ui, sans-serif;
  color: #172033;
  background: #f3f6fb;
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

.motion-demo {
  width: min(100% - 2rem, 960px);
  margin: 0 auto;
  padding: 4rem 0;
}

.motion-demo__header {
  max-width: 680px;
  margin-bottom: 2rem;
}

.motion-demo__eyebrow {
  color: #315f9c;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.motion-demo__header h1 {
  margin: 0.35rem 0 0.75rem;
  font-size: clamp(2rem, 5vw, 3.25rem);
  line-height: 1.1;
}

.motion-demo__header p:last-child {
  color: #526079;
  line-height: 1.6;
}

.motion-demo__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 240px), 1fr));
  gap: 1rem;
  margin: 0;
  padding: 0;
  list-style: none;
}

.motion-card {
  min-height: 220px;
  padding: 1.4rem;
  border: 1px solid #d6deeb;
  border-radius: 1rem;
  box-shadow: 0 8px 24px rgb(22 39 68 / 8%);
}

.motion-card h2 {
  margin-top: 0;
  font-size: 1.2rem;
}

.motion-card p {
  min-height: 4.5rem;
  color: #526079;
  line-height: 1.55;
}

.motion-card button {
  min-height: 2.6rem;
  padding: 0.55rem 0.9rem;
  border: 0;
  border-radius: 0.55rem;
  color: white;
  background: #245da8;
  cursor: pointer;
}

.motion-card button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.motion-demo__status {
  min-height: 1.5rem;
  margin-top: 1.5rem;
  color: #34435c;
}
~~~

## `src/App.jsx`

~~~jsx
import { MotionFundamentals } from "./MotionFundamentals.jsx";

export default function App() {
  return <MotionFundamentals />;
}
~~~

## Run and inspect

Run `npm run dev` from the project root, open the local URL printed by Vite, then:

1. Watch the three cards enter with a small fade and vertical movement.
2. Select a card. Its visual target changes while the live status reports the selected concept.
3. Select the same card again. Its state returns to unselected.
4. Tab to each button and check that the focus outline remains visible.
5. Hover over a card with a pointer. Hover changes its scale but does not select it.

The expected result is a three-card grid with at most one selected card. The selected card has full opacity, no vertical offset, and a pale blue background. State changes do not require a hand-written animation loop.

## Variations

- Replace the background-color target with a border-color target. Keep both start and end values valid CSS colors.
- Add a fourth concept object and confirm that its stable ID is unique.
- Change the transition duration and observe how the pace changes.
- Add a separate boolean state for a details panel. Keep the panel's meaning in React state rather than deriving it from the card's opacity.

## Common mistakes to avoid

- Do not use the array index as a key for reorderable content. The example uses stable concept IDs.
- Do not put per-frame opacity or position values into React state.
- Do not use a clickable article instead of a button for the selection action.
- Do not expect opacity zero to remove an item or disable its controls.
- Define components at module scope. Avoid creating a new component type inside another component's render function.
- Do not add a hover target that overwrites the selection target for the same property without deciding how the states should combine.

## Practice tasks

1. Add a reset button that clears `selectedId`.
2. Add a visual selected marker and make sure its text is not the only indicator.
3. Add `initial={false}` and compare the first render. Explain why the entrance animation no longer runs on mount.
4. Test the example with keyboard only. Then review the reduced-motion chapter before using larger movement in a real interface.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td>Start of course</td><td><a href="../lessons/001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals lesson</a></td><td><a href="../qnas/001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals Q&amp;A</a></td><td><a href="002-installation-and-project-setup.md">002 - Installation and project setup example</a></td></tr>
  </tbody>
</table>

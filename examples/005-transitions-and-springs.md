# Transitions and springs example

This example moves a card between two horizontal positions with either a tween or a spring. The target position and the transition type are separate choices: a button changes the target, while a radio-style pair of buttons chooses the timing model.

## Prerequisites

Use a React project with Motion installed:

~~~bash
npm install motion
~~~

## File structure

~~~text
src/
  TransitionDemo.jsx
  TransitionDemo.css
~~~

## `src/TransitionDemo.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";
import "./TransitionDemo.css";

export function TransitionDemo() {
  const [position, setPosition] = useState("start");
  const [kind, setKind] = useState("tween");

  const transition =
    kind === "spring"
      ? { type: "spring", stiffness: 250, damping: 22, mass: 0.9 }
      : { type: "tween", duration: 0.55, ease: "easeInOut" };

  return (
    <main className="transition-demo">
      <p className="transition-demo__eyebrow">Chapter 005</p>
      <h1>Compare transition types</h1>
      <p>Choose a transition, then move the card between its two targets.</p>

      <div className="transition-demo__controls" aria-label="Transition type">
        <button
          type="button"
          aria-pressed={kind === "tween"}
          onClick={() => setKind("tween")}
        >
          Tween
        </button>
        <button
          type="button"
          aria-pressed={kind === "spring"}
          onClick={() => setKind("spring")}
        >
          Spring
        </button>
      </div>

      <div className="transition-demo__track">
        <motion.div
          className="transition-demo__card"
          animate={{
            x: position === "start" ? 0 : 220,
            rotate: position === "start" ? 0 : 4,
          }}
          transition={transition}
        >
          {kind === "spring" ? "Spring" : "Tween"}
        </motion.div>
      </div>

      <button
        className="transition-demo__move"
        type="button"
        aria-pressed={position === "end"}
        onClick={() =>
          setPosition((current) => (current === "start" ? "end" : "start"))
        }
      >
        {position === "start" ? "Move card" : "Return card"}
      </button>
      <p className="transition-demo__status" aria-live="polite">
        Transition: {kind}. Card target: {position}.
      </p>
    </main>
  );
}
~~~

The `position` state selects destination values, while `kind` selects the transition object. Changing the transition selector alone does not move the card; press the move button to compare the next movement.

## `src/TransitionDemo.css`

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

.transition-demo {
  width: min(100% - 2rem, 760px);
  margin: 0 auto;
  padding: 4rem 0;
}

.transition-demo__eyebrow {
  color: #315f9c;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.transition-demo__controls {
  display: flex;
  gap: 0.75rem;
  margin: 1.25rem 0;
}

.transition-demo__controls button,
.transition-demo__move {
  min-height: 2.7rem;
  padding: 0.6rem 0.9rem;
  border: 0;
  border-radius: 0.55rem;
  color: white;
  background: #245da8;
  cursor: pointer;
}

.transition-demo button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.transition-demo__track {
  min-height: 110px;
  overflow: hidden;
  padding: 1rem;
  border: 1px dashed #9aaac1;
  border-radius: 0.8rem;
  background: #e7edf6;
}

.transition-demo__card {
  display: grid;
  width: 150px;
  min-height: 76px;
  place-items: center;
  border-radius: 0.7rem;
  color: white;
  background: #245da8;
  box-shadow: 0 10px 24px rgb(24 34 56 / 18%);
  font-weight: 700;
}

.transition-demo__move {
  margin-top: 1rem;
}

.transition-demo__status {
  color: #53627a;
}
~~~

## Run and compare

Render `<TransitionDemo />` and start the project's development server.

1. Press “Move card” with Tween selected. Motion uses the configured duration and easing.
2. Press “Return card”.
3. Select Spring, then move the card again. Observe how the spring settles.
4. Change only damping or stiffness and repeat the movement.
5. Tab through the buttons and confirm they remain usable without a pointer.

The expected result is one card moving between two positions inside a clipped track. The status text reports the transition type and current target.

## Common mistakes and practice

- Changing `kind` does not itself change `position`, so it does not start a new movement.
- A spring may overshoot or bounce depending on its configuration.
- A tween's fixed duration can make a long movement seem faster than a short one.
- Do not use both a fixed-timing model and spring assumptions without checking the installed version's transition options.

Practice by giving opacity a short tween while position uses the selected default transition. Then remove the override and compare whether the movement feels more unified.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="004-initial-and-animate-states.md">004 - Initial and animate states</a></td><td><a href="../lessons/005-transitions-and-springs.md">005 - Transitions and springs lesson</a></td><td><a href="../qnas/005-transitions-and-springs.md">005 - Transitions and springs Q&amp;A</a></td><td><a href="006-keyframes-and-repeats.md">006 - Keyframes and repeats example</a></td></tr>
  </tbody>
</table>

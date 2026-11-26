# Keyframes and repeats example

This example uses a finite keyframe sequence to animate a badge, then lets the user start or stop a repeating sequence. The badge can also reset to a static target.

## Prerequisites and setup

Use a React app with Motion installed:

~~~bash
npm install motion
~~~

## File structure

~~~text
src/
  KeyframesDemo.jsx
  KeyframesDemo.css
~~~

## `src/KeyframesDemo.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";
import "./KeyframesDemo.css";

export function KeyframesDemo() {
  const [looping, setLooping] = useState(false);
  const [runSequence, setRunSequence] = useState(0);

  const pulseTransition = looping
    ? {
        duration: 1.1,
        ease: "easeInOut",
        times: [0, 0.25, 0.7, 1],
        repeat: Infinity,
        repeatType: "reverse",
        repeatDelay: 0.25,
      }
    : {
        duration: 0.75,
        ease: "easeInOut",
        times: [0, 0.25, 0.7, 1],
      };

  return (
    <main className="keyframe-demo">
      <p className="keyframe-demo__eyebrow">Chapter 006</p>
      <h1>Keyframes and repeats</h1>
      <p>
        Start a finite keyframe sequence, or choose a repeating pulse and stop it
        when you are done.
      </p>

      <div className="keyframe-demo__stage">
        <motion.div
          key={runSequence}
          className="keyframe-demo__badge"
          initial={false}
          animate={
            looping
              ? { scale: [1, 1.12, 1.12, 1], rotate: [0, 0, -3, 0] }
              : runSequence > 0
                ? { scale: [1, 1.16, 0.94, 1], rotate: [0, 4, -4, 0] }
                : { scale: 1, rotate: 0 }
          }
          transition={pulseTransition}
        >
          READY
        </motion.div>
      </div>

      <div className="keyframe-demo__controls">
        <button type="button" onClick={() => setRunSequence((count) => count + 1)}>
          Play sequence
        </button>
        <button
          type="button"
          aria-pressed={looping}
          onClick={() => setLooping((value) => !value)}
        >
          {looping ? "Stop looping" : "Start loop"}
        </button>
        <button
          type="button"
          onClick={() => {
            setLooping(false);
            setRunSequence(0);
          }}
        >
          Reset
        </button>
      </div>

      <p className="keyframe-demo__status" aria-live="polite">
        {looping ? "Repeating while the loop is enabled." : "No repeating loop."}
      </p>
    </main>
  );
}
~~~

The finite sequence uses several values for scale and rotation. Its timing positions are uneven, so the badge reaches its larger size early and holds before returning. The loop uses `repeat: Infinity`, but the stop button changes the state and lets the badge return to a static target.

The `key` changes only when the finite sequence button is pressed. This intentionally remounts the badge to replay its initial-to-target sequence. The key is stable between ordinary renders; it is not generated randomly.

## `src/KeyframesDemo.css`

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

.keyframe-demo {
  width: min(100% - 2rem, 760px);
  margin: 0 auto;
  padding: 4rem 0;
}

.keyframe-demo__eyebrow {
  color: #315f9c;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.keyframe-demo__stage {
  display: grid;
  min-height: 220px;
  margin: 1.5rem 0;
  place-items: center;
  overflow: hidden;
  border: 1px dashed #9aaac1;
  border-radius: 1rem;
  background: #e7edf6;
}

.keyframe-demo__badge {
  display: grid;
  width: 120px;
  height: 120px;
  place-items: center;
  border-radius: 50%;
  color: white;
  background: #245da8;
  box-shadow: 0 12px 28px rgb(24 34 56 / 20%);
  font-weight: 800;
  letter-spacing: 0.08em;
}

.keyframe-demo__controls {
  display: flex;
  flex-wrap: wrap;
  gap: 0.7rem;
}

.keyframe-demo__controls button {
  min-height: 2.7rem;
  padding: 0.6rem 0.9rem;
  border: 0;
  border-radius: 0.55rem;
  color: white;
  background: #245da8;
  cursor: pointer;
}

.keyframe-demo button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.keyframe-demo__status {
  color: #53627a;
}
~~~

## Run and inspect

Render `<KeyframesDemo />` and start the app's normal development server.

1. Press “Play sequence” to replay the finite pulse.
2. Enable “Start loop” and observe the repeating motion.
3. Press “Stop looping” and confirm the badge returns to a static target.
4. Use “Reset” to clear both the repeat and finite sequence state.
5. Tab to each control and verify its focus indicator.

The expected result is a single badge that pulses through a few target values. The loop stops when the state changes. The status text explains whether a loop is active.

## Common mistakes and practice

- The repeat count and delay live in the transition object, not in the keyframe values.
- Infinite loops should have a stop or meaningful condition.
- The `times` list corresponds to the keyframe arrays. Keep their lengths aligned.
- A random key forces remounting on every render. This example changes a numeric key only when replaying.
- To use the current value as the start, replace the first keyframe with `null` and test interruption behavior.

Practice changing `repeatType` between `"loop"` and `"reverse"`. Explain how the direction changes and why an indefinite loop may need a reduced-motion alternative.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="005-transitions-and-springs.md">005 - Transitions and springs</a></td><td><a href="../lessons/006-keyframes-and-repeats.md">006 - Keyframes and repeats lesson</a></td><td><a href="../qnas/006-keyframes-and-repeats.md">006 - Keyframes and repeats Q&amp;A</a></td><td><a href="007-variants-and-reusable-states.md">007 - Variants and reusable states example</a></td></tr>
  </tbody>
</table>

# Motion values and direct updates example

This example moves a marker by changing a Motion value. The marker's transform updates directly through Motion. A Motion value subscription updates a textual readout through React state only when a step changes.

## Setup

Install Motion with `npm install motion`. Save as `src/MotionValueMarker.jsx` and render `<MotionValueMarker />`.

## `src/MotionValueMarker.jsx`

~~~jsx
import { useState } from "react";
import { motion, useMotionValue, useMotionValueEvent } from "motion/react";

const MIN_X = 0;
const MAX_X = 240;
const STEP = 40;

export function MotionValueMarker() {
  const x = useMotionValue(0);
  const [readout, setReadout] = useState(0);

  useMotionValueEvent(x, "change", (latest) => {
    setReadout(Math.round(latest));
  });

  function move(amount) {
    x.set(Math.max(MIN_X, Math.min(MAX_X, x.get() + amount)));
  }

  function reset() {
    x.jump(0);
  }

  return (
    <main style={{ maxWidth: 640, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Direct Motion value updates</h1>
      <p>The buttons set a visual value without putting it in React state.</p>
      <div
        style={{
          position: "relative",
          height: 100,
          margin: "24px 0",
          border: "1px solid #cbd5e1",
          borderRadius: 12,
          background: "#f8fafc",
        }}
      >
        <motion.div
          style={{
            x,
            position: "absolute",
            top: 20,
            left: 0,
            width: 54,
            height: 54,
            borderRadius: 12,
            background: "#2563a6",
          }}
        />
      </div>
      <div style={{ display: "flex", gap: 8 }}>
        <button type="button" onClick={() => move(-STEP)} disabled={readout <= MIN_X}>
          Move left
        </button>
        <button type="button" onClick={() => move(STEP)} disabled={readout >= MAX_X}>
          Move right
        </button>
        <button type="button" onClick={reset} disabled={readout === MIN_X}>
          Reset
        </button>
      </div>
      <p aria-live="polite">Horizontal position: {readout} pixels.</p>
    </main>
  );
}
~~~

## Walkthrough and expected result

The x Motion value is created once by the hook and passed to the marker's style. Each button reads the current number, clamps the next value to the range, and calls set. Motion updates the transform directly. useMotionValueEvent listens for changes and copies the rounded position into React state so the text and disabled buttons can update.

The textual readout causes a React render when the button changes the value. This does not mean the marker position is controlled by that React state; the marker's x style is controlled by the Motion value. In high-frequency interactions, avoid copying every frame into React state unless the interface needs that information.

Reset uses jump so it moves immediately and clears velocity continuity. Replace jump with an animation target if the reset should glide to the origin.

## Practice and common mistakes

- Change STEP and the maximum to observe clamping.
- Replace jump with a spring-backed value in the next chapter.
- Add a Motion value change listener only for values that need observation.
- Do not expect get() by itself to update JSX.
- Keep button bounds and the displayed range consistent.
- Use React state for a persistent application choice, not merely because a visual value needs to move.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups example</a></td><td><a href="../lessons/015-motion-values-and-direct-updates.md">015 - Motion values and direct updates lesson</a></td><td><a href="../qnas/015-motion-values-and-direct-updates.md">015 - Motion values and direct updates Q&amp;A</a></td><td><a href="016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values example</a></td></tr></tbody>
</table>
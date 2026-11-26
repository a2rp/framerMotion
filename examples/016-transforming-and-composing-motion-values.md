# Transforming and composing motion values example

This demo maps a horizontal range input to position, opacity, rotation, and color. The source value updates immediately; position alone is spring-smoothed to show how outputs can compose differently.

## Setup

Install Motion with `npm install motion`. Save as `src/ComposedMotion.jsx` and render `<ComposedMotion />`.

## `src/ComposedMotion.jsx`

~~~jsx
import { useState } from "react";
import { motion, useMotionValue, useSpring, useTransform } from "motion/react";

const MIN = -160;
const MAX = 160;

export function ComposedMotion() {
  const [slider, setSlider] = useState(0);
  const targetX = useMotionValue(0);
  const smoothX = useSpring(targetX, { stiffness: 320, damping: 32 });
  const opacity = useTransform(targetX, [MIN, 0, MAX], [0.3, 1, 0.3]);
  const rotate = useTransform(targetX, [MIN, MAX], [-12, 12]);
  const backgroundColor = useTransform(
    targetX,
    [MIN, 0, MAX],
    ["#8b5cf6", "#2563a6", "#e11d48"]
  );

  function update(event) {
    const next = Number(event.target.value);
    setSlider(next);
    targetX.set(next);
  }

  return (
    <main style={{ maxWidth: 680, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Composed Motion values</h1>
      <p>Move the slider to update several derived styles from one source value.</p>
      <div
        style={{
          display: "grid",
          placeItems: "center",
          height: 180,
          overflow: "hidden",
          border: "1px solid #cbd5e1",
          borderRadius: 12,
          background: "#f8fafc",
        }}
      >
        <motion.div
          style={{
            x: smoothX,
            opacity,
            rotate,
            backgroundColor,
            width: 110,
            height: 80,
            borderRadius: 16,
          }}
        />
      </div>
      <label style={{ display: "grid", gap: 8, marginTop: 20 }}>
        Horizontal target: {slider}px
        <input
          type="range"
          min={MIN}
          max={MAX}
          step="1"
          value={slider}
          onChange={update}
          aria-label="Horizontal target position"
        />
      </label>
      <p>The marker follows the position with a spring. Opacity, rotation, and color map directly from the target.</p>
    </main>
  );
}
~~~

## Walkthrough and expected result

The range input is the accessible user control. React stores its displayed numeric value, and the handler also sets the target Motion value. useTransform maps that source to opacity, rotation, and color. useSpring follows the same target for the x position, adding a gentle lag only to translation.

At the center, the marker is blue, fully opaque, and unrotated. Moving left makes it purple and less opaque; moving right makes it red and less opaque. The position follows the slider target with spring motion while the other mapped styles respond directly.

## Practice and common mistakes

- Change the input stops and outputs together; keep their lengths equal.
- Set clamp false and move beyond the range in a programmatic source to see the output continue.
- Remove useSpring and compare direct position with smoothed position.
- Keep a keyboard-operable input for the interaction.
- Avoid copying every animated frame into React state.
- Use compatible colors, units, and string structure for complex outputs.
- Do not add side effects to a transform function.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="015-motion-values-and-direct-updates.md">015 - Motion values and direct updates example</a></td><td><a href="../lessons/016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values lesson</a></td><td><a href="../qnas/016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values Q&amp;A</a></td><td><a href="017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll example</a></td></tr></tbody>
</table>
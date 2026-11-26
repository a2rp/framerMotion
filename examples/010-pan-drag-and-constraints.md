# Pan, drag, and constraints example

This example confines a tile to a measured panel. Pointer dragging and keyboard nudge buttons both move the same tile state, so there is a working non-pointer alternative.

## Setup

Install Motion with `npm install motion`. Save as `src/ConstrainedTile.jsx` and render `<ConstrainedTile />`.

## `src/ConstrainedTile.jsx`

~~~jsx
import { useRef, useState } from "react";
import { motion } from "motion/react";

const LIMIT = 80;

function clamp(value) {
  return Math.max(-LIMIT, Math.min(LIMIT, value));
}

export function ConstrainedTile() {
  const bounds = useRef(null);
  const dragStart = useRef(0);
  const [x, setX] = useState(0);

  function nudge(amount) {
    setX((current) => clamp(current + amount));
  }

  function finishDrag(event, info) {
    setX(clamp(dragStart.current + info.offset.x));
  }

  return (
    <main style={{ maxWidth: 620, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Constrained drag</h1>
      <p>Drag the tile or use the buttons to move it horizontally.</p>
      <div
        ref={bounds}
        style={{
          position: "relative",
          display: "grid",
          placeItems: "center",
          height: 220,
          overflow: "hidden",
          border: "2px dashed #64748b",
          borderRadius: 14,
          background: "#f1f5f9",
        }}
      >
        <motion.div
          drag="x"
          dragConstraints={bounds}
          dragElastic={0.08}
          dragMomentum={false}
          animate={{ x }}
          onDragStart={() => {
            dragStart.current = x;
          }}
          onDragEnd={finishDrag}
          whileDrag={{ scale: 1.04 }}
          style={{
            width: 110,
            height: 80,
            display: "grid",
            placeItems: "center",
            borderRadius: 12,
            color: "white",
            background: "#2563a6",
            fontWeight: 700,
            cursor: "grab",
            touchAction: "none",
            userSelect: "none",
          }}
        >
          Move me
        </motion.div>
      </div>
      <div style={{ display: "flex", gap: 8, marginTop: 14 }}>
        <button type="button" onClick={() => nudge(-20)} disabled={x <= -LIMIT}>
          Move left
        </button>
        <button type="button" onClick={() => nudge(20)} disabled={x >= LIMIT}>
          Move right
        </button>
        <button type="button" onClick={() => setX(0)} disabled={x === 0}>
          Reset
        </button>
      </div>
      <p aria-live="polite">Horizontal offset: {x} pixels.</p>
    </main>
  );
}
~~~

## How it works

- The container ref is passed to `dragConstraints`. Motion measures its bounds and keeps the tile inside.
- The tile uses `drag="x"`, so pointer movement is horizontal only.
- `dragElastic={0.08}` allows a small stretch at the boundary; `dragMomentum={false}` stops release velocity from carrying it onward.
- React state stores the committed horizontal offset. The keyboard-operable buttons update that value, and the Motion target animates to it.
- At drag start, the example records the current offset. At drag end, it combines that starting position with the gesture offset and clamps the result to the same range used by the buttons.
- Touch behavior is limited to the tile. The surrounding page remains normally scrollable.

## Expected result and accessibility checks

Drag the tile horizontally inside the panel. The Move left and Move right buttons move the same tile in 20-pixel steps, and Reset returns it to the center. The buttons disable at the allowed ends. The live text reports the committed offset after a button action or completed drag.

Tab to each button and operate it with Enter or Space. Try pointer dragging on a touch device and verify the surrounding page can still scroll outside the tile.

## Practice and common mistakes

- Change `drag="x"` to `drag` and compare horizontal-only movement with two-axis movement.
- Set elasticity to zero and compare the hard boundary.
- Enable momentum and observe why the final offset may need to be read after inertia settles.
- Keep the drag ref on the containing region and ensure its dimensions respond to the viewport.
- Keep keyboard controls connected to the same position state as pointer movement.
- Use native drag-and-drop instead when the task transfers files or data.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures example</a></td><td><a href="../lessons/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints lesson</a></td><td><a href="../qnas/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints Q&amp;A</a></td><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations example</a></td></tr></tbody>
</table>
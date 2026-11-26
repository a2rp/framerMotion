# Pan, drag, and constraints example

This example confines a tile to a measured panel. It disables momentum to make the stopping point predictable and includes keyboard nudge buttons as an alternative to pointer dragging.

## Setup

Install Motion with §npm install motion§. Save this component as §src/ConstrainedTile.jsx§ and render §<ConstrainedTile />§.

## §src/ConstrainedTile.jsx§

~~~jsx
import { useRef, useState } from "react";
import { motion } from "motion/react";

export function ConstrainedTile() {
  const bounds = useRef(null);
  const [announcement, setAnnouncement] = useState("Tile is ready to move.");

  function nudge(direction) {
    setAnnouncement(direction === "left" ? "Move the tile left with the pointer." : "Move the tile right with the pointer.");
  }

  return (
    <main style={{ maxWidth: 620, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Constrained drag</h1>
      <p>Drag the tile inside the panel. Use the buttons as a keyboard alternative.</p>
      <div
        ref={bounds}
        style={{
          position: "relative",
          display: "grid",
          placeItems: "center",
          height: 240,
          border: "2px dashed #64748b",
          borderRadius: 14,
          background: "#f1f5f9",
          touchAction: "pan-y",
        }}
      >
        <motion.div
          drag
          dragConstraints={bounds}
          dragElastic={0.12}
          dragMomentum={false}
          whileDrag={{ scale: 1.06, cursor: "grabbing" }}
          onDragEnd={() => setAnnouncement("Tile movement ended.")}
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
        <button type="button" onClick={() => nudge("left")}>Move left</button>
        <button type="button" onClick={() => nudge("right")}>Move right</button>
      </div>
      <p aria-live="polite">{announcement}</p>
    </main>
  );
}
~~~

## How it works

- The container ref is passed to §dragConstraints§. Motion measures its bounding box and limits the tile to that region.
- §dragElastic={0.12}§ allows only a small stretch past the edge.
- §dragMomentum={false}§ prevents release velocity from carrying the tile onward.
- The tile is draggable on both axes. Change §drag§ to §drag="x"§ to limit it horizontally.
- §touchAction§ is limited to the gesture surface and tile. Test touch scrolling on a real narrow viewport.
- The buttons are native keyboard-operable controls and communicate status through a polite live region. In a production interface, connect these controls to actual position changes; this simple demo emphasizes providing an equivalent non-drag operation.

## Expected result and practice

The tile moves inside the dashed panel and stops when released. It can stretch slightly past an edge and springs back. Try increasing elasticity, enabling momentum, or changing the panel height. Compare the feel without changing the constraint at the same time.

For a production keyboard alternative, store a normalized position in React state, update it through arrow-key buttons or a range control, and apply that value as the Motion target. Ensure pointer drag and keyboard updates use one coherent position model.

## Common mistakes

- A constraint ref is attached to the tile instead of the bounding panel.
- The tile has no measurable dimensions when Motion reads bounds.
- Touch-action is disabled on the entire page instead of only the drag surface.
- The example's status message is mistaken for moving the tile with the buttons; connect the controls to an explicit position model when using the pattern in an application.
- Drag is used for a data-transfer task that requires native drop events.
- Pointer dragging is the only way to perform a meaningful action.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures example</a></td><td><a href="../lessons/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints lesson</a></td><td><a href="../qnas/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints Q&amp;A</a></td><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations example</a></td></tr></tbody>
</table>
# Hover, tap, and focus gestures example

This example adds hover, press, and focus-visible feedback to a native button. React controls the persistent saved status. Motion controls only temporary presentation.

## Setup

Install Motion in the React application with `npm install motion`. Save this component as `src/GestureButton.jsx` and render `<GestureButton />`.

## `src/GestureButton.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

export function GestureButton() {
  const [saved, setSaved] = useState(false);

  return (
    <main style={{ maxWidth: 520, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Gesture feedback</h1>
      <p>Try pointer, touch, and keyboard input.</p>
      <motion.button
        type="button"
        onClick={() => setSaved(true)}
        whileHover={{ y: -2, scale: 1.02 }}
        whileTap={{ scale: 0.97 }}
        whileFocus={{ outlineColor: "#b45309", outlineWidth: 4, outlineOffset: 3 }}
        transition={{ type: "spring", stiffness: 420, damping: 26 }}
        style={{
          padding: "0.8rem 1.2rem",
          border: 0,
          borderRadius: 8,
          color: "white",
          background: "#2366ad",
          cursor: "pointer",
          outlineStyle: "solid",
          outlineColor: "transparent",
          outlineWidth: 0,
        }}
      >
        {saved ? "Saved" : "Save changes"}
      </motion.button>
      <p aria-live="polite">{saved ? "Your changes were saved." : "No changes saved yet."}</p>
    </main>
  );
}
~~~

## Walkthrough

The native button provides role, focus, and standard activation. `whileHover` gives subtle pointer feedback; `whileTap` shrinks the control during the press; `whileFocus` makes keyboard focus prominent. `onClick` updates saved state once. The status text makes the result visible and announces changes politely.

## Expected result and checks

Hover lifts the button slightly. Pressing it scales down temporarily. Keyboard navigation shows a high-contrast outline. Activating with Enter or Space changes the label and status to Saved.

1. Tab to the button and confirm the outline is visible.
2. Activate it with Enter and Space and confirm the action is not duplicated.
3. Try touch input; the action must not depend on hover.
4. Disable animation and confirm the label and status still explain the outcome.
5. Consider what should happen if this action can be undone or saved again.

## Variations and common mistakes

- Add a native `disabled` attribute for a truly disabled control; opacity alone does not disable it.
- Keep a clear focus cue even if its color animation does not run.
- Do not call the save action from both `onClick` and `onTap`.
- Keep scale changes small to avoid obscuring nearby content.
- Do not hide a tooltip or action so it is reachable only on hover.
- If saved status is reversible, model that state explicitly instead of keeping a one-way animation.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger example</a></td><td><a href="../lessons/009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures lesson</a></td><td><a href="../qnas/009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures Q&amp;A</a></td><td><a href="010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints example</a></td></tr></tbody>
</table>
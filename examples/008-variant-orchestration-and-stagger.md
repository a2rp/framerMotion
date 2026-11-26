# Variant orchestration and stagger example

This interactive list demonstrates a center-origin stagger. The parent selects the group state and schedules child motion. React also lets you change the list order to compare the origin with the rendered sequence.

## Setup

Install Motion in a React app with `npm install motion`. Save this as `src/StaggeredList.jsx` and render `<StaggeredList />`.

## `src/StaggeredList.jsx`

~~~jsx
import { useState } from "react";
import { motion, stagger } from "motion/react";

const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { delayChildren: stagger(0.08, { from: "center" }) },
  },
};
const itemVariants = {
  hidden: { opacity: 0, y: 10 },
  visible: { opacity: 1, y: 0 },
};
const topics = ["Parent state", "Child labels", "Start delay", "Stagger interval", "Origin", "Timing"];

export function StaggeredList() {
  const [shown, setShown] = useState(true);
  const [reverse, setReverse] = useState(false);
  const visibleTopics = reverse ? [...topics].reverse() : topics;

  return (
    <main style={{ maxWidth: 520, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Variant orchestration</h1>
      <p>Reveal a group from its center, then change its rendered order.</p>
      <button type="button" onClick={() => setShown((value) => !value)}>
        {shown ? "Hide list" : "Reveal list"}
      </button>{" "}
      <button type="button" onClick={() => setReverse((value) => !value)}>
        Reverse order
      </button>
      <motion.ul
        variants={containerVariants}
        initial={false}
        animate={shown ? "visible" : "hidden"}
        style={{ display: "grid", gap: 10, padding: 0, listStyle: "none" }}
      >
        {visibleTopics.map((topic) => (
          <motion.li key={topic} variants={itemVariants}
            style={{ padding: 14, border: "1px solid #cbd5e1", borderRadius: 10 }}>
            {topic}
          </motion.li>
        ))}
      </motion.ul>
    </main>
  );
}
~~~

## Walkthrough and expected result

The parent and children share `hidden` and `visible` labels. The parent transition calls `stagger(0.08, { from: "center" })`, so children begin at different offsets radiating from the middle. Children contain no separate delay. React chooses the group label and can reorder the array.

`initial={false}` starts directly at the visible target. Click Hide list, then Reveal list to trigger the sequence. Reverse order changes the actual item order, not only the stagger origin. Stable string keys help React track items during reordering.

The center items begin first and the items further away follow. Hiding selects hidden targets. The list contains plain text, so its hidden items are not keyboard controls.

## Practice and common mistakes

- Change the origin to `"first"` and compare.
- Add a short group delay before the stagger.
- Add `when: "beforeChildren"` and observe the parent-first sequence.
- Remove the stagger while considering reduced-motion behavior.
- A child needs to be a Motion component and use the active label.
- Large intervals make long lists slow.
- Reversing data changes DOM reading order; do that only if intended.
- Opacity alone does not remove interactive children from keyboard navigation. Use a visibility or presence strategy for hidden controls.
- Animation timing does not replace React state or async request handling.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="007-variants-and-reusable-states.md">007 - Variants and reusable states example</a></td><td><a href="../lessons/008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger lesson</a></td><td><a href="../qnas/008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger Q&amp;A</a></td><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures example</a></td></tr></tbody>
</table>
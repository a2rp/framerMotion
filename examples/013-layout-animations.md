# Layout animations example

This example filters a card grid. React changes which cards render; Motion's layout prop animates the remaining cards into their new positions.

## Setup

Install Motion with `npm install motion`. Save as `src/AnimatedFilterGrid.jsx` and render `<AnimatedFilterGrid />`.

## `src/AnimatedFilterGrid.jsx`

~~~jsx
import { useState } from "react";
import { AnimatePresence, motion } from "motion/react";

const cards = [
  { id: "motion", group: "Animation", title: "Motion values" },
  { id: "layout", group: "Animation", title: "Layout transitions" },
  { id: "react", group: "React", title: "React state" },
  { id: "variants", group: "Animation", title: "Reusable variants" },
];

const filters = ["All", "Animation", "React"];

export function AnimatedFilterGrid() {
  const [filter, setFilter] = useState("All");
  const visibleCards = filter === "All" ? cards : cards.filter((card) => card.group === filter);

  return (
    <main style={{ maxWidth: 760, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Layout animation</h1>
      <fieldset style={{ display: "flex", gap: 12, border: 0, padding: 0, marginBottom: 18 }}>
        <legend>Filter cards</legend>
        {filters.map((name) => (
          <label key={name}>
            <input
              type="radio"
              name="filter"
              value={name}
              checked={filter === name}
              onChange={() => setFilter(name)}
            />
            {" "}{name}
          </label>
        ))}
      </fieldset>
      <motion.ul
        layout
        style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(190px, 1fr))", gap: 12, padding: 0, listStyle: "none" }}
      >
        <AnimatePresence initial={false}>
          {visibleCards.map((card) => (
            <motion.li
              layout
              key={card.id}
              initial={{ opacity: 0, scale: 0.96 }}
              animate={{ opacity: 1, scale: 1 }}
              exit={{ opacity: 0, scale: 0.96 }}
              transition={{ duration: 0.2, layout: { type: "spring", stiffness: 420, damping: 36 } }}
              style={{ padding: 18, borderRadius: 12, background: "#eaf1fa", boxShadow: "0 2px 8px #0002" }}
            >
              <small>{card.group}</small>
              <h2>{card.title}</h2>
            </motion.li>
          ))}
        </AnimatePresence>
      </motion.ul>
    </main>
  );
}
~~~

## Walkthrough

The radio group changes the filter in React state. The list and cards use layout so the remaining elements animate to their new grid positions. AnimatePresence handles filtered cards leaving and new cards entering. Each record has a stable ID key.

The item uses an opacity and scale entry/exit target. Its layout transition has separate spring timing. The example does not animate grid columns directly; CSS grid chooses the new positions, and Motion animates the measured geometry between renders.

## Expected result and practice

Select Animation or React. Cards that remain move into their new positions, and removed cards fade out. Select All to restore the complete grid.

- Remove layout from the list items and compare the abrupt reflow.
- Use `layout="position"` on cards to avoid scaling their contents.
- Change the grid minimum width and test responsive wrapping.
- Add layout to a nested text block only if that content needs scale correction.

## Common mistakes

- Use stable record IDs rather than filter indices.
- Keep the list re-rendering when the selected filter changes.
- Do not change the same geometry through both layout and a competing animate target.
- Layout does not automatically remove a filtered card; AnimatePresence supplies its exit.
- Check shadows, borders, and text wrapping while cards resize.
- The radio controls are semantic and remain usable without animation.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data example</a></td><td><a href="../lessons/013-layout-animations.md">013 - Layout animations lesson</a></td><td><a href="../qnas/013-layout-animations.md">013 - Layout animations Q&amp;A</a></td><td><a href="014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups example</a></td></tr></tbody>
</table>
# AnimatePresence and exit animations example

This example removes notices from React state and animates each keyed item out before it leaves the DOM.

## Setup

Install Motion with `npm install motion`. Save as `src/DismissibleNotices.jsx` and render `<DismissibleNotices />`.

## `src/DismissibleNotices.jsx`

~~~jsx
import { useState } from "react";
import { AnimatePresence, motion } from "motion/react";

const startingNotices = [
  { id: "profile", text: "Profile details are ready to review." },
  { id: "security", text: "Security settings were updated." },
  { id: "tips", text: "You can manage alerts in preferences." },
];

export function DismissibleNotices() {
  const [notices, setNotices] = useState(startingNotices);

  function dismiss(id) {
    setNotices((current) => current.filter((notice) => notice.id !== id));
  }

  return (
    <main style={{ maxWidth: 640, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Dismissible notices</h1>
      <p>Remove a notice to see its exit animation.</p>
      <ul style={{ display: "grid", gap: 12, padding: 0, listStyle: "none" }}>
        <AnimatePresence initial={false}>
          {notices.map((notice) => (
            <motion.li
              key={notice.id}
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, x: 18, height: 0, marginBottom: -12 }}
              transition={{ duration: 0.22, ease: "easeInOut" }}
              style={{
                display: "flex",
                alignItems: "center",
                justifyContent: "space-between",
                gap: 16,
                overflow: "hidden",
                padding: 16,
                border: "1px solid #cbd5e1",
                borderRadius: 10,
                background: "#f8fafc",
              }}
            >
              <span>{notice.text}</span>
              <button type="button" onClick={() => dismiss(notice.id)}>Dismiss</button>
            </motion.li>
          ))}
        </AnimatePresence>
      </ul>
      <p aria-live="polite">{notices.length} notices remaining.</p>
    </main>
  );
}
~~~

## How it works

React state stores the notices. AnimatePresence remains in the tree even when the array becomes empty. Each direct list item uses its stable record ID. The initial and animate targets control entry; exit applies after React filters an item out. Setting initial false suppresses entry animation for items already present at first render. The live status reports the remaining count.

## Expected result and checks

Click Dismiss. That notice fades, shifts, and collapses before disappearing. Other items remain associated with their own IDs. Remove every item and confirm the manager stays mounted after the list becomes empty.

For an interactive production alert, decide where focus moves after a focused dismiss button disappears. Announce important changes and keep a sensible focus destination.

## Practice and common mistakes

- Add a restore button that recreates the initial records with the same stable IDs.
- Change initial false and compare first-render behavior.
- Temporarily use indices as keys, then reorder or delete items.
- Do not conditionally unmount AnimatePresence itself.
- Keep exits short so dismissal remains responsive.
- Do not generate random keys on each render.
- Presence animation does not set focus or announce a state change on its own.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints example</a></td><td><a href="../lessons/011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations lesson</a></td><td><a href="../qnas/011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations Q&amp;A</a></td><td><a href="012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data example</a></td></tr></tbody>
</table>
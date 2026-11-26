# Shared layout and layout groups example

This example renders two independent rows of filter buttons. Within each row, a shared underline moves between the selected button. Each row has its own LayoutGroup namespace so the indicators never connect to each other.

## Setup

Install Motion with `npm install motion`. Save as `src/SharedIndicators.jsx` and render `<SharedIndicators />`.

## `src/SharedIndicators.jsx`

~~~jsx
import { useState } from "react";
import { LayoutGroup, motion } from "motion/react";

const choices = ["Overview", "Activity", "Settings"];

function ChoiceRow({ groupId, label }) {
  const [selected, setSelected] = useState(choices[0]);

  return (
    <section style={{ marginBottom: 28 }}>
      <h2>{label}</h2>
      <LayoutGroup id={groupId}>
        <div style={{ display: "flex", gap: 8, borderBottom: "1px solid #cbd5e1" }}>
          {choices.map((choice) => (
            <button
              key={choice}
              type="button"
              aria-pressed={selected === choice}
              onClick={() => setSelected(choice)}
              style={{
                position: "relative",
                padding: "10px 14px",
                border: 0,
                background: "transparent",
                color: selected === choice ? "#174a83" : "#334155",
                font: "inherit",
                cursor: "pointer",
              }}
            >
              {choice}
              {selected === choice && (
                <motion.span
                  layoutId="active-indicator"
                  aria-hidden="true"
                  style={{
                    position: "absolute",
                    right: 8,
                    bottom: -1,
                    left: 8,
                    height: 3,
                    borderRadius: 3,
                    background: "#2563a6",
                  }}
                  transition={{ type: "spring", stiffness: 420, damping: 34 }}
                />
              )}
            </button>
          ))}
        </div>
      </LayoutGroup>
      <p aria-live="polite">Selected: {selected}</p>
    </section>
  );
}

export function SharedIndicators() {
  return (
    <main style={{ maxWidth: 720, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Shared layout identity</h1>
      <ChoiceRow groupId="account-filters" label="Account" />
      <ChoiceRow groupId="report-filters" label="Reports" />
    </main>
  );
}
~~~

## Walkthrough

Each row owns its selected choice in React state. The selected button conditionally renders a Motion span with the same layoutId. When selection changes, the old span leaves and a new one enters, so Motion moves the underline between positions.

The two rows reuse the same layoutId string but wrap it in distinct LayoutGroup ids. This namespaces each row and prevents the account indicator from animating to the report row. The buttons expose their selected state with aria-pressed; the underline is decorative and hidden from assistive technology.

## Expected result and practice

Select different choices in either row. Its underline animates to the new button. The other row remains unchanged. Operate all choices with the keyboard and confirm the pressed state and live selection text remain clear.

- Remove one LayoutGroup id and observe the shared identity collision risk.
- Add another row with a unique stable group ID.
- Replace the underline with a selected-card highlight.
- Add reduced-motion handling if the shared movement is too strong for your interface.

## Common mistakes

- Mismatched layoutId strings prevent the shared transition.
- Reusing an un-namespaced ID can connect unrelated components elsewhere on the page.
- LayoutGroup alone does not opt all children into layout animation.
- Use stable group IDs rather than values generated during render.
- The shared visual span is decorative; put accessible state on the actual buttons.
- Keep selection state in React rather than trying to infer it from the underline position.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="013-layout-animations.md">013 - Layout animations example</a></td><td><a href="../lessons/014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups lesson</a></td><td><a href="../qnas/014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups Q&amp;A</a></td><td><a href="015-motion-values-and-direct-updates.md">015 - Motion values and direct updates example</a></td></tr></tbody>
</table>
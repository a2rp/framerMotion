# Variants and reusable states example

This example builds an accessible disclosure card. React state determines whether the details are open. Motion variants define the visual targets for the panel and its child text, so one label coordinates the animation.

## Prerequisites and setup

Use an existing React app with Motion installed:

~~~bash
npm install motion
~~~

Create `src/VariantDisclosure.jsx` and render `<VariantDisclosure />` from an application component. The example uses current Motion for React imports from `motion/react`.

## `src/VariantDisclosure.jsx`

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

const panelVariants = {
  closed: {
    height: 0,
    opacity: 0,
    transition: { duration: 0.2, when: "afterChildren" },
  },
  open: {
    height: "auto",
    opacity: 1,
    transition: { duration: 0.28, when: "beforeChildren", staggerChildren: 0.06 },
  },
};

const detailVariants = {
  closed: { opacity: 0, y: 6 },
  open: { opacity: 1, y: 0 },
};

export function VariantDisclosure() {
  const [open, setOpen] = useState(false);
  const state = open ? "open" : "closed";

  return (
    <main style={{ maxWidth: 620, margin: "3rem auto", padding: "1rem", fontFamily: "system-ui" }}>
      <h1>Motion variants</h1>
      <section style={{ border: "1px solid #cbd5e1", borderRadius: 12, padding: 20 }}>
        <h2 style={{ marginTop: 0 }}>Reusable disclosure</h2>
        <button
          type="button"
          aria-expanded={open}
          aria-controls="variant-details"
          onClick={() => setOpen((value) => !value)}
        >
          {open ? "Hide details" : "Show details"}
        </button>

        <motion.div
          id="variant-details"
          variants={panelVariants}
          initial={false}
          animate={state}
          style={{ overflow: "hidden" }}
        >
          <motion.div variants={detailVariants} style={{ paddingTop: 16 }}>
            <p>React owns the open state; Motion animates the selected targets.</p>
          </motion.div>
          <motion.p variants={detailVariants}>
            The parent and detail text share the same named states.
          </motion.p>
        </motion.div>
      </section>
      <p aria-live="polite">Details are {open ? "open" : "closed"}.</p>
    </main>
  );
}
~~~

## How it works

1. The button changes a boolean React state. That same value controls its label, the `aria-expanded` attribute, the live status text, and the variant label.
2. `panelVariants` contains the panel's `closed` and `open` animation targets. `height: "auto"` lets the panel expand to its content height.
3. The two child motion elements declare matching labels in `detailVariants`. They follow the parent's selected label and animate their own opacity and vertical offset.
4. `initial={false}` avoids an unsolicited opening animation on initial render. Later state changes still animate.
5. `overflow: "hidden"` clips the child content while the panel height changes.

The transition's `when` option controls whether the parent target runs before or after child targets. The short stagger makes the text enter in sequence. The following chapter studies orchestration choices in more detail.

## Expected result and accessibility checks

The panel starts closed. Activating the native button expands it, reveals the details, and changes the button label. Activating it again closes the panel. Verify the button works with Enter and Space, and that `aria-expanded` matches what is visible.

This compact example keeps non-interactive text in the animated panel. If you add inputs or links, make sure closed content cannot receive keyboard focus. Opacity alone does not remove descendants from tab order; use an appropriate presence or visibility strategy for interactive content.

## Variations and practice

- Add a third child and observe how the `staggerChildren` transition coordinates it.
- Change the open label to `expanded` in both variant objects and at the state selection site. A mismatch is a useful debugging exercise.
- Add a hover target to the card using a separate gesture variant. Keep the disclosure state and the hover state conceptually distinct.
- Try `initial={false}` versus `initial="closed"` and compare first-paint behavior.

## Common mistakes

- Giving the parent `open` while the children only define `visible`.
- Using visual opacity as the only way to disable hidden interactive content.
- Updating the animation label but forgetting `aria-expanded` or the visible button label.
- Repeating variant objects inside each render when the targets are static.
- Expecting a variant to change application data without changing React state.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="006-keyframes-and-repeats.md">006 - Keyframes and repeats example</a></td><td><a href="../lessons/007-variants-and-reusable-states.md">007 - Variants and reusable states lesson</a></td><td><a href="../qnas/007-variants-and-reusable-states.md">007 - Variants and reusable states Q&amp;A</a></td><td><a href="008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger example</a></td></tr>
  </tbody>
</table>
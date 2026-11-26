# Motion and React animation fundamentals

Motion for React, formerly known as Framer Motion, adds animation and interaction APIs to React elements. It helps an interface move between meaningful visual states when data or user interaction changes. The current package is `motion` and the React entry point is `motion/react`. This course uses those names; older projects may use the `framer-motion` package.

Motion is an animation system, not an alternative to React. React still owns application state, renders the interface, and decides which elements exist. Motion takes animation targets and smoothly changes supported visual values over time.

## What an animation describes

An animation needs at least two things:

1. A value to change, such as opacity, position, scale, or color.
2. A target state or sequence of target values.

For a small notice, the visual state might be described as:

~~~js
{ opacity: 0, y: 12 } // hidden and slightly lower
{ opacity: 1, y: 0 }  // visible at its normal position
~~~

The object keys are properties Motion understands. Here `opacity` controls transparency and `y` is an independent vertical transform in pixels. Motion can also animate CSS properties, supported CSS custom properties, SVG attributes, and values through other APIs. The exact values that can interpolate depend on the property and Motion version.

## React state chooses the target

A component should store meaningful interface state in React. For example, whether a panel is selected is application state. Motion can use that state to choose a target object:

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

export function SelectionCard() {
  const [selected, setSelected] = useState(false);

  return (
    <section>
      <button
        type="button"
        aria-pressed={selected}
        onClick={() => setSelected((value) => !value)}
      >
        {selected ? "Unselect card" : "Select card"}
      </button>

      <motion.article
        aria-label="A sample card"
        animate={
          selected
            ? { opacity: 1, y: 0, scale: 1 }
            : { opacity: 0.8, y: 8, scale: 0.98 }
        }
        transition={{ duration: 0.25, ease: "easeOut" }}
      >
        This card follows the selected state.
      </motion.article>
    </section>
  );
}
~~~

When the button changes React state, React renders the new target. Motion interpolates from the current animated value to the new target. The animation does not require a React state update for every frame. This distinction is useful: use React state for decisions and content, and use Motion's animation system for continuous visual changes.

The button remains a native button. The `aria-pressed` state communicates its toggle behavior to assistive technology. An animation should support the interaction, not replace semantic HTML or accessible state.

## Declarative targets and interaction

A motion component is a React component that renders an HTML or SVG element and accepts Motion props. For example, `motion.article` renders an article element and can accept `animate` or `whileHover`.

~~~jsx
<motion.button
  type="button"
  initial={{ opacity: 0, y: 8 }}
  animate={{ opacity: 1, y: 0 }}
  whileHover={{ scale: 1.03 }}
  whileTap={{ scale: 0.98 }}
  transition={{ duration: 0.2 }}
>
  Open details
</motion.button>
~~~

`initial` defines the starting target. `animate` defines the target to reach. `whileHover` and `whileTap` add temporary interaction targets. `transition` controls how the values move between targets. The [motion components chapter](003-motion-components-and-props.md) explains the component API, and [the initial and animate chapter](004-initial-and-animate-states.md) explains state changes in detail.

A motion component behaves like the underlying HTML or SVG element. It still needs appropriate content, labels, and keyboard behavior. Use a button for an action, a link for navigation, and a heading for a heading. Avoid adding a click handler to a generic element and relying on animation to imply that it is interactive.

## Declarative and imperative approaches

Most component interactions are easier to read declaratively. The target belongs near the component state:

~~~jsx
<motion.div animate={{ opacity: open ? 1 : 0 }} />
~~~

Use imperative animation only when an event needs a sequence, scoped element selection, or explicit playback control. Motion's `useAnimate` supports that case and is covered later in the course. Imperative code should not replace React state when the animation also changes whether content exists or what the interface means.

## What Motion is useful for

Motion is useful when an interface needs:

- A clear transition between React states.
- Coordinated enter and exit animations.
- Pointer or keyboard feedback.
- A visual response to a layout change.
- Values that follow scroll or pointer input.
- Reusable animation states shared by related components.

A CSS transition may be enough for one simple hover color change. Motion is a better fit when targets depend on React state, several children need coordination, an element needs an exit animation before removal, or a value needs to be derived from another moving value. Keep the simplest system that solves the interface need.

## Common mistakes

- **Putting every animation value in React state.** React state should describe the interface. Motion values are designed for rapidly changing visual values without a render for each frame.
- **Using animation instead of semantic state.** A fading element can remain focusable or exposed to screen readers. Update semantics and interaction behavior too.
- **Animating an element that should be removed immediately.** Conditional removal unmounts it. Use `AnimatePresence` around the child when an exit animation is needed.
- **Using unstable list keys.** React may treat a changed item as a different element and disrupt its animation. Use stable identifiers.
- **Making important content visible only after a long animation.** Keep the page useful if animation is delayed, interrupted, disabled, or reduced.
- **Assuming every CSS property interpolates the same way.** Test the actual start and end values and choose a transition suited to them.
- **Adding Motion before understanding the state change.** First decide what the UI state is, then map it to targets.

## Practical guidance

Start with one meaningful state change and one or two properties. Prefer opacity and transforms for small feedback when they communicate the change. Keep duration short enough to preserve the pace of the interface. Do not animate every element simply because the library makes it possible.

Check the current package and React requirements in the [installation chapter](002-installation-and-project-setup.md). For new projects this course assumes React 18.2 or newer and the current `motion` package. The MIT license in this repository covers the notes, not the Motion library or React.

## Recap

- React state describes what the interface means and chooses a target.
- Motion interpolates visual properties between targets.
- Motion components keep normal HTML or SVG semantics.
- Use declarative props for state-driven animation and imperative APIs for sequences that need them.
- Keep keyboard behavior, accessibility state, and reduced-motion behavior in mind from the beginning.

### Practice

1. Change the selected card example so selection changes only the border and opacity.
2. Add a second card with its own state and verify that toggling one does not change the other.
3. Explain which part of the example is React state and which part is Motion interpolation.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr>
  </thead>
  <tbody>
    <tr><td>Start of course</td><td><a href="../qnas/001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals Q&amp;A</a></td><td><a href="../examples/001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals example</a></td><td><a href="002-installation-and-project-setup.md">002 - Installation and project setup</a></td></tr>
  </tbody>
</table>

# Presence modes and dynamic exit data example

This example switches through a small slide deck. AnimatePresence uses wait mode so one slide exits before the next enters. A custom direction value determines which side the old and new slides use.

## Setup

Install Motion with `npm install motion`. Save as `src/DirectionalSlides.jsx` and render `<DirectionalSlides />`.

## `src/DirectionalSlides.jsx`

~~~jsx
import { useState } from "react";
import { AnimatePresence, motion } from "motion/react";

const slides = [
  { id: "one", title: "Presence mode", text: "Choose how incoming and outgoing children are coordinated." },
  { id: "two", title: "Dynamic exit", text: "Pass changing direction through presence custom data." },
  { id: "three", title: "Stable identity", text: "Use a key that represents the active slide." },
];

const slideVariants = {
  enter: (direction) => ({
    opacity: 0,
    x: direction > 0 ? 48 : -48,
  }),
  center: { opacity: 1, x: 0 },
  exit: (direction) => ({
    opacity: 0,
    x: direction > 0 ? -48 : 48,
  }),
};

export function DirectionalSlides() {
  const [index, setIndex] = useState(0);
  const [direction, setDirection] = useState(1);
  const slide = slides[index];

  function move(step) {
    setDirection(step);
    setIndex((current) => (current + step + slides.length) % slides.length);
  }

  return (
    <main style={{ maxWidth: 640, margin: "3rem auto", padding: 20, fontFamily: "system-ui" }}>
      <h1>Directional presence</h1>
      <div style={{ minHeight: 220, overflow: "hidden", border: "1px solid #cbd5e1", borderRadius: 12, padding: 20 }}>
        <AnimatePresence mode="wait" custom={direction} initial={false}>
          <motion.article
            key={slide.id}
            variants={slideVariants}
            initial="enter"
            animate="center"
            exit="exit"
            transition={{ duration: 0.24, ease: "easeInOut" }}
          >
            <p>Slide {index + 1} of {slides.length}</p>
            <h2>{slide.title}</h2>
            <p>{slide.text}</p>
          </motion.article>
        </AnimatePresence>
      </div>
      <div style={{ display: "flex", justifyContent: "space-between", marginTop: 12 }}>
        <button type="button" onClick={() => move(-1)}>Previous</button>
        <button type="button" onClick={() => move(1)}>Next</button>
      </div>
      <p aria-live="polite">Showing slide {index + 1} of {slides.length}.</p>
    </main>
  );
}
~~~

## Walkthrough and expected result

The key changes when the active slide changes, so React treats the old content as exiting and the new content as entering. The function variants receive the current direction through AnimatePresence's custom prop. Wait mode makes the new slide wait until the old slide finishes exiting. The controls remain available and are native buttons.

Click Next and Previous, including at either end of the deck. The index wraps around. The exit moves opposite the direction of travel, and the entering slide comes from the direction of travel. Initial false prevents the first slide from animating on initial render.

## Variations and common mistakes

- Change the mode to sync and compare simultaneous transitions.
- Use a shorter distance and duration for a calmer transition.
- Remove custom data and see why the exit no longer reflects the newest direction.
- Do not use wait mode for several simultaneous list children.
- Keep slide IDs stable and unique.
- Keep AnimatePresence mounted while selecting a new slide.
- The live status communicates the active position even if movement is reduced.
- For reduced motion, keep the same wait and content order but remove the translation.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations example</a></td><td><a href="../lessons/012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data lesson</a></td><td><a href="../qnas/012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data Q&amp;A</a></td><td><a href="013-layout-animations.md">013 - Layout animations example</a></td></tr></tbody>
</table>
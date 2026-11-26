# Responsive motion patterns example

This example uses CSS breakpoints to define a one-column or two-column card grid. Each card's entrance distance comes from a responsive CSS variable, while Motion animates the same opacity and y targets at every size.

## Setup

Install Motion with `npm install motion`. Save the component as `src/ResponsiveCards.jsx` and its styles as `src/responsive-cards.css`. Import the CSS from the component file.

## `src/ResponsiveCards.jsx`

~~~jsx
import { motion, MotionConfig, useReducedMotion } from "motion/react";
import "./responsive-cards.css";

const cards = [
  {
    title: "CSS owns the layout",
    body: "The grid changes its number of columns with a media query.",
  },
  {
    title: "Motion owns the entrance",
    body: "The same animated opacity and y targets work at each breakpoint.",
  },
  {
    title: "The endpoint is responsive",
    body: "A CSS variable supplies a smaller or larger initial distance.",
  },
];

function CardGrid() {
  const shouldReduceMotion = useReducedMotion();

  return (
    <main className="responsive-shell">
      <header>
        <p className="responsive-eyebrow">Motion learning note</p>
        <h1>Responsive cards</h1>
        <p>
          Resize the viewport to change the grid. The animation keeps one
          behavior while CSS supplies the appropriate starting distance.
        </p>
      </header>

      <section className="responsive-grid" aria-label="Responsive motion examples">
        {cards.map((card, index) => (
          <motion.article
            key={card.title}
            className="responsive-card"
            initial={{
              opacity: 0,
              y: shouldReduceMotion ? 0 : "var(--entry-distance-y)",
            }}
            animate={{ opacity: 1, y: 0 }}
            transition={{
              duration: shouldReduceMotion ? 0 : 0.3,
              delay: shouldReduceMotion ? 0 : index * 0.05,
              ease: "easeOut",
            }}
          >
            <h2>{card.title}</h2>
            <p>{card.body}</p>
          </motion.article>
        ))}
      </section>
    </main>
  );
}

export function ResponsiveCards() {
  return (
    <MotionConfig reducedMotion="user">
      <CardGrid />
    </MotionConfig>
  );
}
~~~

## `src/responsive-cards.css`

~~~css
.responsive-shell {
  max-width: 960px;
  margin: 0 auto;
  padding: 48px 20px 96px;
  color: #172033;
  font-family: system-ui, sans-serif;
  line-height: 1.6;
}

.responsive-eyebrow {
  color: #6d28d9;
  font-weight: 700;
}

.responsive-grid {
  display: grid;
  grid-template-columns: minmax(0, 1fr);
  gap: 16px;
}

.responsive-card {
  --entry-distance-y: 12px;
  padding: 22px;
  border: 1px solid #d8dce6;
  border-radius: 16px;
  background: #f8fafc;
}

@media (min-width: 48rem) {
  .responsive-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .responsive-card {
    --entry-distance-y: 28px;
  }
}
~~~

## How it works

CSS controls the grid columns and defines `--entry-distance-y` at each breakpoint. Motion reads that variable as the initial y value, then animates every card to y 0. The React component does not inspect viewport width, so its initial tree is consistent between server and browser.

The reduced-motion hook removes the initial y distance and duration. The global policy also suppresses transform/layout animation when the system preference requests it. The cards remain readable and in the same document order.

The breakpoint changes the layout through CSS. This example does not promise that an in-progress Motion animation restarts if the viewport changes mid-transition; inspect that case and add explicit retargeting if the design requires it.

## Expected behavior

- A narrow viewport shows one column.
- A wide viewport shows two columns.
- Cards use a smaller starting y distance on narrow screens.
- Reduced motion shows the same content without entrance travel.
- Resizing changes layout without changing content order or semantics.

## Adapt the example

- Add another CSS breakpoint and choose a restrained entry distance.
- Use `layout` for a React state change that reorders or resizes cards.
- Remove the stagger when a long list should appear immediately.
- Add a media-query state only if animation behavior truly needs JavaScript branching.
- Test resizing during the entrance and decide whether live retargeting is required.

## Common mistakes

- **Viewport read during render:** let CSS select responsive values instead.
- **Variable missing:** define it on the animated element or one of its ancestors.
- **Breakpoint animation not retargeted:** resize may change CSS without restarting Motion; model that requirement explicitly.
- **CSS transition conflict:** remove generic transition classes from Motion-controlled properties.
- **Reduced motion tied to screen size:** handle the user preference separately from breakpoints.
- **Content order changes unexpectedly:** keep DOM order meaningful across the grid.
- **Excess travel on mobile:** keep movement proportional and easy to follow.

## Practice

1. Change the narrow and wide entry distances and compare.
2. Resize while the cards are animating.
3. Add a third column at a wider breakpoint.
4. Turn off the stagger and compare how quickly content becomes available.
5. Enable reduced motion and confirm the cards stay clear and in order.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion example</a></td><td><a href="../lessons/025-responsive-motion-patterns.md">025 - Responsive motion patterns lesson</a></td><td><a href="../qnas/025-responsive-motion-patterns.md">025 - Responsive motion patterns Q&amp;A</a></td><td><a href="026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion example</a></td></tr></tbody>
</table>

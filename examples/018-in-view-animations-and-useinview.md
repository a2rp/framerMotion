# In-view animations and useInView example

This example shows two scroll-triggered patterns: a card whose transition is driven by `useInView`, and a set of direct Motion reveals using `whileInView`. Both keep the written content in normal document order.

## Setup

Install Motion with `npm install motion`. Save the component as `src/InViewShowcase.jsx` and render `<InViewShowcase />` in a React page.

## `src/InViewShowcase.jsx`

~~~jsx
import { useRef } from "react";
import {
  motion,
  useInView,
  useReducedMotion,
} from "motion/react";

const features = [
  {
    title: "Direct component animation",
    body: "whileInView is a clear choice when the target itself is a Motion component and no separate visibility state is required.",
  },
  {
    title: "Boolean visibility state",
    body: "useInView returns a boolean for React logic or for an animate target. It is useful when visibility affects more than one visual prop.",
  },
  {
    title: "A deliberate viewport",
    body: "The amount and once options decide how much of the target must appear and whether the entrance repeats after scrolling away.",
  },
];

function ObservedCard() {
  const cardRef = useRef(null);
  const isInView = useInView(cardRef, {
    once: true,
    amount: 0.25,
  });
  const shouldReduceMotion = useReducedMotion();

  return (
    <motion.article
      ref={cardRef}
      aria-labelledby="observed-title"
      initial={false}
      animate={
        isInView
          ? { opacity: 1, y: 0 }
          : { opacity: 0.55, y: shouldReduceMotion ? 0 : 18 }
      }
      transition={{ duration: shouldReduceMotion ? 0 : 0.35, ease: "easeOut" }}
      style={{
        padding: 24,
        borderRadius: 16,
        border: "1px solid #cbd5e1",
        background: "white",
      }}
    >
      <h2 id="observed-title">Visibility state from useInView</h2>
      <p>
        {isInView
          ? "This card entered the observed area. The hook stops observing after its first entry."
          : "Scroll until at least a quarter of this card is visible."}
      </p>
    </motion.article>
  );
}

export function InViewShowcase() {
  const shouldReduceMotion = useReducedMotion();

  return (
    <main
      style={{
        maxWidth: 760,
        margin: "0 auto",
        padding: "48px 24px 120px",
        fontFamily: "system-ui, sans-serif",
        lineHeight: 1.6,
        color: "#172033",
      }}
    >
      <header style={{ minHeight: "55vh", display: "grid", alignContent: "center" }}>
        <p style={{ color: "#6750a4", fontWeight: 700 }}>Motion learning note</p>
        <h1>Scroll-triggered reveals</h1>
        <p>
          The observed card uses a boolean hook state. The later feature cards
          animate directly when they enter the browser viewport.
        </p>
      </header>

      <ObservedCard />

      <section aria-labelledby="features-title" style={{ paddingTop: "20vh" }}>
        <h2 id="features-title">Direct whileInView examples</h2>
        <div style={{ display: "grid", gap: 24 }}>
          {features.map((feature, index) => (
            <motion.article
              key={feature.title}
              initial={
                shouldReduceMotion
                  ? { opacity: 1 }
                  : { opacity: 0, y: 20 }
              }
              whileInView={{ opacity: 1, y: 0 }}
              viewport={{ once: true, amount: 0.2 }}
              transition={{
                duration: shouldReduceMotion ? 0 : 0.35,
                delay: shouldReduceMotion ? 0 : index * 0.06,
                ease: "easeOut",
              }}
              style={{
                padding: 24,
                borderRadius: 16,
                border: "1px solid #d8dce6",
                background: "#f7f5ff",
              }}
            >
              <h3>{feature.title}</h3>
              <p>{feature.body}</p>
            </motion.article>
          ))}
        </div>
      </section>

      <footer style={{ marginTop: "20vh" }}>
        <p>
          The cards do not control focus or hide information from the document.
          Reduced motion removes their entrance travel and timing.
        </p>
      </footer>
    </main>
  );
}
~~~

## How it works

`ObservedCard` attaches the same ref to the Motion article and passes it to `useInView`. The hook returns a boolean. Before the card enters the viewport, it uses a restrained offset; after the first entry, `once: true` leaves the state true.

The feature list uses `whileInView` directly. Its `viewport` options request one-time reveals after 20 percent of a card is visible. A short delay based on list position creates a small stagger without holding the content for long.

`useReducedMotion` removes the vertical travel and duration. The text remains in the document and the examples do not move focus.

## Expected behavior

- The observed card changes after at least a quarter of it enters view.
- The hook driven card does not reverse when you scroll away because `once` is true.
- Each feature card reveals as it enters the viewport.
- Reduced motion removes the travel and delay.
- Scrolling does not change keyboard focus.

## Adapt the example

- Change `amount` from `0.25` to `"some"` or `"all"` and compare the threshold.
- Set `once: false` if a repeated visible/invisible state is actually useful.
- Move the features into an overflow panel and use a root ref with `useInView` or `viewport.root`.
- Adjust `margin` to trigger earlier or later at a viewport edge.
- For a progress-linked effect instead of a threshold reveal, use `useScroll`.

## Common mistakes

- **Ref not attached:** the hook must measure a rendered element.
- **Wrong scroll root:** nested panels need their own root configured.
- **Threshold too strict:** a tall card may never satisfy `amount: "all"`.
- **Unexpected replay:** the default observes repeated entries; opt into `once` for a one-time reveal.
- **Overlong stagger:** keep delays short so a long list remains readable.
- **Motion carries meaning:** ensure the text and controls remain useful without the transition.
- **Reduced motion ignored:** make large travel and repeated motion optional.

## Practice

1. Set `once` to false and observe the card as it enters and leaves.
2. Increase the threshold and see whether the card can meet it at your viewport size.
3. Configure a scrollable panel as the root.
4. Remove the delay from the feature cards and compare readability.
5. Enable reduced motion and confirm that the content remains clear.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll example</a></td><td><a href="../lessons/018-in-view-animations-and-useinview.md">018 - In-view animations and useInView lesson</a></td><td><a href="../qnas/018-in-view-animations-and-useinview.md">018 - In-view animations and useInView Q&amp;A</a></td><td><a href="019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate example</a></td></tr></tbody>
</table>

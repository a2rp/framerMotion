# In-view animations and useInView

An in-view animation responds when an element enters or leaves a viewport. It is a good fit for a one-time reveal, a section that changes state when visible, or an item inside a scrolling panel.

Motion offers two related APIs:

- `whileInView` animates a Motion component directly.
- `useInView` returns a boolean that can drive React logic or a Motion component's `animate` prop.

Use these when a visibility threshold is enough. If an effect must follow every point of a scroll range, use `useScroll` instead.

## Animate a component with whileInView

A Motion component can define its visible target inline:

~~~jsx
<motion.article
  initial={{ opacity: 0, y: 20 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true, amount: 0.25 }}
>
  ...
</motion.article>
~~~

The component starts from `initial` and animates toward `whileInView` when it meets the viewport condition. By default, the animation can respond again when the element leaves and re-enters. Set `viewport.once` to true when it should animate only the first time.

For larger interfaces, variants can keep the hidden and visible states reusable:

~~~jsx
const reveal = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
};

<motion.article
  variants={reveal}
  initial="hidden"
  whileInView="visible"
  viewport={{ once: true, amount: 0.2 }}
/>
~~~

Use the `viewport` option to control when the state changes:

| Option | Purpose |
| --- | --- |
| `once` | Stop observing after the first entry |
| `amount` | Required visible portion: `"some"`, `"all"`, or a number from 0 to 1 |
| `margin` | Expand or contract the detection area |
| `root` | Use a scrollable ancestor instead of the browser viewport |

The default `amount` is `"some"`. Choose a numeric amount when the element should be substantially visible before changing state. For a very tall element, `"all"` may never be satisfied, so a proportion is often more practical.

## Read visibility with useInView

Use `useInView` when visibility should affect React logic or a component that is not itself a Motion element:

~~~jsx
import { useRef } from "react";
import { motion, useInView } from "motion/react";

function Notice() {
  const ref = useRef(null);
  const isInView = useInView(ref, { once: true, amount: 0.3 });

  return (
    <motion.aside
      ref={ref}
      initial={{ opacity: 0, y: 16 }}
      animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 16 }}
    >
      ...
    </motion.aside>
  );
}
~~~

The ref must be attached to the element whose visibility is measured and passed to the hook. The hook returns a boolean. It starts as false by default, becomes true when the element enters the configured viewport, and can return to false after exit when `once` is false.

`useInView` is React state: changing the boolean renders the component again. That is suitable for discrete transitions and conditional logic. For continuous visual interpolation, use Motion values with `useScroll` rather than converting every scroll frame into React state.

## Choose a viewport

By default, the browser window is the viewport. To observe items inside a scrollable panel, make the panel the `root` and observe a descendant:

~~~jsx
const panelRef = useRef(null);
const itemRef = useRef(null);

const isInView = useInView(itemRef, { root: panelRef });

return (
  <div ref={panelRef} style={{ height: 300, overflowY: "auto" }}>
    <article ref={itemRef}>...</article>
  </div>
);
~~~

The same idea is available with `whileInView`:

~~~jsx
<motion.article
  initial={{ opacity: 0 }}
  whileInView={{ opacity: 1 }}
  viewport={{ root: panelRef, once: true }}
/>
~~~

`root` should refer to the scrollable ancestor whose visible area defines the observation. The target should be inside that root. The root needs a real scrolling viewport, such as a constrained height with overflow.

## Tune the detection area

`margin` adjusts the viewport boundary used to detect entry and exit. Positive margins extend the detection region; negative margins shrink it. The value follows CSS margin order: top, right, bottom, left.

For example, `margin: "0px 0px -80px 0px"` shrinks the lower boundary so a target must travel farther into view before it counts. A positive bottom margin can trigger earlier, before the element is fully visible.

`amount` and `margin` solve different problems:

- `amount` specifies how much of the target must be visible.
- `margin` changes the viewport boundary used for detection.

Adjust one at a time. A condition that requires the entire target to be visible may never happen when the target is taller than the available viewport.

## Control initial and repeated behavior

The `useInView` hook accepts `initial`, which sets the value returned before the element has been measured. The default is false. Set `initial: true` only when the interface should begin in the visible state before measurement. It is not a replacement for choosing a clear animation state.

With `once: true`, the hook becomes true after the first entry and stops observing. With `once: false`, it can switch back to false after exit and true on re-entry. The `whileInView` viewport option has the same one-time versus repeat choice.

Choose repeat behavior intentionally:

- A card entrance usually runs once so scrolling back does not replay it.
- A small emphasis tied to a view state may repeat if that behavior remains calm and predictable.
- Avoid frequent repeated motion when users scroll back and forth near a threshold.

## Use effects and state carefully

Because `useInView` returns React state, a `useEffect` can respond to transitions:

~~~jsx
useEffect(() => {
  if (isInView) {
    // Perform a discrete action when visibility changes.
  }
}, [isInView]);
~~~

Use effects for real side effects, not simply to copy the boolean into another state variable. Do not start audio, submit data, or perform an irreversible action only because a user scrolled an element into view. If the effect should happen once, guard it explicitly or use `once` for the observation behavior.

Do not attach a high-frequency continuous animation to a boolean observer. The hook reports threshold entry/exit, not a normalized progress value.

## Accessibility and content

A reveal should decorate content, not make essential instructions unavailable. Keep content in the document and avoid requiring a scroll animation to understand what a control does. If the initial visual state is transparent or translated, make sure the content still has a sensible fallback for server rendering and before client measurement.

Respect reduced motion for large translations, zooms, or parallax-like reveals. A simple opacity change or immediate visible state may be more suitable. Preserve keyboard focus visibility and do not move focus because an item entered the viewport.

For a sequence of cards, stagger can add order, but keep the delay short and avoid making a long list wait to become readable. Consider animating only a few visual properties and keep the final layout stable.

## Troubleshooting

1. **Nothing enters view:** confirm the ref is on the target and the target can intersect the actual viewport.
2. **A nested panel is ignored:** set its ref as `root` or `container` as appropriate and confirm the panel scrolls.
3. **Animation never begins:** reduce `amount`, inspect `margin`, and check whether the target is taller than the viewport.
4. **Animation replays on every scroll:** use `once: true` if the interaction should be one-time.
5. **State stays true after leaving:** that is expected when `once` is true.
6. **Content flashes hidden:** reconsider the hidden initial state and use a visible initial/fallback state when appropriate.
7. **React renders more than expected:** use `whileInView` for direct component animation or reserve `useInView` state for discrete logic.
8. **Movement feels uncomfortable:** reduce distance, honor reduced motion, and keep text readable.

## Recap and practice

Use `whileInView` for direct Motion component transitions. Use `useInView` when a boolean visibility state is needed by React. Tune `amount`, `margin`, and `root` to describe a meaningful intersection, and choose whether the animation should run once or repeat.

Practice with a small card reveal. First use the window viewport, then place the card in a scroll panel and set the panel as the root. Compare a one-time entrance with a repeated state, and verify the content remains useful when the motion is reduced.

Continue with [imperative sequences with useAnimate](019-imperative-sequences-with-useanimate.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll</a></td><td><a href="../qnas/018-in-view-animations-and-useinview.md">018 - In-view animations and useInView Q&amp;A</a></td><td><a href="../examples/018-in-view-animations-and-useinview.md">018 - In-view animations and useInView example</a></td><td><a href="019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate</a></td></tr></tbody>
</table>

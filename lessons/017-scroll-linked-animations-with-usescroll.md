# Scroll-linked animations with useScroll

Scrolling can act as a continuous animation input. Motion's `useScroll` hook exposes scroll position and progress as Motion values. Bind them directly to styles or derive values with `useTransform` and `useSpring`.

This is different from an in-view animation. Scroll-linked effects follow a measured range continuously; in-view effects react to an element crossing a visibility threshold. Chapter 018 covers the latter.

## The values returned by useScroll

~~~jsx
import { useScroll } from "motion/react";

const {
  scrollX,
  scrollY,
  scrollXProgress,
  scrollYProgress,
  scrollXLength,
  scrollYLength,
} = useScroll();
~~~

| Value | Meaning |
| --- | --- |
| `scrollX`, `scrollY` | Current scroll position in pixels |
| `scrollXProgress`, `scrollYProgress` | Normalized progress from 0 to 1 over the measured range |
| `scrollXLength`, `scrollYLength` | Maximum scroll position of the container in pixels |

With no options, the browser viewport is the container and the vertical axis is tracked. Progress is relative to the selected offsets, so it does not always mean whole-document progress when a target or custom offsets are used.

A page reading indicator can use the progress Motion value directly:

~~~jsx
const { scrollYProgress } = useScroll();

return (
  <motion.div
    aria-hidden="true"
    style={{ scaleX: scrollYProgress, transformOrigin: "0% 50%" }}
  />
);
~~~

Motion updates the style without a React render for every scroll frame.

## Scroll-linked versus scroll-triggered

| Pattern | Behavior | Typical API |
| --- | --- | --- |
| Scroll-linked | A value changes continuously through a scroll interval | `useScroll` and Motion values |
| Scroll-triggered | An action starts when an element crosses a visibility threshold | `whileInView` or `useInView` |

Use continuous progress for a reading bar, restrained parallax, or a visual tied to a section's travel. Use an in-view trigger when the requirement is simply to animate an item when it becomes visible.

## Target and container

Two refs have separate roles:

- `target` is the element whose progress is measured.
- `container` is the scrollable viewport whose position is tracked. It defaults to the browser viewport.

Measure an article section as it travels through the page:

~~~jsx
const sectionRef = useRef(null);

const { scrollYProgress } = useScroll({
  target: sectionRef,
  offset: ["start end", "end start"],
});

return <section ref={sectionRef}>...</section>;
~~~

Here, progress starts when the target's start meets the container's end and finishes when the target's end meets the container's start.

To track a nested scroll panel, attach a ref to the element that actually scrolls and pass it as `container`:

~~~jsx
const panelRef = useRef(null);
const { scrollYProgress } = useScroll({ container: panelRef });

return <div ref={panelRef} style={{ height: 320, overflowY: "auto" }}>...</div>;
~~~

A custom container needs a constrained size and scrollable content. If Motion warns that the ref is not hydrated, attach it to a mounted HTML or SVG element. A custom component must forward its ref to a DOM element.

## Define the progress interval with offset

`offset` describes intersections between a point on the target and a point on the container. The default is `["start start", "end end"]`. For example, `"start end"` means the target's start meets the container's end.

| Offset pair | Interval described |
| --- | --- |
| `["start end", "end start"]` | Target enters at the far edge and finishes after leaving the opposite edge |
| `["start start", "end end"]` | Target start meets container start through target end meeting container end |
| `["start end", "start start"]` | Target enters until its start meets the container start |
| `["0 0.5", "1 0.5"]` | Target travels from the container midpoint to that midpoint |

Points can be `start`, `center`, `end`, normalized numbers, percentages, or pixel distances. Values outside 0 to 1 are allowed when the interval should begin before or end after the visible edges.

An offset defines measurement geometry, not animation duration. If an effect starts at an unexpected point, check the target, container, and intersections before changing the output mapping.

## Derive visual values

Use `useTransform` to map progress to visual output:

~~~jsx
const { scrollYProgress } = useScroll();
const opacity = useTransform(scrollYProgress, [0, 1], [0.35, 1]);
const y = useTransform(scrollYProgress, [0, 1], [36, -36]);

return <motion.div style={{ opacity, y }} />;
~~~

Input and output ranges need matching lengths. One progress value can drive several derived values. Prefer binding Motion values to styles for visual changes; copying every frame into React state adds renders and work.

Use `useMotionValueEvent` when logic needs to observe a change, such as detecting scroll direction. Keep that callback lightweight and do not use it as a reason to render on every frame unless the UI actually requires that.

## Performance and stability

Motion can use browser scroll-driven animation capabilities for supported styles and configurations. The execution path varies by browser and effect, so keep the design correct if it runs on the main thread.

- Prefer transforms and opacity for continuous visual changes.
- Bind Motion values directly or derive them with `useTransform`.
- Avoid expensive calculations, layout reads, or React state updates per scroll frame.
- Use CSS `position: sticky` for pinned content when suitable.
- Keep parallax distances modest and content easy to read.
- Target progress uses layout position; CSS transforms on the target or its ancestors are ignored in that measurement. Track an untransformed wrapper if needed.

## Accessibility

Keep all essential text and controls available regardless of scroll position. Motion should not be the only way to reveal information or communicate a state.

Use `useReducedMotion` to remove or reduce large decorative movement while preserving a useful static presentation. A decorative progress line can use `aria-hidden="true"`; if progress is meaningful information, expose an appropriate accessible name and progress semantics. Do not announce every frame through a live region.

## Troubleshooting

1. **Progress stays at zero:** ensure the page or container has a real scroll range.
2. **Wrong start or end:** verify the target and read each offset as a target/container intersection.
3. **Nested scrolling is ignored:** `container` must be attached to the element whose scroll position changes.
4. **Ref hydration warning:** attach the ref to a mounted DOM element; ensure custom components forward it.
5. **Effect lags behind scrolling:** a `useSpring` adds intentional smoothing.
6. **Too many React renders:** keep visual-only updates in Motion values and styles.
7. **Transformed target measures oddly:** track a wrapper and animate its child.
8. **Motion distracts from reading:** reduce travel and honor reduced-motion preferences.

## Recap and practice

`useScroll` exposes scroll position, progress, and scroll lengths as Motion values. Select the container and target deliberately, define the interval with offsets, and map progress to visual output. Use continuous scrolling only when the visual should correspond to a continuous position.

Build a page progress line, then add a subtle fade to a long article section. Change one offset at a time and compare the interval. Finally, decide whether each effect needs continuous progress or only an in-view trigger.

Continue with [in-view animations and useInView](018-in-view-animations-and-useinview.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values</a></td><td><a href="../qnas/017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll Q&amp;A</a></td><td><a href="../examples/017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll example</a></td><td><a href="018-in-view-animations-and-useinview.md">018 - In-view animations and useInView</a></td></tr></tbody>
</table>

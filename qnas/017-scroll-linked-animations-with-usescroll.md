# Scroll-linked animations with useScroll Q&A

## 1. What does useScroll do?

`useScroll` returns Motion values that track scroll position and normalized progress. Bind them to styles or derive output with hooks such as `useTransform`.

## 2. Which values does it return?

`scrollX` and `scrollY` are pixel positions; `scrollXProgress` and `scrollYProgress` are progress values; `scrollXLength` and `scrollYLength` are the container's maximum scroll positions.

## 3. What does progress from 0 to 1 represent?

It represents movement between the configured offset intersections. With a target or custom offsets, it may represent only that target's measured interval, not the whole page.

## 4. What is tracked by default?

With no options, `useScroll` tracks the browser viewport on the vertical axis. Its default offset is `["start start", "end end"]`.

## 5. What is the difference between target and container?

`target` is the element whose progress is measured. `container` is the scrollable viewport being tracked; the default container is the browser viewport.

## 6. How can I track one section as it crosses the page?

Attach a ref to the section and pass it as `target`. For a full entry-to-exit interval, offsets such as `["start end", "end start"]` describe when the section enters and leaves the viewport.

## 7. How do I track a nested scroll panel?

Attach a ref to the element that actually scrolls and pass it as `container`. Give it a constrained size, scrollable overflow, and content that creates a scroll range.

## 8. What does "start end" mean?

It means the target's start point meets the container's end point. The first point refers to the target and the second to the container.

## 9. Which offset points are allowed?

Use `start`, `center`, `end`, normalized numbers, percentages, or pixel offsets. Values outside 0 to 1 are allowed when the interval should extend past an edge.

## 10. Is offset an animation duration?

No. It defines the geometric intersections that bound progress. Map the progress to visual values separately, for example with `useTransform`.

## 11. How do I map progress to opacity?

Create a derived value with `useTransform(progress, [0, 1], [0.3, 1])` and attach it to a Motion component's `opacity` style.

## 12. Should I copy progress to React state?

Usually not for visual-only effects. Direct Motion style bindings avoid React rendering on every scroll frame. Use React state only when application logic or rendered text genuinely needs the value.

## 13. When is useMotionValueEvent useful?

Use it when logic needs to observe a change, such as detecting direction or a meaningful threshold. Keep callbacks inexpensive and avoid frame-by-frame state updates without a concrete need.

## 14. How is useScroll different from whileInView?

`useScroll` provides continuous progress across an interval. `whileInView` and `useInView` handle visibility-threshold behavior, such as starting an entrance when an item becomes visible.

## 15. Why does progress remain zero?

The page or container might not have a scroll range, the wrong container may be tracked, or the target ref may not point to a mounted element. Check dimensions and ref placement.

## 16. Why does a nested panel not respond?

`container` must point to the actual scrolling element. Confirm that it has a constrained size, scrollable overflow, and that the user is scrolling the panel.

## 17. What causes a ref hydration warning?

Motion cannot measure a target or container unless its ref reaches a mounted HTML or SVG element. Ensure a custom component forwards the ref or attach it directly to a DOM element.

## 18. Are transforms used to measure the target?

Target progress is based on layout position. CSS transforms on the target and its ancestors are ignored for that measurement. Track an untransformed wrapper when the geometry is confusing.

## 19. Is every useScroll animation hardware accelerated?

No. Motion can use browser scroll-driven capabilities for supported values and configurations, but behavior depends on the browser and effect. Keep it correct if it runs on the main thread.

## 20. Which properties are practical for continuous effects?

Transforms and opacity are good starting points. Avoid changing layout dimensions on every scroll update unless the effect requires it and performance has been checked.

## 21. Should a decorative progress line be announced?

A purely decorative line can use `aria-hidden="true"`. If progress conveys useful information, use appropriate progress semantics and a helpful label. Avoid announcing every frame with a live region.

## 22. How should reduced motion affect the effect?

Remove or reduce large decorative travel and parallax when reduced motion is requested. Keep the content and controls usable without motion.

## 23. Can useSpring smooth scroll progress?

Yes, but it adds lag. Use it only when softness is intended; a precise reading indicator generally should follow progress directly.

## 24. What should I inspect when timing feels wrong?

Check which element is the target, which element is the container, and what each offset intersection means. Adjust one endpoint at a time before changing the visual mapping.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values Q&amp;A</a></td><td><a href="../lessons/017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll lesson</a></td><td><a href="../examples/017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll example</a></td><td><a href="018-in-view-animations-and-useinview.md">018 - In-view animations and useInView Q&amp;A</a></td></tr></tbody>
</table>

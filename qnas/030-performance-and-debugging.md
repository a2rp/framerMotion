# Performance and debugging Q&A

## 1. What should I do first when an animation feels slow?

Reproduce the same interaction and record it before changing code. A trace can show whether the cost is JavaScript, React work, layout, paint, image loading, or another part of the page.

## 2. Does Motion guarantee that every animation runs off the main thread?

No. The execution path depends on the animation, property, and browser support. Motion can use browser animation features where possible, but JavaScript, layout, and paint work can still affect frame timing.

## 3. Which properties are usually good starting points for performance?

Transform properties such as x, y, scale, and rotate, plus opacity, are often efficient because they can avoid layout recalculation. Measure the actual page; layers, effects, and surrounding content can change the cost.

## 4. Why can animating width or top be expensive?

Changing geometry can require the browser to recalculate layout or repaint affected content. When it fits the visual design, a transform may produce a similar effect with less layout work.

## 5. Are transform and opacity always cheap?

No. They are useful defaults, not a universal guarantee. Large elements, filters, shadows, clipping, layer memory, and other work can still make an animation expensive.

## 6. Do Motion component animations cause a React render every frame?

Motion components can update their animated values without React rendering on every frame. React can still render when component props, state, or context change for other reasons.

## 7. When should I use a Motion Value?

Use one for a high-frequency visual value that needs to update styles but does not need to change the React component tree. Motion Values can update the DOM without triggering a React render for each change.

## 8. When should I use React state instead?

Use React state when the application meaningfully changes, such as a selected tab, dialog visibility, or submitted form result. Do not move semantic application state into a Motion Value only to avoid rendering.

## 9. Why can setState in an animation callback cause stutter?

If it updates state on every animation frame, it can schedule repeated React work alongside the animation. Update only when the application needs a semantic change, or keep purely visual values in a Motion Value.

## 10. Should I use useMemo or memo for every animated component?

No. First check a React profile to see whether repeated component rendering is a real cost. Memoization adds complexity and does not fix expensive layout or paint.

## 11. What does a browser Performance recording show?

It records work performed while loading or interacting with the page. Inspect frame activity, long tasks, JavaScript, layout, paint, and the timing of the interaction that felt slow.

## 12. Is an FPS counter enough to diagnose an issue?

No. It can show dropped frames, but it does not identify the cause. Use a trace to inspect what the browser and application were doing during those frames.

## 13. When do I need a React profile as well?

Use a React profile when you suspect components are rendering too often or doing expensive render work. A browser trace helps find browser and JavaScript costs; a React profile helps attribute component work.

## 14. How do I make an optimization comparison fair?

Use the same route, viewport, browser, device conditions, content, interaction, and capture settings before and after. Repeat the same operation and compare more than one recording.

## 15. Why should I profile on a lower-powered device?

A powerful idle desktop can hide CPU contention that users experience on phones or busy pages. CPU throttling or a representative device can reveal whether the optimization helps under realistic constraints.

## 16. What if the page stutters while React updates?

Look for long synchronous work and expensive component rendering during the same interaction. Separate high-frequency visual updates from semantic React state and verify the change with both a browser trace and, when useful, a React profile.

## 17. What if the page stutters during scrolling?

Check scroll callback work, layout changes, image decoding, and the animated properties. Avoid expensive work on every scroll update. If the goal is only to reveal content when it enters view, consider a viewport-triggered animation instead of continuously tracking scroll.

## 18. What if an animation flashes or jumps on first render?

Check the initial values, font and image loading, hydration output, and layout shifts after mount. Keep the initial state deterministic and reserve the correct space for content that loads later.

## 19. Is a layout animation always slow?

No. It is useful when elements should move or resize as layout changes. Its measurements and affected subtree still have a cost, so keep it focused and profile it with realistic content.

## 20. What does useMotionValue update without React rendering?

It updates the value and lets Motion apply it to the bound visual property. React does not have to render a new component tree for each Motion Value update.

## 21. Can console logging hurt a performance recording?

Yes. Logging on every frame adds diagnostic work and can distort the measurement. Remove or limit high-frequency logging when recording production-like behavior.

## 22. Does LazyMotion fix a slow-running animation?

Not by itself. LazyMotion can reduce loaded feature code and help startup bundle size. Runtime animation cost is a separate problem that needs a runtime trace.

## 23. Should I simplify the animation before profiling?

First establish what work is slow. Then test one change at a time. A simpler effect can be a good solution, but profiling helps avoid removing visual quality when the real bottleneck is elsewhere.

## 24. What is a good debugging checklist?

- Reproduce and record the exact interaction.
- Inspect missed frames and long tasks.
- Check script, React work, layout, paint, and asset loading.
- Use a React profile if component renders appear to be a cause.
- Form one hypothesis and change one thing.
- Record again under the same conditions.
- Confirm visual quality, interaction, and reduced-motion behavior.

## 25. What should I practice?

Record a list entrance animation once using a layout-affecting property and once using a transform. Compare the trace. Then compare a per-frame React state value with a Motion Value and confirm that the visible result stays equivalent.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion Q&amp;A</a></td><td><a href="../lessons/030-performance-and-debugging.md">030 - Performance and debugging lesson</a></td><td><a href="../examples/030-performance-and-debugging.md">030 - Performance and debugging example</a></td><td><a href="031-testing-motion-components.md">031 - Testing Motion components Q&amp;A</a></td></tr></tbody>
</table>

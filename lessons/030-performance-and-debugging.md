# Performance and debugging

Animation performance is a browser rendering problem as well as a JavaScript problem. Motion can update animated values without making React render on every animation frame, but expensive layout, paint, component work, large asset decoding, and synchronous JavaScript can still make an animation feel slow.

Measure a repeatable interaction first. Then locate the work that consumes the frame budget and change the smallest cause.

## Learning outcomes

By the end of this lesson, you should be able to:

- Distinguish React rendering work from browser layout, paint, and compositing work.
- Prefer suitable animation properties without treating them as a guarantee.
- Use Motion Values for high-frequency visual updates instead of React state.
- Record and inspect a browser performance trace.
- Diagnose slow layout, expensive paint, long JavaScript tasks, and excessive component work.
- Compare before and after changes under the same conditions.

## What makes an animation feel slow?

A frame requires the browser and application to do enough work before the next display refresh. A 60 Hz display has about 16.7 milliseconds per frame; a 120 Hz display has about 8.3 milliseconds. These are approximate frame intervals, not universal performance guarantees.

A rendered update may involve:

1. JavaScript calculating new values.
2. React rendering and committing component updates.
3. Style calculation and layout.
4. Paint and compositing.
5. The browser presenting the frame.

Not every animation uses every stage for every frame. Motion may use browser animation features or JavaScript updates depending on the property and animation. Prefer paths that avoid unnecessary rendering work, then profile the real interaction on representative devices.

## Start with a repeatable measurement

Before optimizing:

- Identify the exact route and interaction that feels slow.
- Use a representative device, browser, viewport, and content size.
- Record the initial behavior before changing code.
- Repeat the same interaction several times.
- Capture a browser Performance recording while reproducing the issue.
- Compare recordings made with the same throttling and capture settings.

Use a trace to find long tasks, frame drops, layout work, paint work, and expensive script. An FPS display can reveal that frames are being missed, but it does not identify the cause by itself.

If React rendering is suspected, use a React profiling workflow to find components that render repeatedly. A browser trace and a React component profile answer different questions; use both when needed.

## Prefer transform and opacity when they fit

Transforms such as x, y, scale, and rotate, and opacity are often efficient animation targets. The browser can often update them without recalculating layout, and may be able to composite the result efficiently.

~~~jsx
<motion.div
  initial={{ opacity: 0, y: 16 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.22 }}
/>
~~~

Properties that change geometry, such as width, height, top, or left, can cause layout or paint work. If the desired effect can be expressed with a transform, that is often a better first attempt.

This is a guideline, not a rule that every transform is fast or every layout animation is slow. Filters, shadows, clipping, image size, layer count, and surrounding content can change the cost. Measure the actual page.

## Avoid React state for every animation frame

Motion components can update animated values without triggering a React render for every frame. A Motion Value can also update a visual property directly:

~~~jsx
import { useEffect } from "react";
import { motion, useMotionValue } from "motion/react";

function Meter() {
  const x = useMotionValue(0);

  useEffect(() => {
    const timeout = window.setTimeout(() => {
      x.set(120);
    }, 500);

    return () => window.clearTimeout(timeout);
  }, [x]);

  return <motion.div className="meter" style={{ x }} />;
}
~~~

Use React state when the application meaningfully changes, such as the selected tab or whether a dialog is open. Use a Motion Value for a high-frequency visual value that does not need to change the React component tree.

Avoid setting React state from an every-frame callback such as Motion's update callback unless the application truly needs those React renders. Also avoid logging every frame in production; diagnostic work can itself distort a measurement.

## Layout animation costs and checks

The layout prop measures and animates layout changes. It is useful when content moves because the layout changed, but measurement and visual correction still have a cost.

When profiling layout animation:

- Limit layout animation to the elements that need it.
- Avoid enabling layout on a very large subtree by default.
- Check nested layout groups and elements that animate together.
- Inspect scaling artifacts on text, borders, and shadows.
- Ensure surrounding CSS provides the intended final layout.
- Test with realistic text, images, and list lengths.

Use shared layout identifiers only for elements that are intended to connect visually. Unnecessary layout work can be more expensive than a small CSS transition.

## Diagnose common symptoms

### The animation stutters when React updates

Check for a large synchronous render or expensive work during the same interaction. Keep frame-by-frame visual values in Motion Values when they do not need to update React state. Memoization is useful only after the profile shows repeated component work is a cause.

### The animation stutters when the page scrolls

Check the animated property, scroll handler work, image decoding, and layout changes. Avoid doing expensive work in scroll callbacks. Use viewport-triggered animation when the intent is only to reveal content on entry rather than track every pixel of scroll.

### The animation looks smooth on one computer but not another

Profile on a representative lower-powered device or use CPU throttling. A desktop with an idle CPU can hide work that users will feel on phones or busy pages.

### The animation causes a flash or jump

Check initial values, server-rendered output, font and image loading, and layout changes that happen after mount. Make the initial visual state deterministic and reserve space for content that loads later.

### The page is slow even when Motion is removed

The bottleneck may be data loading, component rendering, images, third-party scripts, or CSS rather than the animation. Compare the same interaction without the animated effect and inspect the browser trace before rewriting Motion code.

### The trace shows repeated layout or paint

Look at which properties change and which elements are affected. Reduce the number or size of affected elements, choose a transform when appropriate, or simplify the visual effect. Confirm the change with a new trace.

## A practical debugging workflow

1. Reproduce the issue without changing animation code.
2. Record a browser Performance trace while performing the interaction.
3. Inspect frame drops, long tasks, script, layout, paint, and compositing-related work.
4. If component work appears expensive, capture a React profile for the same interaction.
5. Form one hypothesis about the dominant cost.
6. Change one thing, such as replacing a geometry property with a transform or removing an unnecessary per-frame state update.
7. Repeat the same recording under the same conditions.
8. Confirm the visual result, interaction, keyboard behavior, and reduced-motion behavior.
9. Keep the change only when the trace or user-visible result improves without a meaningful quality regression.

Do not optimize a component based only on a guess or a single FPS number.

## Keep bundle and runtime costs separate

Bundle size affects download and startup work. Runtime animation cost affects behavior after the page is loaded. They need different measurements.

Use the bundle analyzer and production build to understand download and parse costs. Use a browser performance trace and React profile to understand runtime work. LazyMotion can reduce loaded feature code, but it does not automatically make a costly animation cheaper once it is running.

## Common mistakes

- Assuming that Motion makes every animation run off the main thread.
- Animating width or top without checking layout and paint costs.
- Updating React state every frame for a purely visual value.
- Adding memoization without profiling first.
- Recording different interactions before and after a change.
- Measuring only on a powerful development machine.
- Treating a high FPS reading as proof that loading, interaction, and accessibility are all good.
- Logging or running diagnostic work on every frame.
- Reducing animation quality before identifying the actual bottleneck.
- Confusing bundle download savings with runtime frame performance.

## Practice

Create a page with a list of cards and an entrance animation. Record the interaction in the browser Performance panel. Try one version that animates top or height and another that animates y or scale. Compare frame work, then check the result on a throttled CPU.

Add a high-frequency progress indicator using React state, then rework its visual value with a Motion Value. Compare React component renders and confirm the visible behavior is unchanged.

Continue with [Testing Motion components](031-testing-motion-components.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion</a></td><td><a href="../qnas/030-performance-and-debugging.md">030 - Performance and debugging Q&amp;A</a></td><td><a href="../examples/030-performance-and-debugging.md">030 - Performance and debugging example</a></td><td><a href="031-testing-motion-components.md">031 - Testing Motion components</a></td></tr></tbody>
</table>

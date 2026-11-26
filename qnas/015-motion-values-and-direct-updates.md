# Motion values and direct updates Q&A

## 1. What is a Motion value?

It is an observable value for animated presentation. Motion components can render it and update it independently of React's render cycle.

## 2. How do I create one in a React component?

Use `useMotionValue(initialValue)` from Motion for React and keep the returned value for the component's lifetime.

## 3. How do I attach it to an element?

Pass it through the Motion component's style for CSS values such as x or opacity, or through a supported attribute prop for SVG attributes.

## 4. Does calling set trigger React to render?

No. Motion updates its rendered visual output directly. React state changes are still needed when React-rendered content or application logic must update.

## 5. How do get and set work?

`get()` reads the latest value. `set(next)` updates it directly. Use an animation API when you want a timed interpolation rather than a direct update.

## 6. What does jump do?

It changes the value while breaking continuity: active animation/effects are stopped or ignored and velocity resets. Use it for an intentional discontinuity such as an immediate reset.

## 7. Can I read a Motion value in JSX?

You can call get in code, but React will not subscribe to it. The displayed value can become stale. Use React state or subscribe to the Motion value for a rendered readout.

## 8. How do I subscribe to changes safely?

Use `useMotionValueEvent(value, "change", callback)`. It manages the subscription through the React component lifecycle. If you call `value.on` manually, unsubscribe during cleanup.

## 9. Which events can a Motion value emit?

Events include change, animationStart, animationComplete, and animationCancel. A change callback receives the latest value.

## 10. How is getVelocity calculated?

It reports numeric velocity per second using Motion's rendered frame history. For non-numeric strings and colors, velocity is zero. Do not use it as a meaningful numeric signal for a color.

## 11. Can multiple components use the same Motion value?

Yes. Each can render the same value and therefore update together. Share it only when the elements should be visually synchronized.

## 12. Should I use a Motion value for selected-item state?

Usually not. Selection is an application fact that should be represented in React state or a store. A Motion value is best for visual values such as transform coordinates or opacity.

## 13. Why use a Motion value for dragging?

Pointer movement can update visual position many times per second. A Motion value avoids a React render for each visual sample and is designed for these updates.

## 14. Does set animate to the new target?

No. Set changes the value directly. Use an animation function, transition target, or spring when you want interpolation.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups Q&amp;A</a></td><td><a href="../lessons/015-motion-values-and-direct-updates.md">015 - Motion values and direct updates lesson</a></td><td><a href="../examples/015-motion-values-and-direct-updates.md">015 - Motion values and direct updates example</a></td><td><a href="016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values Q&amp;A</a></td></tr></tbody>
</table>
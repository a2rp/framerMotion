# Motion values and direct updates

A Motion value is an observable value that Motion can render and animate independently of React's render cycle. Use one when a visual property changes frequently or must be shared between components without setting React state on every frame.

## Create and render a Motion value

Create a Motion value with `useMotionValue` and pass it to a Motion component's style or supported attribute prop:

~~~jsx
import { motion, useMotionValue } from "motion/react";

function Marker() {
  const x = useMotionValue(0);
  return <motion.div style={{ x }} />;
}
~~~

The initial value is the starting state. Setting the value updates the rendered visual property through Motion's renderer. This does not trigger a React render. Motion batches updates to its frame loop, which is useful for drag, pointer-following, and other high-frequency visual changes.

Use Motion values for presentation values such as a transform, opacity, or color. Keep application facts such as selected record, form validity, or whether a request succeeded in React state.

## Read and set values

The common methods are `get()` and `set(next)`:

~~~jsx
x.set(120);
const currentX = x.get();
~~~

`set` updates the current value. It does not by itself define a timed transition. Use Motion's animation APIs or an animation target when a value should interpolate over time. Use `jump(next)` when you deliberately want to break continuity, reset velocity, and stop an attached animation or effect.

A Motion value can be shared by multiple Motion components. They then render the same latest value. Share a value only when the elements should truly move or change together.

## Track velocity and events

Numeric Motion values can report velocity with `getVelocity()`, measured per second. It is useful for gesture and physics decisions. String or color values do not have numeric velocity and report zero.

Subscribe to changes with `useMotionValueEvent` inside React:

~~~jsx
useMotionValueEvent(x, "change", (latest) => {
  console.log("Latest x:", latest);
});
~~~

The hook subscribes and cleans up with the component lifecycle. Motion values also provide `on(event, callback)`, which returns an unsubscribe function. For event subscriptions created in an effect, clean them up when the effect ends. Events include change and animation lifecycle notifications.

## Separate visual updates from semantic state

A UI may need a textual readout while the visual property updates continuously. Subscribe to the Motion value and copy a suitably throttled or rounded value into React state only when the interface needs that text. This causes React renders for the label, but the Motion component can still update its style directly.

Avoid reading a Motion value during render as if it were React state. React does not subscribe to `get()`. Use React state for content that React must render, or a Motion value subscription for observations of an animated value.

## Direct update versus React state

| Need | Use |
| --- | --- |
| High-frequency transform value | Motion value |
| Current visual value in an event handler | `value.get()` |
| Set a new visual value directly | `value.set(next)` |
| Reset continuity and velocity | `value.jump(next)` |
| Render text, selection, or application facts | React state |
| React to Motion value events | `useMotionValueEvent` |

## Common mistakes and limits

- **Expecting a React render:** Motion value changes update Motion output directly; they do not notify React to render.
- **Using it as the source for all app state:** keep domain facts in React state or another application store.
- **Creating a value on every render:** use `useMotionValue` inside the component so the hook retains a stable value.
- **Forgetting cleanup:** use `useMotionValueEvent` or unsubscribe from manual listeners.
- **Assuming strings have velocity:** velocity is meaningful for numeric values; string and color values report zero.
- **Using set for a timed tween:** set changes the value directly; choose an animation API when interpolation is desired.
- **Sharing one value accidentally:** every component receiving the same value will reflect its updates.
- **Updating React state at frame rate:** reserve React subscriptions for information the interface needs, and avoid turning every visual frame into a whole component render.

## Recap and practice

Motion values hold observable presentation values that Motion can update without a React render. Use get and set for direct access, getVelocity for numeric motion, and useMotionValueEvent for lifecycle-safe subscriptions.

Practice with a horizontal marker. Update its Motion value from buttons, display a rounded textual readout through an event subscription, and keep the visual position driven directly by the Motion value. Compare this with storing the position in React state on every pointer move.

Continue with [transforming and composing motion values](016-transforming-and-composing-motion-values.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups</a></td><td><a href="../qnas/015-motion-values-and-direct-updates.md">015 - Motion values and direct updates Q&amp;A</a></td><td><a href="../examples/015-motion-values-and-direct-updates.md">015 - Motion values and direct updates example</a></td><td><a href="016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values</a></td></tr></tbody>
</table>
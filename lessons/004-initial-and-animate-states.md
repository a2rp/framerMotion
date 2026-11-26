# Initial and animate states

A Motion component uses target states to describe how it should look at different points in its lifecycle. The `initial` prop describes its starting visual state. The `animate` prop describes the target it should reach. When React supplies a new target, Motion transitions from the current rendered values toward that target.

## Define a first state and a destination

~~~jsx
import { motion } from "motion/react";

export function WelcomeMessage() {
  return (
    <motion.p
      initial={{ opacity: 0, y: 12 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3, ease: "easeOut" }}
    >
      The message enters the page.
    </motion.p>
  );
}
~~~

The paragraph begins transparent and slightly below its final position, then moves to opacity 1 and y 0. The `y` value is an independent transform in pixels. A transition controls the interpolation; this lesson uses a tween, while the next chapter compares tween and spring transitions.

Use properties that communicate a useful change. A short fade or small position change can establish continuity. Large movement or long delays can make content harder to reach and understand.

## Let React choose the target

The `animate` object can depend on React props or state:

~~~jsx
<motion.div
  initial={{ opacity: 0, scale: 0.96 }}
  animate={{
    opacity: active ? 1 : 0.6,
    scale: active ? 1 : 0.98,
  }}
/>
~~~

When `active` changes, React renders a new target object. Motion animates the changed values. Do not store every animation frame in React state. Store the meaningful state and let Motion interpolate visual values.

If a component mounts directly in a state that can vary, decide whether its first render should always animate from a separate starting state. For example, a card that is already selected on initial render may not need to first appear unselected and animate into the selected state.

## Skip the initial entrance

Set `initial={false}` when an element should render immediately at its `animate` target on its first render:

~~~jsx
<motion.div initial={false} animate={{ opacity: isReady ? 1 : 0.5 }}>
  Current content
</motion.div>
~~~

This is useful for content that should not animate from an invented pre-mount state, such as an element rendered with server output or an interface whose initial state is already established. It does not disable later transitions when the `animate` target changes.

Keep the initial state deterministic between server rendering and hydration. Avoid reading browser-only state during render to produce different initial markup on the server and client.

## Objects, labels, and controls

`animate` can receive a target object, a variant label, or supported animation controls. This chapter focuses on objects. A target object is straightforward for a single component or a small state mapping. Variant labels are useful when multiple elements share named states; see [variants](007-variants-and-reusable-states.md).

The `initial` and `animate` props are different from `exit`. `exit` describes a target to play when a child is removed inside `AnimatePresence`. Without a presence boundary, React unmounts a removed element immediately and it cannot finish an exit animation. See [AnimatePresence](011-animatepresence-and-exit-animations.md).

## Values and transitions

An animation target maps supported property names to values. Motion can animate common CSS values and independent transforms such as `x`, `y`, `scale`, and `rotate`. The types and formats of the starting and destination values need to be compatible. If a property does not interpolate as expected, inspect its computed CSS and verify the value format supported by the installed version.

A `transition` can be attached to a Motion component or included in animation targets where supported. Keep target values separate from timing decisions when doing so makes the component easier to read:

~~~jsx
<motion.div
  initial={{ opacity: 0, x: -8 }}
  animate={open ? { opacity: 1, x: 0 } : { opacity: 0.7, x: 0 }}
  transition={{ duration: 0.22 }}
/>
~~~

Do not assume every repeated render should replay an entrance. Motion reacts to target changes; if the target remains equal, there may be no new transition. To replay an entrance, change component identity deliberately with a stable key or control the animation through an appropriate API. Changing keys remounts React components, so use it only when a new visual identity is actually intended.

## Server output and first paint

Motion components can render an initial visual state as part of server-generated output. Choose `initial` based on the intended first paint. If the server should emit the destination state, use `initial={false}` when that matches the interface. Keep the value consistent between server and client to avoid hydration mismatches.

## Common mistakes

- **Using a boolean to hide content with opacity only.** Opacity changes appearance but not whether an element is interactive or exposed to assistive technology.
- **Expecting `exit` to work without `AnimatePresence`.** React removal happens immediately outside the presence boundary.
- **Animating from a state that users should never see.** Use `initial={false}` when the destination is the correct first render.
- **Changing the target without updating React state.** The target is just a description; React props or state usually determine when it changes.
- **Updating React state every frame.** Use Motion values for continuous visual data.
- **Using incompatible value types.** Make sure the start and end formats can be interpolated.
- **Using a new key on every render.** That forces remounts and restarts entrance behavior.

## Recap and practice

- `initial` describes the starting target.
- `animate` describes the target Motion should reach.
- React state can choose the target; Motion handles the visual interpolation.
- `initial={false}` skips a separate entrance on the first render but allows later target changes.
- `exit` needs an `AnimatePresence` boundary when React removes the child.

Practice by adding a boolean prop called `active` to a card. Keep the same `initial` state but switch the `animate` target when the prop changes. Then set `initial={false}` and describe which part of the animation changes on first render.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="003-motion-components-and-props.md">003 - Motion components and props</a></td><td><a href="../qnas/004-initial-and-animate-states.md">004 - Initial and animate states Q&amp;A</a></td><td><a href="../examples/004-initial-and-animate-states.md">004 - Initial and animate states example</a></td><td><a href="005-transitions-and-springs.md">005 - Transitions and springs</a></td></tr>
  </tbody>
</table>

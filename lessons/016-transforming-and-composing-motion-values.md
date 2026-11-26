# Transforming and composing motion values

Motion values become more powerful when one value can derive other values. `useTransform` maps a source value into a new output, and `useSpring` can smooth a changing source. The derived values remain Motion values, so Motion can render them without React updating on every animation frame.

## Map one value to another range

Use the range form of `useTransform` to map input stops to output stops:

~~~jsx
const opacity = useTransform(
  x,
  [-160, 0, 160],
  [0.25, 1, 0.25]
);
~~~

At x = 0, opacity is 1. At either outer stop it is 0.25. Motion interpolates between the stops. The input and output arrays must have the same length. Input values must be a consistently increasing or decreasing numeric range; output values can be numbers, colors, units, or compatible strings.

By default, mapping clamps output at the first and last output values. Pass `{ clamp: false }` when the mapping should continue beyond the input endpoints, such as a rotation that repeats for each scroll interval. Unclamped mapping can grow without bound, so use it intentionally.

## Derive values with a function

The function form of `useTransform` reads one or more Motion values and returns a derived value:

~~~jsx
const combined = useTransform(() => x.get() + y.get());
~~~

Motion subscribes to Motion values read with `get()` inside the function. When either source changes, the derived function runs again in the frame loop. Keep the calculation pure and inexpensive. Use range mapping for a simple one-value mapping; use a function when combining or calculating from multiple sources.

## Add spring smoothing

`useSpring` can create a spring Motion value directly or follow another Motion value:

~~~jsx
const targetX = useMotionValue(0);
const smoothX = useSpring(targetX, { stiffness: 300, damping: 30 });
~~~

When targetX changes, smoothX follows it with spring motion. Use the source value for immediate derived feedback and the spring output for a softer physical response. A spring adds lag by design; do not smooth every value when the interface needs precise immediate feedback.

Calling `jump()` on a spring value intentionally breaks continuity and resets velocity. Use it for a reset that should happen immediately rather than gliding.

## Compose a visual relationship

One pointer or scroll source can drive position, opacity, rotation, and color:

~~~jsx
const x = useMotionValue(0);
const smoothX = useSpring(x);
const opacity = useTransform(x, [-160, 0, 160], [0.3, 1, 0.3]);
const rotate = useTransform(x, [-160, 160], [-10, 10]);
~~~

Attach the derived values to different style properties or components. Keep a single source of truth for the input, then derive presentation from it. This avoids repeated per-frame React state updates and keeps the relationships predictable.

`useMotionTemplate` can combine several Motion values into one CSS string, such as a filter or shadow. It uses JavaScript tagged-template syntax; keep the static CSS pieces and interpolated values format-compatible. Use it when the final property is a string, not when a plain numeric or color output is sufficient.

## Choose clamping and output types

| Need | Pattern |
| --- | --- |
| Map position into opacity | `useTransform(source, inputRange, outputRange)` |
| Combine multiple sources | `useTransform(() => calculation)` |
| Continue mapping outside endpoints | `{ clamp: false }` |
| Smooth a target value | `useSpring(source)` |
| Build a CSS string from values | `useMotionTemplate` |

Each interpolation range needs matching input and output lengths. For complex strings, each output should have compatible structure so Motion can interpolate corresponding numbers and colors. If output jumps unexpectedly, check range lengths, order, units, and whether clamping is intended.

## Common mistakes

- **Range lengths differ:** input and output arrays must contain the same number of stops.
- **Input stops are not monotonic:** use consistently increasing or decreasing input values.
- **Accidental unclamped growth:** clamp is on by default; opt out only when continued mapping is intentional.
- **Spring hides important feedback:** use the immediate source for exact readouts and the spring for soft visual response.
- **Derived React state per frame:** attach derived Motion values directly to Motion components.
- **Incompatible complex strings:** use matching units and string structure at each output stop.
- **Expensive transform function:** keep derivation lightweight and free of side effects.
- **Conflicting sources:** decide which Motion value owns the input and derive the others from it.

## Recap and practice

Use range mapping for a single source, transform functions for combinations, and springs for smoothing. Derived outputs can drive many styles without a React render per frame.

Practice with a horizontal input mapped to opacity, rotation, and color. Then wrap only the position output in a spring and compare which effects should react immediately versus smoothly.

Continue with [scroll-linked animations with useScroll](017-scroll-linked-animations-with-usescroll.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="015-motion-values-and-direct-updates.md">015 - Motion values and direct updates</a></td><td><a href="../qnas/016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values Q&amp;A</a></td><td><a href="../examples/016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values example</a></td><td><a href="017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll</a></td></tr></tbody>
</table>
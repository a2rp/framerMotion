# Keyframes and repeats

A single target is enough for a transition from the current value to one destination. A keyframe array describes several values that Motion should visit in order. Repeat options control whether the sequence runs again after reaching its end.

## Animate through keyframes

Pass an array for a property in an animation target:

~~~jsx
<motion.div
  animate={{ x: [0, 80, 0] }}
  transition={{ duration: 0.9, ease: "easeInOut" }}
/>
~~~

Motion moves through each x value in sequence. By default, keyframes are distributed evenly across the duration. A keyframe array does not need a separate component for every step.

You can animate multiple properties at once. Keep their keyframe arrays aligned when they should change at the same moments:

~~~jsx
<motion.div
  animate={{
    scale: [1, 1.12, 1],
    rotate: [0, 4, 0],
  }}
  transition={{ duration: 0.8, ease: "easeInOut" }}
/>
~~~

## Use the current value

Use `null` as the first keyframe to start from the current value instead of specifying a new one:

~~~jsx
<motion.div animate={{ x: [null, 100, 0] }} />
~~~

This is useful when a keyframe sequence can interrupt an animation that is already in progress. A `null` keyframe in the middle holds the previous keyframe value, which can avoid writing the same value twice.

Only use this behavior when the current value is the intended starting point. If a specific starting value is important, write it explicitly.

## Control keyframe timing

The `times` transition option sets the normalized progress position for each keyframe. Its values range from 0 at the start to 1 at the end:

~~~jsx
<motion.div
  animate={{ x: [0, 40, 160, 180] }}
  transition={{
    duration: 1.2,
    times: [0, 0.15, 0.8, 1],
  }}
/>
~~~

The keyframe values and `times` entries correspond by index. In this example the object pauses its progress near x 40 for much of the animation, then reaches x 160 and x 180 near the end. Use times only when evenly spaced keyframes do not express the intended pacing.

## Repeat an animation

A transition can repeat after its first iteration:

~~~jsx
<motion.div
  animate={{ rotate: 180 }}
  transition={{
    duration: 0.7,
    repeat: 2,
    repeatType: "reverse",
    repeatDelay: 0.2,
  }}
/>
~~~

`repeat` is the number of additional iterations. A value of 2 means the first play plus two repeats. `repeatType` controls how it plays again:

- `"loop"` restarts from the beginning.
- `"reverse"` alternates forward and backward playback.
- `"mirror"` alternates by switching the animation origin and target.

The default repeat type is loop. `repeatDelay` adds a pause between iterations. A negative `delay` can start partway through an animation, but this is a specialized timing tool and should be used only when the offset is deliberate.

## Keep loops intentional

Infinite repeats are useful for status indicators or ambient motion only when users can still understand the interface. They consume attention and can be uncomfortable, especially when they move large areas or continue without a clear purpose. Prefer finite repeats for feedback that should stop. Respect reduced-motion preferences and provide a static or simpler state when needed.

Do not use a looping animation to imply that work is progressing if the interface has no actual progress data. Label loading and status states with text or semantics as appropriate.

## Per-property timing

Keyframes can use a shared transition or per-property transitions. Use a shared duration and easing when properties move together. If opacity should fade quickly while position follows a longer sequence, assign separate settings only where that difference is clear.

## Common mistakes

- **Mismatched keyframe timing.** The `times` array should describe the sequence's keyframe positions.
- **Unexpected start point.** Use `null` only when the current value should supply the first frame.
- **Repeat count confusion.** `repeat: 1` means the first iteration plus one repeat.
- **Infinite loop with no stop or purpose.** Loops should have clear interface meaning and respect reduced motion.
- **Using repeat to sequence unrelated UI states.** Use variants or imperative sequences when the interaction is stateful and coordinated.
- **Making multiple arrays hard to follow.** Keep arrays short and align their indices to meaningful moments.
- **Animating the wrong property.** Confirm the target and property before tuning timing.

## Recap and practice

- Keyframe arrays animate a property through ordered values.
- Keyframes are evenly distributed by default.
- `null` can use the current value.
- `times` sets normalized positions for keyframes.
- `repeat` counts additional iterations; `repeatType` controls direction or restart behavior.
- Infinite loops require a deliberate reason and an accessible alternative.

Practice by animating a badge through three scale values. First use evenly spaced keyframes, then add `times` to make the badge hold at its largest size before returning.

Continue with [variants and reusable states](007-variants-and-reusable-states.md).

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="005-transitions-and-springs.md">005 - Transitions and springs</a></td><td><a href="../qnas/006-keyframes-and-repeats.md">006 - Keyframes and repeats Q&amp;A</a></td><td><a href="../examples/006-keyframes-and-repeats.md">006 - Keyframes and repeats example</a></td><td><a href="007-variants-and-reusable-states.md">007 - Variants and reusable states</a></td></tr>
  </tbody>
</table>

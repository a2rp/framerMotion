# Transitions and springs

An animation target says where a value should end. A transition says how the value should get there. Motion supports time-based tweens and spring-based motion, along with per-property configuration. Choose the transition from the role of the interaction instead of choosing one animation style for the whole interface.

## Tween transitions

A tween follows a duration and easing curve:

~~~jsx
<motion.div
  animate={{ opacity: 1, x: 0 }}
  transition={{ type: "tween", duration: 0.3, ease: "easeOut" }}
/>
~~~

`duration` is measured in seconds. `ease` controls the rate of progress across that duration. A tween is useful when the timing should be deliberate and repeatable, such as a short fade or a coordinated page transition.

Easing affects perceived acceleration. A linear curve moves at a constant rate, while an ease-in or ease-out curve changes speed near the start or end. Keep the chosen curve consistent with what the element is doing.

## Spring transitions

A spring responds to a target using spring parameters rather than only a fixed duration:

~~~jsx
<motion.div
  animate={{ x: 120 }}
  transition={{ type: "spring", stiffness: 260, damping: 24, mass: 1 }}
/>
~~~

- `stiffness` influences the spring's pull toward the target. Higher stiffness generally responds more sharply.
- `damping` reduces oscillation. Higher damping generally settles with less bounce.
- `mass` affects the perceived weight and response of the moving value.

A spring is useful for direct manipulation and state changes where a responsive, physical feel makes sense. More bounce is not automatically more natural. Tune parameters in the size and context of the actual component.

Motion versions may also provide duration-oriented spring controls such as `visualDuration` or `bounce`. Prefer the parameter model documented for the installed release and avoid mixing controls whose interactions are unclear.

## Choose based on intent

Use a tween when a duration should be predictable. Use a spring when the motion should respond to a target with a settling feel. A button press often needs only a short tween or a small scale target. A draggable card released by a user can benefit from spring or inertia behavior. A subtle fade might be better as a tween.

Do not choose a spring merely because it appears more sophisticated. If the element must reach a particular endpoint by a deadline, a time-based tween can be easier to reason about.

## Configure properties separately

A transition can use default settings for all animated properties and override one value:

~~~jsx
<motion.div
  animate={{ x: 120, opacity: 1 }}
  transition={{
    default: { type: "spring", stiffness: 250, damping: 26 },
    opacity: { type: "tween", duration: 0.18, ease: "linear" },
  }}
/>
~~~

This lets position settle with a spring while opacity changes at a fixed pace. Per-property transitions can make complex motion coherent, but too many independent timings make an interface hard to understand. Start with one shared transition and add exceptions only when they improve the result.

## Delays and higher-level defaults

A transition can include options such as delay or repeat behavior, covered in the [keyframes and repeats chapter](006-keyframes-and-repeats.md). Shared fallback transitions can be provided through `MotionConfig`; use local values when a particular interaction needs a different feel. The [global configuration chapter](023-global-configuration-with-motionconfig.md) explains that relationship.

## Tune by observation

When a transition feels wrong:

1. Confirm the target is correct before changing timing.
2. Pick tween or spring based on the intended behavior.
3. Change one timing or spring parameter at a time.
4. Observe it at the real element size and in the surrounding layout.
5. Check the transition from both directions if the target can reverse.
6. Remove decorative motion that does not improve feedback or comprehension.

A spring can look different when the distance or starting velocity changes. A tween duration stays fixed, but the distance can still affect the apparent speed. Always tune with the actual interaction, not only a tiny isolated box.

## Common mistakes

- **Too much bounce.** A spring that oscillates strongly can distract and slow the interface.
- **Using a long duration for a small control.** Small feedback should usually not block the next action.
- **Mixing defaults without understanding precedence.** A local transition can replace a broader default. See the configuration lesson for inheritance rules.
- **Applying one transition to every property.** A color fade and a springing position can need different timing.
- **Changing several parameters at once.** It becomes difficult to tell which change helped.
- **Treating a spring as a fixed-duration tween.** Spring settling time depends on its configuration and motion.
- **Using motion to cover incorrect state logic.** Verify the target and event behavior first.

## Recap and practice

- A target chooses destination values; a transition controls interpolation.
- Tweens use duration and easing.
- Springs use physical or visual spring controls supported by the installed version.
- Per-property transitions can combine different behaviors.
- Tune one parameter at a time and judge the result in context.

Practice by implementing the same movement once with a tween and once with a spring. Ask which behavior communicates a state change more clearly, then simplify the slower or more decorative version.

Continue with [keyframes and repeats](006-keyframes-and-repeats.md).

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="004-initial-and-animate-states.md">004 - Initial and animate states</a></td><td><a href="../qnas/005-transitions-and-springs.md">005 - Transitions and springs Q&amp;A</a></td><td><a href="../examples/005-transitions-and-springs.md">005 - Transitions and springs example</a></td><td><a href="006-keyframes-and-repeats.md">006 - Keyframes and repeats</a></td></tr>
  </tbody>
</table>

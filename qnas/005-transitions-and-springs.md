# Transitions and springs Q&A

## 1. What is a transition?

A transition controls how Motion interpolates between target values. It can specify a tween, spring, duration, easing, delay, or per-property options.

## 2. When should I use a tween?

Use a tween when the duration and easing need to be predictable, such as a short fade or a transition that must fit a known sequence.

## 3. When should I use a spring?

Use a spring when the interaction should respond naturally to a target or direct manipulation. Tune its spring parameters so it settles quickly and does not bounce without purpose.

## 4. What does `duration` mean?

For a tween, duration is the time in seconds for the animation. Spring timing depends on the spring model and its configuration.

## 5. What do `stiffness` and `damping` control?

Stiffness changes the spring's pull toward its target. Damping reduces oscillation. Increasing damping often makes a spring settle with less bounce.

## 6. What does `mass` do?

Mass changes the perceived weight and response of a spring. Its effect depends on the other spring parameters, so adjust it in small steps.

## 7. Should I set `type: "tween"` explicitly?

It can make intent clear when you want duration and easing behavior. If you rely on defaults, confirm those defaults for the installed Motion version and keep code consistent.

## 8. Can opacity and position use different transitions?

Yes. Use a shared default transition and a property-specific transition when the visual properties need distinct timing. Keep overrides limited so the result still feels like one interaction.

## 9. What if the transition prop changes with React state?

React can supply different transition objects for different modes. Ensure each object is complete and intentional. A transition change controls how future target changes are handled; it does not itself choose a new target.

## 10. Why does a spring not stop at a fixed time?

Spring settling depends on its parameters and motion. If a fixed duration matters more than physical response, use a tween.

## 11. Are all spring options supported in every version?

No. The API can evolve. Parameters such as stiffness, damping, mass, and version-specific visual-duration controls should be checked against the installed Motion release.

## 12. Does a longer distance need a longer tween?

Not necessarily, but the same duration over a longer distance can appear faster. Tune the actual distance and surrounding content rather than relying on an isolated value.

## 13. Why does the transition seem to snap?

Check whether the target values changed, whether the component remounted, whether a higher-level or local transition overrides the expected config, and whether the property value types are compatible.

## 14. Where do delays and repeats fit?

They are transition options, but use them intentionally. The next lesson covers keyframe sequences and repeat behavior.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="004-initial-and-animate-states.md">004 - Initial and animate states Q&amp;A</a></td><td><a href="../lessons/005-transitions-and-springs.md">005 - Transitions and springs lesson</a></td><td><a href="../examples/005-transitions-and-springs.md">005 - Transitions and springs example</a></td><td><a href="006-keyframes-and-repeats.md">006 - Keyframes and repeats Q&amp;A</a></td></tr>
  </tbody>
</table>

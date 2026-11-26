# Keyframes and repeats Q&A

## 1. What is a keyframe array?

It is an ordered list of values for one animated property. Motion animates through the listed values during the transition.

## 2. Are keyframes evenly timed by default?

Yes. Motion distributes them evenly across the transition unless `times` sets different normalized positions.

## 3. What does `null` mean in a keyframe array?

A leading `null` can use the current value as the starting keyframe. A `null` after another value holds the previous value at that point. Use it only when the current or previous value is the desired one.

## 4. What is `times`?

It is an array of normalized progress positions for keyframes, from 0 at the start to 1 at the end. Each position corresponds to a keyframe at the same index.

## 5. Does `repeat: 1` mean one play total?

No. It means one additional repeat after the first iteration, for two iterations total.

## 6. What is the default `repeatType`?

The documented default is `"loop"`, which restarts from the beginning. `"reverse"` alternates direction. `"mirror"` alternates by switching the origin and target.

## 7. What does `repeatDelay` do?

It adds a delay in seconds between repeated iterations. It does not add a pause before the first iteration.

## 8. Can I repeat forever?

Use an infinite repeat only when the behavior needs to loop continuously. Infinite decoration can distract and should respect reduced-motion preferences. A finite repeat is often better for feedback.

## 9. Can keyframe arrays animate more than one property?

Yes. Put an array for each property in the target object. If values should align, use arrays with corresponding keyframes and timing positions.

## 10. Why did my keyframe sequence start from an unexpected value?

Check whether the first value is `null`, whether another animation was active, and whether the element remounted. Specify the initial value explicitly when the exact start matters.

## 11. How do I pause on one keyframe?

Use `times` to place keyframes at different points, including repeated values when a hold is required. A middle `null` can hold the previous value. Keep the number and order of timing positions aligned with the keyframes.

## 12. Should I use repeats to coordinate a menu?

Usually no. A menu is stateful and often has child sequencing. Use variants and orchestration to coordinate its states. Repeats are for repeating one animation, not for modeling application state.

## 13. How do I stop an infinite loop?

Drive the animation from a meaningful state or switch to a non-looping target. Do not leave a decorative loop running after its interface state is no longer relevant.

## 14. What should I read next?

Continue with [variants and reusable states](../lessons/007-variants-and-reusable-states.md) to coordinate named states across related components.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="005-transitions-and-springs.md">005 - Transitions and springs Q&amp;A</a></td><td><a href="../lessons/006-keyframes-and-repeats.md">006 - Keyframes and repeats lesson</a></td><td><a href="../examples/006-keyframes-and-repeats.md">006 - Keyframes and repeats example</a></td><td><a href="007-variants-and-reusable-states.md">007 - Variants and reusable states Q&amp;A</a></td></tr>
  </tbody>
</table>

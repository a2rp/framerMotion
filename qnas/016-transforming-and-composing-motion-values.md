# Transforming and composing motion values Q&A

## 1. What does useTransform return?

It returns a new Motion value whose output is derived from one or more source Motion values.

## 2. What is range mapping?

It maps corresponding positions in input and output ranges. For example, x values from negative to positive can map to opacity values that rise in the center and fall at the edges.

## 3. Must the input and output ranges have the same length?

Yes. Each input stop corresponds to one output stop, so the arrays must have equal lengths.

## 4. What rules apply to input ranges?

Input values must form a consistently increasing or decreasing numeric sequence. Avoid unordered stops.

## 5. Are output values limited to numbers?

No. Output can include compatible numbers, units, colors, and complex strings that Motion can interpolate.

## 6. What does clamp do?

Clamping is enabled by default, so values outside the input range use the nearest endpoint output. Set `clamp: false` when the mapping should continue past its endpoints.

## 7. When should I use the function form of useTransform?

Use it when you need to combine multiple source values or calculate a derived value. Motion subscribes to Motion values read through get() inside the function.

## 8. What is the difference between a transform and a spring?

A transform derives a value from its source. A spring follows a target with physical smoothing and lag. They can be composed, such as spring-smoothing a transformed target.

## 9. Can useSpring follow another Motion value?

Yes. Pass the source Motion value to useSpring. Updates to the source become targets for the spring output.

## 10. When should I use jump on a spring value?

Use jump when a change should be immediate and should break continuity, such as a reset. It stops the spring effect and resets velocity rather than animating to the new value.

## 11. Why might my output keep growing beyond the last range point?

The mapping may have `clamp: false`. Remove that option if the output should stay at the endpoint.

## 12. What does useMotionTemplate do?

It combines static text and Motion values into one derived string, useful for CSS properties such as filters or shadows. Use the tagged template form and keep string structures compatible.

## 13. Should I copy every derived value into React state?

Usually not. Attach derived Motion values directly to Motion styles. Copy a rounded or sampled value into React only when React-rendered text or application logic needs it.

## 14. Why does a color or complex string interpolate strangely?

Check that output stops use compatible units and the same string structure, and verify that the input and output ranges line up.

## 15. Can a transform function perform side effects?

Keep it pure. It can be recalculated as its input values update, so logging, network requests, or state changes inside it can run repeatedly and unexpectedly.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="015-motion-values-and-direct-updates.md">015 - Motion values and direct updates Q&amp;A</a></td><td><a href="../lessons/016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values lesson</a></td><td><a href="../examples/016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values example</a></td><td><a href="017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll Q&amp;A</a></td></tr></tbody>
</table>
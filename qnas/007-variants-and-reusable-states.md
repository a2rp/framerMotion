# Variants and reusable states Q&A

## 1. What is a Motion variant?

A variant is a named animation target. It groups values under a label such as `open` or `selected`, so components can select that state without repeating the target object inline.

## 2. How does a component select a variant?

Pass the label to a state prop such as `initial="closed"`, `animate="open"`, `whileHover="hover"`, or `exit="closed"`. The component must also receive a `variants` object that defines that label.

## 3. What is the difference between `initial` and `animate`?

`initial` describes the starting visual target on mount. `animate` is the persistent target after mounting and when application state changes. Use `initial={false}` to render directly at the animate target without an enter animation.

## 4. Can a parent variant coordinate child components?

Yes. Motion can pass the active label through motion descendants. Each participating child defines a matching label with its own target. This keeps a group under one state decision while allowing each element to animate different properties.

## 5. Must every child declare every parent label?

No. A child only needs the labels it uses. A missing matching target means that child has no target for that label. Keep labels consistent where coordinated behavior is intended to avoid silent mismatches.

## 6. Do variants replace React state?

No. React state or props should express application facts, such as whether a disclosure is open. The selected variant is the presentation of that fact. Keep semantics such as `aria-expanded` tied to the same React value.

## 7. Can a variant include a transition?

Yes. Put a `transition` next to the target values for a label when that state needs distinct timing. A top-level transition prop is useful when the timing should apply to multiple animation targets.

## 8. When should I use an inline target instead of a variant?

Use an inline target for a single simple animation with no meaningful state reuse. Use variants when labels improve readability, several states belong to one component, or parent and descendants should coordinate.

## 9. Why does a child not follow the parent's `animate` label?

Check that the child is a Motion component, has a matching variant name, and is part of the parent's motion tree. Also check whether the child has its own `animate` prop that intentionally gives it independent state ownership.

## 10. Can variant targets depend on a value?

Yes. A dynamic variant can be a function and can use a value supplied with `custom`. This is useful when the chosen target needs direction or other data. Use stable, explicit inputs rather than reading unrelated mutable state inside the animation definition.

## 11. Should an element be hidden only by animating opacity to zero?

Not when it contains controls that must not receive focus while visually hidden. Opacity does not remove an element from layout or keyboard navigation. Coordinate visual animation with DOM presence, visibility, pointer behavior, and accessibility semantics.

## 12. How should I name variant labels?

Use names that describe the interface state, such as `expanded`, `selected`, or `error`. Avoid labels that merely describe one CSS value. Meaningful names make animation code easier to compare with the React state and accessibility attributes.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="006-keyframes-and-repeats.md">006 - Keyframes and repeats Q&amp;A</a></td><td><a href="../lessons/007-variants-and-reusable-states.md">007 - Variants and reusable states lesson</a></td><td><a href="../examples/007-variants-and-reusable-states.md">007 - Variants and reusable states example</a></td><td><a href="008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger Q&amp;A</a></td></tr>
  </tbody>
</table>
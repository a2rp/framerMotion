# Initial and animate states Q&A

## 1. What is the difference between `initial` and `animate`?

`initial` describes the first visual target. `animate` describes the target Motion should reach. React props or state can change the `animate` target later.

## 2. Does `initial` run every time a component renders?

No. It describes the component's initial state when it enters the React tree. A normal rerender with the same component identity does not remount it.

## 3. What does `initial={false}` do?

It tells Motion to render the first frame at the `animate` target instead of animating from a separate `initial` target. Later changes to `animate` can still transition.

## 4. When should I set `initial={false}`?

Use it when the destination is the correct first visual state and an entrance from a fabricated starting point would be distracting. It can also help keep server-rendered first output consistent with the intended initial interface.

## 5. How do I animate when a React boolean changes?

Use the boolean to choose an `animate` target object. When React renders the new target, Motion interpolates from the current value. Keep the boolean as React state if it affects interface behavior.

## 6. Can I use an array as an animation target?

Motion supports keyframe arrays for supported properties, but that is a sequence of values rather than a React state object. See the keyframes chapter for sequencing, repeats, and timing.

## 7. Why does opacity zero not hide an element from keyboard users?

Opacity only changes appearance. The element can still receive focus and remain in the accessibility tree. Use appropriate semantic state or remove the element when it is unavailable. If it should animate out before removal, use `AnimatePresence` and `exit`.

## 8. Why is my `exit` target ignored?

React removes the element immediately unless a parent `AnimatePresence` boundary observes its removal. The child also needs a stable identity and an `exit` target.

## 9. Why does the animation play again unexpectedly?

Check whether the component's `key` changes, whether its type or tree position changes, and whether it is being conditionally remounted. A new React identity starts a new initial lifecycle.

## 10. Can Motion animate from CSS styles?

Motion can animate supported values that it can read and interpolate, but behavior depends on the property and value formats. Define explicit targets when the start state matters and inspect computed styles when a value does not behave as expected.

## 11. Should each animation frame go into React state?

No. React state should represent meaningful application state. Motion updates its animated values without requiring React to render every frame. Use Motion values for continuously changing visual data.

## 12. Is the initial target visible in server-rendered output?

Motion can render its initial visual state into server-generated markup. Choose `initial` or `initial={false}` based on the intended first paint and keep the server and client output consistent.

## 13. What if my target object has not changed?

Motion responds to target changes. If the target is still the same, there may not be a new animation to play. Use a deliberate state change or an appropriate control API if replaying is part of the interaction.

## 14. Where should I go next?

Continue with [transitions and springs](../lessons/005-transitions-and-springs.md) to choose timing and interpolation behavior.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="003-motion-components-and-props.md">003 - Motion components and props Q&amp;A</a></td><td><a href="../lessons/004-initial-and-animate-states.md">004 - Initial and animate states lesson</a></td><td><a href="../examples/004-initial-and-animate-states.md">004 - Initial and animate states example</a></td><td><a href="005-transitions-and-springs.md">005 - Transitions and springs Q&amp;A</a></td></tr>
  </tbody>
</table>

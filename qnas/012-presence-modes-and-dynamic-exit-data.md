# Presence modes and dynamic exit data Q&A

## 1. What does the AnimatePresence mode control?

It controls how entering and exiting children relate to each other and to layout. It is a presence strategy, not simply a transition duration.

## 2. What does `mode="sync"` do?

It is the default. Entering and exiting children animate at the same time. If their layouts overlap, manage that overlap or use another mode.

## 3. When should I use `mode="wait"`?

Use it when one new child should wait until the old child exits, such as a one-at-a-time slideshow. Wait mode supports only one child at a time.

## 4. What does `mode="popLayout"` do?

It removes exiting children from normal layout immediately so surrounding elements can reflow while the old child continues animating. It is useful with layout animation in lists.

## 5. Why might a custom child fail with popLayout?

The immediate custom component must forward the provided ref to the DOM element that should be popped from layout. Without that ref, Motion cannot apply the layout behavior correctly.

## 6. Why is custom presence data needed?

An exiting component has been removed from React's tree and no longer receives updated props. AnimatePresence's custom value can pass fresh data to its exit variants.

## 7. How does a function variant use custom data?

The variant can be a function whose argument receives the custom value. Use it to select an exit direction or another explicit target.

## 8. Can I read presence data inside a descendant?

Yes. The usePresenceData hook reads data from the nearest presence context. Prefer a function variant when only the target itself needs the value.

## 9. What is the difference between custom data and component props?

Ordinary props stop updating once the component is removed. Presence custom data is provided through AnimatePresence for use during exit.

## 10. What does propagate do?

It allows nested AnimatePresence children to run their exit animations when their containing presence boundary exits. Enable it only when those nested exits should be triggered.

## 11. What is onExitComplete for?

It runs after all exiting nodes have completed. It can coordinate a visual follow-up, but does not prove that unrelated application or server work succeeded.

## 12. Can wait mode animate a list of several children in sequence?

Wait mode supports a single child at a time. For lists, use another mode or explicitly model which child is present.

## 13. Does popLayout make exiting content disappear instantly?

No. It removes the exiting element from normal layout so siblings can reflow, while its exit animation can continue visually.

## 14. How should presence modes respond to reduced motion?

Simplify or shorten movement. Keep the content order and interaction clear even if the visual transition is removed.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations Q&amp;A</a></td><td><a href="../lessons/012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data lesson</a></td><td><a href="../examples/012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data example</a></td><td><a href="013-layout-animations.md">013 - Layout animations Q&amp;A</a></td></tr></tbody>
</table>
# Presence modes and dynamic exit data

§AnimatePresence§ can let entering and exiting elements overlap, sequence them, or remove an exiting element from layout immediately. Its §mode§ prop selects this relationship. It also provides data to an exiting child whose React props are already frozen.

## Presence modes

- §"sync"§ is the default. Entering and exiting children animate at the same time. Use it when overlap is acceptable or when you manage layout yourself.
- §"wait"§ holds the entering child until the exiting child finishes. It supports one child at a time and is useful for a slideshow or step-by-step panel.
- §"popLayout"§ removes the exiting child from normal layout immediately so siblings can reflow while it animates away. It works well with layout animation in lists. The immediate child custom component must forward its ref to the DOM element being popped.

Choose the mode based on the visual relationship and layout behavior. A mode is not just an easing option; it changes how children are presented while entering and exiting.

## Dynamic exit targets

When React removes an element, it cannot pass new props to that element. If a slideshow changes direction in the same update that replaces a slide, the old slide still needs that new direction to choose its exit target. Pass the value through AnimatePresence's §custom§ prop and define a function variant:

~~~jsx
const slideVariants = {
  enter: (direction) => ({ x: direction > 0 ? 40 : -40, opacity: 0 }),
  center: { x: 0, opacity: 1 },
  exit: (direction) => ({ x: direction > 0 ? -40 : 40, opacity: 0 }),
};

<AnimatePresence custom={direction} mode="wait">
  <motion.article
    key={slide.id}
    variants={slideVariants}
    initial="enter"
    animate="center"
    exit="exit"
  />
</AnimatePresence>
~~~

The custom value can be used by variant functions during the exit. Keep the data small and explicit, such as direction, a selected edge, or a transition mode. It is presence data for animation, not a replacement for application state.

The §usePresenceData§ hook can read the value from inside a descendant when it is needed outside a dynamic variant function. Use it only where the value is needed; passing it through the variant function is simpler for a direct target.

## Nested exits and completion

A nested AnimatePresence boundary can prevent descendant exit animations from firing when an outer component leaves. Set §propagate§ on the nested manager when its children should also run their exit animations as the boundary exits.

Use §onExitComplete§ when the application needs a callback after all exiting nodes complete. Keep application state authoritative; an animation callback is useful for presentation coordination, but should not be the only way to persist important data or complete a network operation.

## Choose carefully

| Need | Mode or prop |
| --- | --- |
| Enter and exit at the same time | §mode="sync"§ |
| One incoming child waits for outgoing child | §mode="wait"§ |
| Exiting list item leaves layout before fade ends | §mode="popLayout"§ |
| Exit target depends on newly selected direction | §custom§ with function variants |
| Nested descendants must also exit | §propagate§ |
| Run work after all exits | §onExitComplete§ |

## Common mistakes and limits

- §wait§ is designed for a single child, not a list of simultaneous children.
- §popLayout§ relies on positioning behavior. Give the parent a deliberate positioning context and forward refs from immediate custom children.
- A custom variant only receives useful dynamic data if the value is passed through presence context or the appropriate component prop.
- Exiting props are otherwise frozen; changing React state does not update a removed component's ordinary props.
- Do not use §onExitComplete§ as proof that a server save succeeded.
- Nested presence managers can intentionally isolate exits. Enable propagation only when the nested exits should run.
- Shorten or simplify movement for reduced-motion preferences.

## Recap and practice

Use §sync§ for concurrent transitions, §wait§ for one-child sequencing, and §popLayout§ when an exiting item should stop affecting sibling layout. Use §custom§ when an exit target needs fresh data after React has removed the child.

Practice a directional slideshow. Pass the direction to AnimatePresence, use a function variant for enter and exit, and compare §wait§ with §sync§. Then place an exiting item inside a nested presence manager and test propagation.

Continue with [layout animations](013-layout-animations.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations</a></td><td><a href="../qnas/012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data Q&amp;A</a></td><td><a href="../examples/012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data example</a></td><td><a href="013-layout-animations.md">013 - Layout animations</a></td></tr></tbody>
</table>
# AnimatePresence and exit animations

React removes components as soon as they leave the rendered tree. `AnimatePresence` detects that a direct child is leaving and retains it long enough for its `exit` target to finish. It is the standard way to animate a component out of a React interface.

## Conditional exit

Keep AnimatePresence mounted and put the conditional inside it:

~~~jsx
import { AnimatePresence, motion } from "motion/react";

<AnimatePresence>
  {isOpen && (
    <motion.div
      key="panel"
      initial={{ opacity: 0, y: 8 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -8 }}
    >
      Panel content
    </motion.div>
  )}
</AnimatePresence>
~~~

If the conditional wraps AnimatePresence itself, the manager disappears at the same time as the child and cannot complete the exit. Give each direct child a stable unique key.

The exit prop accepts a target object or a variant label. Motion descendants may also define exit targets. Use variants when a parent coordinates child states or when several elements share a state map.

## Lists and identity

When an array item is removed, the keyed child is removed from the React tree. AnimatePresence matches its key with the previous render and runs its exit target:

~~~jsx
<AnimatePresence>
  {items.map((item) => (
    <motion.li key={item.id} exit={{ opacity: 0, x: 12 }}>
      {item.label}
    </motion.li>
  ))}
</AnimatePresence>
~~~

Use a stable record ID. Array indices can be reused after deletion or reordering, making React associate a mounted instance with a different record. Random keys are also wrong because they change identity on every render.

If a different content item replaces the old one in a single child slot, change the child's key to represent the new identity. React then treats the old child as leaving and the new child as entering.

## Initial behavior and frozen props

By default, children present on first render can animate from their initial target. Set `initial={false}` on AnimatePresence to suppress those initial animations, while keeping entry animations for children added later.

Once removed, an exiting component no longer receives updated props from React. If its final direction or value must reflect the same update that removed it, use the custom presence data described in the next chapter.

## Choosing useful exits

Keep exits brief. An exit should clarify dismissal, navigation, or replacement and should not make the interface feel blocked. For dialogs and other interactive regions, also decide where focus goes and whether the departing controls can still be used. Presence animation is visual behavior; it does not manage dialog focus.

| Situation | Presence design |
| --- | --- |
| Conditional panel | Keep wrapper mounted; conditionally render a keyed Motion child |
| Dismissible list item | Stable ID key and an exit target |
| Replace one slide | Change the slide key |
| Prevent first-paint entry | Set initial false on AnimatePresence |
| Exit needs current direction | Use custom presence data |

## Common mistakes

- Removing AnimatePresence together with its child.
- Missing or unstable keys on direct children.
- Using a map index where records can move or be removed.
- Expecting changed text with the same key to count as a component exit.
- Expecting exiting components to receive new props.
- Leaving controls active in a long visual exit.
- Forgetting focus policy when a dialog leaves.
- Treating an exit animation as application state.

## Recap and practice

AnimatePresence retains removed keyed children until their exit animation completes. Keep the wrapper mounted, use stable identity, and choose initial behavior intentionally.

Practice by building dismissible notices. Remove an item from React state, observe its exit, then reorder the list and compare stable IDs with array indices. Consider focus and announcements when the removed content contains interactive controls.

Continue with [presence modes and dynamic exit data](012-presence-modes-and-dynamic-exit-data.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints</a></td><td><a href="../qnas/011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations Q&amp;A</a></td><td><a href="../examples/011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations example</a></td><td><a href="012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data</a></td></tr></tbody>
</table>
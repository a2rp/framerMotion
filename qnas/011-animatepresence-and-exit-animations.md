# AnimatePresence and exit animations Q&A

## 1. Why is the exit target skipped when React removes a component?

React normally removes it immediately. `AnimatePresence` must remain mounted around the conditional child to retain the removed component until the exit completes.

## 2. Where does the conditional expression belong?

Inside AnimatePresence. Do not place the whole presence manager inside a condition that unmounts it.

## 3. Do direct children need keys?

Yes. Stable unique keys let AnimatePresence identify which component was removed or replaced.

## 4. Why are array indices poor keys for animated lists?

Deletion and reordering can reuse an index for a different record. React may then associate the wrong component instance with an item and presence animation becomes incorrect.

## 5. Can exit select a variant?

Yes. The exit prop accepts a target object or a variant label. Descendants can define matching variant targets too.

## 6. Do props keep updating while a child exits?

No. Once removed from the React render, the exiting child does not receive new props. Use custom presence data if the exit needs a value from the removal update.

## 7. What does initial false on AnimatePresence do?

It suppresses initial animations for children present on the first render. New children added later can still animate in.

## 8. Does changing text trigger an exit?

No, if the same component remains rendered with the same key. React must remove or replace the child's identity.

## 9. What happens with nested presence managers?

A nested AnimatePresence can establish a boundary. A parent removal does not necessarily trigger nested child exits unless propagation is enabled.

## 10. Can a list animate several items leaving?

Yes. AnimatePresence can track multiple keyed children. The presence mode determines how entering and exiting children are sequenced; the next chapter explains the modes.

## 11. Does AnimatePresence handle dialog focus?

No. It keeps a component for its exit animation. Dialog focus entry, focus return, and keyboard behavior must be handled separately.

## 12. Can an exiting button still be used?

It may remain rendered during the exit. Keep exits short and make an explicit interaction policy for controls that are disappearing.

## 13. Can descendants of a removed component have exit targets?

Yes. Motion descendants can animate too. Check nested AnimatePresence boundaries and propagation when a nested group does not exit.

## 14. What should I check when exit never runs?

Keep AnimatePresence mounted, put the conditional inside it, use a stable key, and ensure the child with the exit target is a Motion component.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints Q&amp;A</a></td><td><a href="../lessons/011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations lesson</a></td><td><a href="../examples/011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations example</a></td><td><a href="012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data Q&amp;A</a></td></tr></tbody>
</table>
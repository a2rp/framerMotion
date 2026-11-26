# Pan, drag, and constraints Q&A

## 1. How are pan and drag different?

Pan recognizes pointer movement and reports it through callbacks. Drag also applies pointer movement to the motion component's x or y position.

## 2. Which callbacks are available for pan?

Use lifecycle callbacks such as `onPanStart`, `onPan`, and `onPanEnd`. They receive a pointer event and gesture information including point, offset, and velocity.

## 3. How do I limit movement to one axis?

Set `drag="x"` for horizontal movement or `drag="y"` for vertical movement. This is clearer than allowing both axes and manually resetting one.

## 4. What does an object passed to `dragConstraints` mean?

It defines pixel bounds relative to the draggable element's origin, such as left, right, top, and bottom limits.

## 5. How do ref-based constraints work?

Pass a React ref pointing to the container. Motion measures the container and draggable element and constrains the movement to that measured area.

## 6. What does `dragElastic` control?

It controls how far the element can move beyond its constraints before returning. Zero removes elastic overshoot; larger values permit more stretch.

## 7. What does `dragMomentum` control?

It controls whether the pointer's release velocity continues into an inertia animation. Set it to false for predictable stopping behavior.

## 8. When should I use `dragTransition`?

Use it to customize the release inertia and bounce when momentum is enabled. Tune it for the perceived weight of the surface, not as a replacement for constraints.

## 9. Why might dragging on a phone scroll the page?

The browser may interpret touch movement as scrolling. Set an appropriate `touch-action` value on the gesture surface, limited to the axis or region that needs it.

## 10. How can I start dragging from a separate handle?

Create controls with `useDragControls`, pass them to the draggable element, and call `controls.start(event)` from the handle's `onPointerDown`. Use `dragListener={false}` to disable the draggable's own start listener.

## 11. Is a custom drag interaction keyboard-accessible by default?

No. Dragging is a pointer interaction. If moving the item is meaningful, add keyboard-operable controls such as nudge buttons or a range input that accomplishes the same task.

## 12. Should I update React state on every drag frame?

Only if React needs every sample. For smooth visual updates, Motion's own values are optimized for frame updates. You can use drag callbacks to update React state at meaningful boundaries such as drag end.

## 13. Is Motion drag the browser's native drag-and-drop API?

No. Motion moves the element visually. Use native drag-and-drop when dragging transfers data, files, or payloads between drop targets.

## 14. How do I check whether ref constraints are correct?

Confirm the ref is on the intended container, both elements have measurable dimensions, and the container has the expected size at the current viewport. Recheck when responsive layout changes dimensions.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures Q&amp;A</a></td><td><a href="../lessons/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints lesson</a></td><td><a href="../examples/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints example</a></td><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations Q&amp;A</a></td></tr></tbody>
</table>
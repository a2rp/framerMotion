# Pan, drag, and constraints

Pan and drag both begin with pointer movement, but they solve different problems. Pan recognizes movement and reports pointer information; drag also applies that movement to a Motion component's position. Use pan for a gesture that controls other state, and drag when the element itself should move.

## Pan callbacks

A motion component can respond to pan lifecycle callbacks such as `onPanStart`, `onPan`, and `onPanEnd`. The callback receives the pointer event and an information object with values such as point, offset, and velocity.

~~~jsx
<motion.div
  onPan={(event, info) => {
    console.log(info.offset.x, info.offset.y);
  }}
/>
~~~

Pan is recognized after the pointer travels beyond a small movement threshold. For touch input, set an appropriate CSS `touch-action` value so browser scrolling does not take over the axis you intend to handle. Do not disable page scrolling globally for a small gesture region.

## Drag and axis control

Set `drag` to allow movement on both axes, or use `drag="x"` and `drag="y"` to limit the gesture. Motion applies movement through transforms, which generally avoids repeatedly changing layout.

~~~jsx
<motion.div drag="x" dragMomentum={false} />
~~~

Drag is appropriate for a draggable card, slider thumb, or movable item. It is not a replacement for the browser's native drag-and-drop API when the task is transferring files or data between drop targets.

## Define constraints

Pixel constraints limit movement relative to the element's origin:

~~~jsx
<motion.div
  drag="x"
  dragConstraints={{ left: -120, right: 120 }}
/>
~~~

For a responsive containment area, pass a React ref to the container as `dragConstraints`. Motion measures the container and draggable bounds and keeps the item within that region. Make the container's geometry meaningful and ensure the draggable element has a real rendered size.

Constraints are measured from layout. If the container or draggable element changes size, check the result at the new dimensions. A ref-based constraint is easier to keep responsive than recalculating pixel bounds from the viewport.

## Elasticity and release behavior

By default, movement can stretch beyond the constraint and spring back. `dragElastic` controls how much movement is allowed beyond an edge: zero prevents elastic overshoot; a larger value permits more. Use a small amount for a soft boundary, or disable elasticity for strict containment.

`dragMomentum` controls whether pointer velocity continues into an inertia animation on release. Turning it off makes position easier to predict. When momentum is enabled, `dragTransition` can customize the inertia and bounce behavior. Use these settings to match the surface and task, not just to make movement dramatic.

Callbacks such as `onDragStart`, `onDrag`, and `onDragEnd` can synchronize a drag with application logic. The info object includes the pointer's movement and velocity. Avoid updating React state on every drag frame unless that state is needed by React; Motion values are better for frame-by-frame visual values.

## Start dragging from a handle

`useDragControls` can start a drag from a separate pointer target. Call `controls.start(event)` from its pointer-down handler and pass the controls into the draggable component. Set `dragListener={false}` when the draggable element itself must not initiate a drag.

A drag handle on touch should set `touch-action: none` so the browser does not interpret the same touch as page scrolling. Add a visible handle and instructions. A custom drag interaction is not automatically a complete keyboard interaction: provide buttons, a range input, or another keyboard-accessible way to accomplish the same task.

## Touch and accessibility

- Limit touch-action changes to the gesture surface.
- Keep controls and instructions visible without hover.
- Provide a keyboard alternative when moving the item is meaningful.
- Report meaningful position changes in a non-disruptive way if users need to know them.
- Do not make an element appear draggable unless it can be moved.
- Keep the drag surface large enough to use and avoid trapping scrolling.

## Common mistakes

- **Expecting pan to move the element:** pan reports a gesture; use `drag` to move the component.
- **Unexpected page scrolling:** configure `touch-action` on the intended gesture surface.
- **Bad constraints:** ensure the constraint ref is attached to the correct container and the child has measurable dimensions.
- **Excessive overshoot:** reduce `dragElastic` or disable it.
- **Surprising release movement:** set `dragMomentum={false}` when inertia is not desired.
- **State updates every frame:** prefer Motion values or end callbacks unless React needs each sample.
- **Pointer-only control:** supply an accessible keyboard alternative for meaningful drag operations.
- **Using element drag for data transfer:** use native drag-and-drop for files and data payloads.

## Recap and practice

Pan observes pointer movement; drag applies movement to the component. Choose an axis, add pixel or ref constraints, then tune elasticity and momentum. Touch behavior and keyboard alternatives are part of the interaction design.

Practice by keeping a draggable tile inside a bounded panel. Compare both-axis movement with horizontal-only movement, turn off momentum, then add a keyboard-operated nudge control that can move the same item.

Continue with [AnimatePresence and exit animations](011-animatepresence-and-exit-animations.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures</a></td><td><a href="../qnas/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints Q&amp;A</a></td><td><a href="../examples/010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints example</a></td><td><a href="011-animatepresence-and-exit-animations.md">011 - AnimatePresence and exit animations</a></td></tr></tbody>
</table>
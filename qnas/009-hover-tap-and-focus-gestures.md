# Hover, tap, and focus gestures Q&A

## 1. What is a Motion gesture animation?

It is a temporary target activated by input, such as hovering, tapping, focusing, panning, or dragging. When the gesture ends, Motion returns to the underlying target.

## 2. What does §whileHover§ do?

It animates to a target while a supported pointer hovers and returns when hover ends. Use it for subtle feedback, not as the only way to reveal essential actions.

## 3. Is hover a touch interaction?

No. Touch devices do not provide a consistent hover state. Motion filters emulated hover to avoid sticky behavior. Provide a visible or tap-accessible alternative.

## 4. What does §whileTap§ mean?

It animates while a primary pointer press is active on the component. If the press ends outside the component, the gesture is cancelled instead of completing as a tap.

## 5. How do §onTapStart§, §onTap§, and §onTapCancel§ differ?

Start runs when the press begins, tap runs after a successful press and release on the same target, and cancel reports an interrupted or unsuccessful press.

## 6. Should a button action use §onTap§ or §onClick§?

Use §onClick§ on a native button for the action. Tap callbacks are for gesture lifecycle behavior. Avoid firing the same action through both handlers.

## 7. Are tap gestures keyboard-accessible?

Motion tap gestures support keyboard activation. Native buttons and links are still preferred because they provide semantics, focus, and standard activation behavior directly.

## 8. What does §whileFocus§ respond to?

It responds to focus-visible interaction. This generally provides a focus animation for keyboard navigation without adding unnecessary styling after pointer focus.

## 9. Can animation replace a visible focus indicator?

No. A focus cue must remain clear when animation is disabled. Do not remove the outline unless a high-contrast replacement is present.

## 10. Can gesture props use variant labels?

Yes. Define named targets in §variants§ and pass a label to §whileHover§, §whileTap§, or §whileFocus§. Ensure each label describes a clear state.

## 11. What if hover and focus happen together?

Check the combined result. The hover target should not hide the focus cue. Keep the effects compatible, for example a small lift plus a high-contrast outline.

## 12. Do gesture props store application state?

No. They describe temporary input states. Use React state for lasting facts such as saved, selected, or disabled.

## 13. Which properties work well for feedback?

Small transforms and opacity are often appropriate because they do not normally change document flow. Avoid large scale or layout-size changes for routine hover feedback.

## 14. How should I validate a gesture control?

Use mouse or pointer, touch, and keyboard. Confirm the action runs once, focus is visible, and the control remains understandable without hover or animation.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger Q&amp;A</a></td><td><a href="../lessons/009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures lesson</a></td><td><a href="../examples/009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures example</a></td><td><a href="010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints Q&amp;A</a></td></tr></tbody>
</table>
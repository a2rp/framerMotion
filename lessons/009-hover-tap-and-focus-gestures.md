# Hover, tap, and focus gestures

Motion gestures animate temporary interaction states. This chapter covers hover, tap, and keyboard-visible focus using `whileHover`, `whileTap`, and `whileFocus`. These props supplement real HTML controls and event handlers; they do not replace semantics or application state.

## Hover feedback

`whileHover` selects an animation target while a supported pointer hovers over an element, then returns to the underlying target when hover ends.

~~~jsx
<motion.button whileHover={{ y: -2, scale: 1.02 }}>
  Save
</motion.button>
~~~

Hover is a pointer affordance, not a dependable touch or keyboard action. Do not hide essential instructions or controls behind hover. Keep the effect small so it does not obscure content or shift nearby layout.

## Tap and press feedback

`whileTap` is active while a primary pointer is pressing the element. A successful tap begins and ends on the same target. Motion also provides `onTapStart`, `onTap`, and `onTapCancel` for code that needs the gesture lifecycle.

Attach feedback to semantic controls. A native button has the expected role and activation behavior:

~~~jsx
<motion.button type="button" whileTap={{ scale: 0.97 }} onClick={save}>
  Save changes
</motion.button>
~~~

Use `onClick` for the application action on a real button. Tap callbacks are useful for gesture-specific events, such as logging a cancelled press or starting temporary feedback. Do not call the same action from both `onClick` and `onTap` or the user may trigger it twice.

Motion tap gestures support keyboard activation. Prefer native buttons and links anyway; they provide familiar semantics and browser behavior without recreating controls on generic elements.

## Focus feedback

`whileFocus` responds to focus-visible interaction. It can animate an outline or other focus cue when a user navigates by keyboard.

~~~jsx
<motion.a
  href="/account"
  whileFocus={{ outlineColor: "#0b63ce", outlineWidth: 3, outlineOffset: 3 }}
>
  Account
</motion.a>
~~~

A visible focus indicator is required even when animation is disabled. Do not remove the outline without replacing it with a high-contrast cue. Color alone is not enough; use outline width, shape, or offset so the focused control is easy to identify.

## Combine gesture states

A motion component can define several gesture targets. Each prop can use an object or a label from `variants`.

~~~jsx
const states = {
  rest: { scale: 1 },
  hover: { scale: 1.03 },
  press: { scale: 0.97 },
  focus: { y: -1 },
};

<motion.button
  variants={states}
  initial="rest"
  whileHover="hover"
  whileTap="press"
  whileFocus="focus"
>
  Continue
</motion.button>
~~~

Gesture states are temporary. When the gesture ends, Motion returns to the persistent target. If a user action changes a lasting fact, such as a saved or selected state, update React state and expose it with appropriate semantics.

## Practical choices

| Input | Motion prop | Design use |
| --- | --- | --- |
| Pointer hover | `whileHover` | Subtle pointer feedback |
| Press and release | `whileTap` | Press response |
| Keyboard-visible focus | `whileFocus` | Focus outline or cue |
| Application action | Native `onClick` | Perform the action once |

Keep effects brief and modest. Transforms and opacity usually avoid changing document flow. Large size changes, animated layout dimensions, or hover-only menus can create movement and access problems.

## Common mistakes

- Using a generic `div` as an action instead of a native button or link.
- Making essential content visible only while hovering.
- Removing the browser outline without an equally clear focus indicator.
- Running one action from both click and tap handlers.
- Treating a temporary gesture as persistent application state.
- Forgetting to test touch, pointer, and keyboard input.
- Using dramatic scale changes that cover neighboring controls.
- Assuming focus feedback is available only through animation.

## Recap and practice

`whileHover`, `whileTap`, and `whileFocus` express temporary gesture targets. Keep semantic controls, preserve visible focus, and store lasting application facts in React state.

Practice by creating a button with all three props. Activate it with pointer, touch, Enter, and Space. Check that the action fires once, focus remains obvious, and the control stays understandable when motion is disabled.

Continue with [pan, drag, and constraints](010-pan-drag-and-constraints.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger</a></td><td><a href="../qnas/009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures Q&amp;A</a></td><td><a href="../examples/009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures example</a></td><td><a href="010-pan-drag-and-constraints.md">010 - Pan, drag, and constraints</a></td></tr></tbody>
</table>
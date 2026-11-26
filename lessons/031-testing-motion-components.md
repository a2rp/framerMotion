# Testing Motion components

Animation tests should protect behavior that matters to a person using the interface. They should verify that controls work, content appears at the right time, keyboard interaction remains available, and reduced-motion preferences are respected. They should not depend on an exact intermediate transform value at a particular millisecond.

## Learning outcomes

By the end of this lesson, you should be able to:

- Choose component, integration, browser, and visual tests for the behavior being checked.
- Assert accessible states and user-visible outcomes instead of Motion's internal animation state.
- Keep tests deterministic without hiding application bugs.
- Recognize what a DOM test environment cannot measure.
- Test reduced motion, presence transitions, gestures, and scrolling at the right level.

## Choose the right test boundary

A component test can verify the DOM and interaction contract. A browser test can verify actual layout, focus movement, pointer behavior, scrolling, and rendered animation. A screenshot comparison can detect visual drift, but it does not replace interaction assertions.

| Test level | Good target | Weak target |
| --- | --- | --- |
| Pure unit test | Variant selection or a helper that maps state to a target | Reimplementing Motion's easing calculations |
| Component test | Button state, accessible name, expanded state, status text | An exact transform sampled halfway through an animation |
| Browser integration test | Focus, scroll-triggered behavior, exit timing, layout measurement | A tiny pure function tested through a full browser |
| Visual comparison | Unexpected layout or appearance changes in a stable viewport | Proving keyboard access or announcements |

Choose the smallest level that can observe the behavior. Use a real browser when a feature depends on geometry, browser rendering, pointer capture, intersection observation, or focus across an exit transition.

## Start with a behavior contract

Write down what a user can observe before writing assertions. For a save control, that contract might be:

1. It is a button with a clear accessible name.
2. Its pressed state communicates whether the item is saved.
3. Activating it changes that state.
4. A status region announces the result.
5. Decorative motion does not add a duplicate accessible name.
6. The action works with keyboard activation as well as a pointer.

These assertions remain useful if the animation changes from scale to opacity or its duration changes.

## Assert state, not a frame

A browser animation passes through many intermediate values. Those values depend on time, scheduling, device speed, and the rendering environment. Tests that inspect one interpolated transform can be flaky while the interaction is correct.

Prefer assertions such as:

- The control starts with `aria-pressed="false"`.
- Activating it changes the pressed state to `true`.
- The status message contains the expected result.
- A disclosure's content becomes available after activation.
- Closing a panel returns focus to the control that opened it.

Use a browser-level assertion for a property that is itself part of the product behavior, such as an element being visible after scrolling. Avoid inspecting Motion's private values or relying on a computed transform matrix produced by a DOM emulator.

## Make tests deterministic

A deterministic test sets the inputs that matter and avoids waiting on an arbitrary duration.

- Use explicit component state and stable accessible names.
- Keep tests independent of real network requests, image decoding, and unrelated timers.
- Use a test-only Motion policy when the test only needs final semantic state.
- Use real browser time when transition timing is part of the requirement.
- Use fake timers for application timers you own, not as a blanket setting for every animation test.
- If a test must advance animation time, explain why and use the test runner's supported clock APIs.

A test wrapper can set a reduced-motion policy:

~~~jsx
import { MotionConfig } from "motion/react";

export function MotionTestProvider({ children }) {
  return (
    <MotionConfig reducedMotion="always">
      {children}
    </MotionConfig>
  );
}
~~~

This policy disables Motion's transform and layout animations. It does not mean every animated value or every browser effect has been disabled. Keep assertions focused on the DOM contract, and test visible timing in a browser when timing matters.

## Test reduced-motion behavior deliberately

Reduced motion is a user preference, not a test failure. Test the product under normal and reduced-motion settings when the interface contains meaningful movement.

A component test can verify that the same action and content remain available. A browser test can enable `prefers-reduced-motion: reduce`, open the interface, and verify that large travel, scaling, parallax, or autoplay is removed or replaced with a restrained alternative.

If the application branches on `useReducedMotion`, give the preference a controlled value in the test environment. Do not assume that a DOM emulator has a useful `matchMedia` implementation by default. Restore any global mock after each test so preference state cannot leak into another test.

Motion's global `MotionConfig reducedMotion="user"` setting is useful as an application default. A component can use `useReducedMotion` for a tailored alternative, such as removing position travel while preserving a short opacity change. Test the intended policy rather than asserting that every animation must disappear.

## Presence and exit behavior

An exiting element remains mounted while its exit animation runs. A component test that expects immediate removal can be incorrect if it renders the real exit lifecycle.

For a simple semantic test, verify that the close action changes the controlling state and that focus remains sensible. For an integration test whose requirement is that content is gone after its exit, wait for the exit through a supported browser or animation-aware setup. Avoid arbitrary sleeps such as `await new Promise(resolve => setTimeout(resolve, 500))`. They slow the suite and become stale when a transition changes.

When content contains focusable elements, test what happens if it closes while focus is inside. Move focus to a meaningful surviving control or otherwise preserve a logical focus position.

## Gestures, layout, and scroll

DOM emulators do not reproduce all browser geometry. Prefer browser tests for:

- Drag constraints and pointer capture.
- Hover and touch behavior that depends on real pointer input.
- Layout animation and measured element size.
- Scroll progress and viewport intersection.
- Sticky and fixed-position elements.
- Focus order during an exit transition.
- Visual clipping, stacking contexts, and responsive breakpoints.

A component test is still useful for checking that a drag handle has a name, keyboard alternative, or accessible value. It cannot prove that the physical interaction works in a real viewport.

## Keep mocks narrow

Mock APIs that are missing from the environment, such as `matchMedia`, only when the component needs them. Mock a preference response or a real external dependency, not the component behavior under test. Prefer a real Motion component for semantic tests; this catches integration mistakes in imports and props.

If you mock Motion itself, the test no longer verifies that the application uses Motion correctly. Reserve that approach for a narrow test where the animation library is irrelevant, and keep a separate integration or browser check.

## Use stable queries

Use role and accessible-name queries first. A selector tied to a generated class or nested DOM structure can keep passing after the user-facing behavior breaks.

~~~js
screen.getByRole("button", { name: "Save" })
screen.getByRole("status")
~~~

Use a test id only when the element has no meaningful user-facing query. If labels repeat, scope the query to a named region.

## Suggested workflow

1. Describe the observable behavior and user action.
2. Choose the smallest test level that can prove it.
3. Render real component code with only necessary providers.
4. Query by role, label, or visible text.
5. Drive interaction with a user-oriented event helper.
6. Assert the resulting semantic state and content.
7. Add a browser test for geometry, timing, scrolling, or visual output the DOM environment cannot represent.
8. Repeat with reduced motion when movement is part of the feature.
9. Keep assertions focused on user-facing requirements and remove incidental implementation details.

## Common testing mistakes

- Requiring a specific transform string when the user only needs the state to change.
- Using a fixed timeout instead of waiting for a condition.
- Treating a DOM emulator as proof of layout or paint performance.
- Mocking all of Motion, then claiming the real integration was tested.
- Wrapping every test in fake timers when the application does not own the animation clock.
- Testing only the visual effect while ignoring names, pressed state, or keyboard use.
- Assuming reduced motion means the control should stop working.
- Comparing screenshots with unstable fonts, data, viewport, or capture time.

## Practice

Write a component test for a toggle that animates a decorative icon. Assert its accessible name, pressed state, and status message before and after pointer and keyboard activation. Do not assert an intermediate scale value.

Then add a browser check for the feature's real geometry or exit lifecycle. Run it with normal motion and reduced motion enabled. Record which requirements belong in each test layer and why.

Continue with [Capstone: accessible animated interface](032-capstone-accessible-animated-interface.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="030-performance-and-debugging.md">030 - Performance and debugging</a></td><td><a href="../qnas/031-testing-motion-components.md">031 - Testing Motion components Q&amp;A</a></td><td><a href="../examples/031-testing-motion-components.md">031 - Testing Motion components example</a></td><td><a href="032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface</a></td></tr></tbody>
</table>
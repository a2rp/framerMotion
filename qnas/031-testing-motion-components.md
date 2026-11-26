# Testing Motion components: Q&A

## 1. What should an animation test prove?

It should prove a user-facing contract: the control can be found and operated, the state changes correctly, content is announced or revealed, and focus remains usable. Assert timing only when timing itself is a product requirement.

## 2. Should I test exact transform values?

Usually not. A computed transform can be sampled at different points depending on scheduling and the environment. Prefer checking state, content, accessibility attributes, and behavior. Use a browser-level visual assertion only when rendered movement is itself important.

## 3. What is a good component test for a Motion component?

Render the real component, query controls by role and accessible name, perform an interaction, and assert the resulting state. A test can use a Motion provider with a deterministic policy if semantic behavior does not require animated timing.

## 4. Does a passing DOM-emulator test prove that the animation looks right?

No. A DOM emulator checks markup and many interactions, but it does not reproduce full layout, paint, viewport geometry, pointer capture, or the real animation pipeline. Use a browser for those requirements.

## 5. How do I choose between a component test and a browser test?

Use a component test for accessible names, state, callbacks, and content. Use a browser test for geometry, scrolling, drag behavior, focus across exit, responsive layout, and timing that depends on real rendering. Use both when a feature has both kinds of requirements.

## 6. Should I assert an animation's intermediate frame?

Only if that exact frame is part of the product requirement. Otherwise it couples the test to duration, easing, and scheduling details that can change without breaking the experience.

## 7. How can tests avoid waiting for an animation duration?

Assert the semantic result after the user action. If the test needs an exiting element to be gone, wait for that condition with the test framework or run the lifecycle in a browser. Avoid fixed sleeps.

## 8. Is `waitFor` always needed after a click?

No. Await the user interaction helper, then assert the expected result. Use asynchronous queries or `waitFor` when the result genuinely appears asynchronously. Do not wrap every synchronous assertion in a retry loop.

## 9. What does `MotionConfig reducedMotion="always"` do in a test?

It sets a reduced-motion policy for descendant Motion components and disables transform and layout animations. It can stabilize tests that care about semantic outcomes. It does not disable every possible animation or replace browser testing.

## 10. Does reduced motion mean every animation must disappear?

No. A product may replace large travel or parallax with a restrained opacity change, or remove only specific movement. Test the policy chosen by the interface, and ensure the same control and content remain available.

## 11. How can I test code that calls `useReducedMotion`?

Control the media preference in the test environment, for example by providing a predictable `matchMedia` implementation that reports `prefers-reduced-motion: reduce` as matching. Restore that mock after each test. Also test the integrated preference in a browser.

## 12. Why does `matchMedia` sometimes fail in a DOM test?

Some test DOM environments do not implement it. If the component or Motion hook needs it, add a narrow setup mock with the fields and event methods the code uses. Avoid a broad mock that changes unrelated tests.

## 13. Should I use fake timers for Motion animations?

Not by default. Fake timers can interact with animation frames and make a test harder to understand. Use them for timers owned by application code. Use a real browser when actual animation timing is part of the requirement.

## 14. How should I test an exit animation?

First test the close action and focus behavior. If the requirement includes staying mounted during exit and then leaving the DOM, test that lifecycle with an animation-aware integration setup or a real browser. Do not assume removal is immediate.

## 15. How do I test keyboard use?

Use an interaction helper to tab to the control and activate it with Enter or Space. Assert the same accessible state and content that a pointer test asserts. For a disclosure, also test Escape behavior if the interface supports it.

## 16. Which queries should I prefer?

Prefer role, accessible name, label, and visible text. For example, query a save action as a button named "Save" and a live region by role "status". Use a test id only when an element has no meaningful user-facing query.

## 17. Should snapshots be the main test for an animated component?

No. A snapshot records markup at one point, but it does not prove interaction, timing, keyboard access, or appearance across a browser. Use focused behavioral assertions. Keep a small snapshot only when rendered structure itself is a stable contract.

## 18. Should I mock Motion to make tests faster?

Usually not for component integration tests. Mocking the library can hide incorrect imports, props, and interactions. If a test is about unrelated logic, a narrow mock may be reasonable, but retain a real Motion integration or browser test.

## 19. How should I test drag gestures?

A component test can check accessible names, labels, and state updates. Test actual pointer movement, constraints, capture, and release in a browser with real pointer input. A synthetic click does not prove drag behavior.

## 20. How should I test scroll-triggered animation?

Test semantic content and configuration at component level. Use a real browser for intersection, scroll position, viewport visibility, and reduced-motion behavior. A DOM emulator does not provide reliable viewport geometry.

## 21. Can a visual regression test replace accessibility tests?

No. A screenshot cannot establish that a button has a useful name, focus is visible, or state is announced. Keep accessibility and interaction assertions alongside visual checks.

## 22. Why is a test flaky only on CI?

Look for dependence on duration, frame timing, font or image loading, viewport size, animation capture time, leaked media mocks, or parallel tests sharing global state. Make inputs explicit and wait for the actual condition rather than a guessed delay.

## 23. What should I do when a selector breaks after a harmless refactor?

Replace selectors tied to generated classes or nested markup with queries based on role, accessible name, or text. If no user-facing query exists, add a stable test id and explain why.

## 24. How should I verify a performance claim about a component?

A component test cannot prove frame performance. Use the browser Performance panel and, when React rendering is suspected, a React profile. Compare the same interaction before and after under the same conditions.

## 25. What belongs on a testing checklist?

- Can a user find the control by its role and name?
- Does pointer and keyboard activation produce the same state?
- Is the resulting state communicated accessibly?
- Is focus still logical after opening and closing?
- Does reduced motion preserve the action and useful content?
- Are geometry, scrolling, dragging, and visual timing checked in a browser?
- Are mocks restored and waits condition-based?

## 26. What should I practice?

Test a toggle that animates a decorative icon. Verify the accessible name, pressed state, and status message. Then add a browser test for a visual behavior a DOM emulator cannot measure, and repeat with reduced motion enabled.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="030-performance-and-debugging.md">030 - Performance and debugging Q&amp;A</a></td><td><a href="../lessons/031-testing-motion-components.md">031 - Testing Motion components lesson</a></td><td><a href="../examples/031-testing-motion-components.md">031 - Testing Motion components example</a></td><td><a href="../qnas/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface Q&amp;A</a></td></tr></tbody>
</table>
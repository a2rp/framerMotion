# Imperative sequences with useAnimate Q&A

## 1. What does useAnimate return?

It returns a scope ref and a scoped `animate` function. Attach the ref to an HTML/SVG element or a Motion component.

## 2. When should I use useAnimate?

Use it when a sequence, imperative event handler, or playback control is central. For ordinary state-to-style changes, declarative Motion props are usually simpler.

## 3. What does scoped selector mean?

A selector passed to the scoped `animate` function resolves only within the scope element. It does not select matching elements elsewhere on the page.

## 4. Can animate target an element directly?

Yes. Pass the DOM element, such as `scope.current`, or pass a selector for matching descendants in the scope.

## 5. How do I define a timeline sequence?

Pass an array of segments. Each segment contains a target, keyframes, and optional transition settings such as duration or `at`.

## 6. Do sequence segments run in parallel?

By default, segments run sequentially. Use `at` to overlap them or start at a specific time.

## 7. What does at: "<" mean?

It starts the next segment at the same time as the previous segment. A string like `"<+0.1"` starts shortly after the previous segment begins.

## 8. What does at: "+0.2" mean?

It starts relative to the end of the previous segment, with a 0.2 second offset.

## 9. What are timeline labels useful for?

A string label marks a time in the sequence. Later segments can refer to the label, making a longer timeline easier to read and adjust.

## 10. Can I await an animation?

Yes. The returned animation controls can be awaited so code can continue after that animation completes. Use this for truly dependent steps; one timeline is often clearer for coordinated timing.

## 11. How do I pause and resume?

Store the returned controls in a ref. Call `pause()` to pause and `play()` to resume.

## 12. Can a sequence be reversed?

Yes. The playback controls provide `reverse()`. Add a real button if the user needs to control direction.

## 13. How do I stop a running sequence?

Call `stop()` on its controls. Decide whether a new play action should stop and replace the previous timeline, ignore the click, or queue new work.

## 14. Does useAnimate clean up on unmount?

Yes. Animations started with the hook's scoped animate function are cleaned up when the component unmounts. Explicitly stop controls earlier when application behavior requires cancellation before unmount.

## 15. Should I start an animation in render?

No. Start it from an event handler or an effect. Starting imperatively during render can repeat and conflicts with React rendering.

## 16. When should an effect start an animation?

When the animation should respond to a state or lifecycle change. Declare dependencies correctly so the effect does not restart on every render.

## 17. Why does my selector find nothing?

Check that the scope ref is attached to a mounted element and the selector matches descendants inside it. The selector cannot target a sibling outside the scope.

## 18. Why does a second click create strange motion?

A previous timeline may still be active. Keep its controls and explicitly stop or replace it when the interaction should restart.

## 19. Should I store animation progress in React state?

Usually not. Keep high-frequency progress in Motion. React state can represent discrete labels such as paused, playing, or completed.

## 20. When are variants better than an imperative sequence?

Use variants for reusable visual states and declarative orchestration. Use `useAnimate` when code needs a controlled timeline, DOM targets, or playback methods.

## 21. Is useAnimate the right way to animate exit?

Usually use `AnimatePresence` with an `exit` prop. For a custom exit lifecycle, Motion also provides presence APIs, but a normal exit should stay declarative.

## 22. How should reduced motion affect a timeline?

Remove or reduce large travel and make the timeline immediate if appropriate. Preserve the same final state and keep controls usable.

## 23. Can an animation move keyboard focus?

It should not move focus as a side effect. Keep focus on the user's activated control unless a clear interaction requirement explicitly moves it.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="018-in-view-animations-and-useinview.md">018 - In-view animations and useInView Q&amp;A</a></td><td><a href="../lessons/019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate lesson</a></td><td><a href="../examples/019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate example</a></td><td><a href="020-svg-and-path-animations.md">020 - SVG and path animations Q&amp;A</a></td></tr></tbody>
</table>

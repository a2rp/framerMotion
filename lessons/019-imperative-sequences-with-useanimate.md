# Imperative sequences with useAnimate

Most interface animations are easiest to describe declaratively with props such as `animate`, `whileHover`, and `exit`. Use an imperative sequence when an interaction needs an ordered timeline, when a button must control playback, or when several descendants need coordinated timing.

In React, `useAnimate` provides a scoped animation function and automatically cleans up animations when its component unmounts.

## The scope and animate function

~~~jsx
import { useAnimate } from "motion/react";

function Panel() {
  const [scope, animate] = useAnimate();

  return <section ref={scope}>...</section>;
}
~~~

The returned `scope` ref belongs on an HTML/SVG element or a Motion component. The `animate` function can target that element directly or use a selector. Selectors are scoped to the element holding `scope`, so `"h2"` finds matching descendants of this component rather than headings elsewhere on the page.

~~~jsx
animate(scope.current, { opacity: 1 }, { duration: 0.3 });
animate("li", { opacity: 1 }, { duration: 0.3 });
~~~

This local scope helps keep a component's animation from reaching unrelated page elements. Keep selectors specific enough to communicate which child is meant to animate.

## Build a sequence

Pass an array of animation segments to `animate`. Each segment contains a target, keyframes, and optional transition settings:

~~~jsx
const controls = animate([
  ["[data-part='title']", { opacity: [0, 1], y: [16, 0] }],
  ["[data-part='copy']", { opacity: [0, 1], y: [12, 0] }],
]);
~~~

By default, segments run one after another. Each one can define duration, easing, delay, and an `at` position. Use `at` to overlap or position segments:

- A number starts a segment at a timeline time in seconds.
- A string such as `"+0.2"` starts relative to the end of the previous segment.
- `"<"` starts at the same time as the previous segment.
- A string such as `"<+0.1"` starts shortly after the previous segment begins.
- A label in the sequence gives a readable named time that later segments can reference.

For a simple stagger, `stagger` can be used in a segment's delay. Use the smallest timing vocabulary that makes the sequence understandable. Explicit overlaps are useful for an intro where a heading and supporting copy should animate together; long chains of tiny offsets are difficult to maintain.

## Await one animation, then continue

The animation returned by `animate` can be awaited. This is useful when the next step depends on the prior animation completing:

~~~jsx
async function reveal() {
  await animate("[data-part='title']", { opacity: 1, y: 0 });
  await animate("[data-part='action']", { opacity: 1, y: 0 });
}
~~~

A timeline sequence is usually clearer when the steps are part of one coordinated intro. Separate awaited calls are useful for truly dependent steps or when code between steps must run. Avoid using timer callbacks to approximate animation completion; use animation controls and completion callbacks instead.

## Control playback

Each `animate` call returns playback controls. Store them in a ref when event handlers need to access the active sequence:

~~~jsx
const controlsRef = useRef(null);

function start() {
  controlsRef.current = animate(sequence);
}

function pause() {
  controlsRef.current?.pause();
}

function resume() {
  controlsRef.current?.play();
}

function reverse() {
  controlsRef.current?.reverse();
}
~~~

Playback controls also support operations such as seeking or changing speed. Use them when the interface needs that control. For a basic interaction, play, pause/resume, and stop may be enough. Keep React state for UI labels such as “Playing” or “Paused”; do not mirror every animation frame into React state.

A repeated click can start another sequence while the previous one is active. Decide whether that should restart, ignore the click, or queue another action. For a restart button, stop the previous controls before creating a new timeline.

## Event handlers and effects

Call `animate` from a user event when a button explicitly starts the sequence. Use an effect when animation should start in response to a state or lifecycle change. Effects should declare their dependencies and avoid accidentally starting a new animation on every render.

`useAnimate` automatically cleans up animations started through its scoped function when the component unmounts. Explicitly stop or replace controls when the user starts a different sequence or when application logic needs to cancel playback before unmount.

Do not animate before the scope exists. Calling an animation in a click handler after render naturally targets a mounted scope. If an effect uses the ref, check that the referenced element is available.

## Choose declarative or imperative motion

| Need | Good starting point |
| --- | --- |
| Animate to a value when React state changes | `animate` prop |
| Respond to hover, tap, focus, or drag | Gesture props |
| Animate an element when it enters view | `whileInView` or `useInView` |
| Coordinate multiple descendants in a controlled timeline | `useAnimate` |
| Let the user pause, resume, or reverse a sequence | `useAnimate` controls |
| Animate an element leaving the React tree | `AnimatePresence` and `exit` |

Imperative code is not automatically more powerful for every interaction. Prefer declarative props when state already describes the desired visual state. Reach for a sequence when timing and playback control are central to the interaction.

## Accessibility and reliable behavior

An animation should not be required to operate a control or understand content. Keep labels and information present in the document. Do not make focus depend on an animation's completion.

Offer real buttons for playback controls, use clear names, and preserve focus. Avoid controls that disappear while the animation is running. Respect reduced-motion preferences: remove large travel or make the timeline immediate while retaining the same final visual state.

If a sequence runs automatically, do not create long or looping motion that users cannot stop when it distracts from content. Use status text for meaningful discrete state changes, not for every timeline update.

## Troubleshooting

1. **Nothing animates:** confirm the scope ref is on a mounted element and the selector matches one or more descendants.
2. **The wrong element changes:** use a scoped, specific selector and verify the scope boundary.
3. **The sequence starts too late:** check whether segments are sequential by default or use an unintended `at` offset.
4. **Segments all start together:** inspect `at: "<"` and overlapping positions.
5. **Repeated click causes competing motion:** stop or replace the current controls before starting again.
6. **Animation restarts every render:** move it to a user event or a correctly-declared effect.
7. **Animations survive a state change unexpectedly:** stop the active controls when that interaction requires cancellation.
8. **Motion feels uncomfortable:** reduce distance, duration, and repetition, and support reduced motion.

## Recap and practice

`useAnimate` combines a component-local scope with an imperative animation function. Use it for sequences or playback controls that are awkward to express declaratively. Keep selectors within the scope, schedule only the segments that need overlap, and choose cancellation behavior for repeated interactions.

Practice with an intro sequence that reveals a heading and button. Add pause and resume controls, then test what a second play click should do. Finally, make the motion immediate when reduced motion is preferred.

Continue with [SVG and path animations](020-svg-and-path-animations.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="018-in-view-animations-and-useinview.md">018 - In-view animations and useInView</a></td><td><a href="../qnas/019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate Q&amp;A</a></td><td><a href="../examples/019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate example</a></td><td><a href="020-svg-and-path-animations.md">020 - SVG and path animations</a></td></tr></tbody>
</table>

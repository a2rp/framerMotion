# Imperative sequences with useAnimate example

This example uses a scoped sequence to introduce a small welcome card. Buttons let the user play, pause/resume, reverse, or stop the sequence. When reduced motion is preferred, the same final states appear immediately with no travel.

## Setup

Install Motion with `npm install motion`. Save the component as `src/WelcomeSequence.jsx` and render `<WelcomeSequence />`.

## `src/WelcomeSequence.jsx`

~~~jsx
import { useRef, useState } from "react";
import { useAnimate, useReducedMotion } from "motion/react";

export function WelcomeSequence() {
  const [scope, animate] = useAnimate();
  const controlsRef = useRef(null);
  const shouldReduceMotion = useReducedMotion();
  const [paused, setPaused] = useState(false);
  const [status, setStatus] = useState("Ready");

  function playSequence() {
    // A repeated Play action deliberately restarts the current timeline.
    controlsRef.current?.stop();

    const duration = shouldReduceMotion ? 0 : 0.32;
    const offset = shouldReduceMotion ? 0 : "<+0.08";
    const travel = shouldReduceMotion ? 0 : 16;

    const sequence = [
      [
        "[data-part='eyebrow']",
        { opacity: [0, 1], y: [travel, 0] },
        { duration },
      ],
      [
        "[data-part='title']",
        { opacity: [0, 1], y: [travel, 0] },
        { at: offset, duration },
      ],
      [
        "[data-part='copy']",
        { opacity: [0, 1], y: [travel, 0] },
        { at: offset, duration },
      ],
      [
        "[data-part='actions']",
        { opacity: [0, 1], y: [travel, 0] },
        { at: offset, duration },
      ],
    ];

    setPaused(false);
    setStatus("Playing");
    controlsRef.current = animate(sequence, {
      onComplete: () => {
        setPaused(false);
        setStatus("Sequence complete");
      },
    });
  }

  function togglePause() {
    const controls = controlsRef.current;
    if (!controls) return;

    if (paused) {
      controls.play();
      setStatus("Playing");
    } else {
      controls.pause();
      setStatus("Paused");
    }
    setPaused((value) => !value);
  }

  function reverseSequence() {
    if (!controlsRef.current) return;
    controlsRef.current.reverse();
    setPaused(false);
    setStatus("Reversing");
  }

  function stopSequence() {
    if (!controlsRef.current) return;
    controlsRef.current.stop();
    setPaused(false);
    setStatus("Stopped");
  }

  return (
    <main
      style={{
        maxWidth: 680,
        margin: "64px auto",
        padding: 24,
        color: "#1e293b",
        fontFamily: "system-ui, sans-serif",
        lineHeight: 1.6,
      }}
    >
      <section
        ref={scope}
        style={{
          padding: 32,
          border: "1px solid #cbd5e1",
          borderRadius: 20,
          background: "#f8fafc",
          boxShadow: "0 18px 50px rgb(15 23 42 / 10%)",
        }}
      >
        <p data-part="eyebrow" style={{ color: "#6750a4", fontWeight: 700 }}>
          A small timeline
        </p>
        <h1 data-part="title">Welcome to the sequence</h1>
        <p data-part="copy">
          The heading, explanation, and actions are animated in order with a
          small overlap. The selectors are scoped to this card.
        </p>
        <div
          data-part="actions"
          style={{ display: "flex", flexWrap: "wrap", gap: 10 }}
        >
          <button type="button" onClick={playSequence}>Play or restart</button>
          <button
            type="button"
            onClick={togglePause}
            aria-pressed={paused}
            disabled={!controlsRef.current}
          >
            {paused ? "Resume" : "Pause"}
          </button>
          <button
            type="button"
            onClick={reverseSequence}
            disabled={!controlsRef.current}
          >
            Reverse
          </button>
          <button
            type="button"
            onClick={stopSequence}
            disabled={!controlsRef.current}
          >
            Stop
          </button>
        </div>
        <p role="status" aria-live="polite">{status}</p>
      </section>

      <p style={{ marginTop: 24 }}>
        The controls keep their place in the document. Animation does not move
        keyboard focus or change the meaning of the instructions.
      </p>
    </main>
  );
}
~~~

## How it works

`useAnimate` returns `scope` and a scoped `animate` function. The ref is attached to the card. Each `data-part` selector resolves inside that card, which prevents the sequence from selecting matching content elsewhere.

Each sequence segment animates opacity and y. By default, segments run one after another; the `at` value overlaps later segments shortly after the previous segment starts. The returned controls are stored in a ref so event handlers can pause, resume, reverse, or stop the current timeline.

The Play button stops a prior timeline before restarting. React state updates only the paused label and status text, not the animation frame by frame. The hook also cleans up its animations when the component unmounts.

## Expected behavior

- Play reveals the eyebrow, heading, copy, and controls in a short overlapping sequence.
- Pause holds the current playback position; Resume continues it.
- Reverse plays the timeline in the opposite direction.
- Stop halts the active timeline.
- Play again deliberately stops and replaces an active timeline.
- Reduced motion removes travel and makes the sequence immediate.

## Adapt the example

- Change `at` positions to make segments sequential or more overlapped.
- Add a named label to the sequence and schedule another segment at that label.
- Animate a child directly by passing a DOM element rather than a selector.
- Add an explicit completed state if the interface needs to react after the timeline ends.
- Compare the same interaction with declarative `animate` props and keep the simpler approach when possible.

## Common mistakes

- **Scope is on the wrong element:** attach the ref to a DOM element containing all intended targets.
- **Selector reaches outside scope:** use the hook's scoped function, not a global document query.
- **Sequence timing is confusing:** remember segments run sequentially unless `at` changes their start.
- **Repeated play competes with the old sequence:** stop or replace the current controls.
- **Animation runs during render:** start it only from an event or effect.
- **State mirrors every frame:** use Motion controls for playback and React state for discrete labels.
- **Controls vanish during playback:** keep pause/stop actions available if the user needs them.
- **Reduced motion is ignored:** make movement small or immediate for that preference.

## Practice

1. Remove `at` from the later segments and compare sequential timing.
2. Change `"<+0.08"` to `"<"` to start segments together.
3. Make the Play button ignore clicks while running instead of restarting.
4. Add a named label and start a final segment at that label.
5. Enable reduced motion and verify the same content and controls remain available.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="018-in-view-animations-and-useinview.md">018 - In-view animations and useInView example</a></td><td><a href="../lessons/019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate lesson</a></td><td><a href="../qnas/019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate Q&amp;A</a></td><td><a href="020-svg-and-path-animations.md">020 - SVG and path animations example</a></td></tr></tbody>
</table>

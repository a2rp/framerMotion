# Performance and debugging example

This example compares two ways to move a visual indicator: a deliberately React-state-driven animation and a Motion Value animation. The state-driven version is a diagnostic comparison, not a production recommendation. Record each interaction separately and use a React profile to inspect component renders.

## What this example demonstrates

- Motion Values can update a visual style without React rendering for each frame.
- Updating React state on every animation frame can cause repeated component renders.
- A before-and-after profile is more useful than guessing from the source code.
- Reduced-motion preference can make both demonstrations finish immediately.

## Setup

Use a React application with Motion installed:

~~~bash
npm install motion
~~~

Save the files below under src and render PerformanceLab from the application entry point.

## src/PerformanceLab.jsx

~~~jsx
import { useEffect, useState } from "react";
import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
} from "motion/react";
import "./performance-lab.css";

function StateDrivenTrack() {
  const [running, setRunning] = useState(false);
  const [progress, setProgress] = useState(0);
  const shouldReduceMotion = useReducedMotion();

  useEffect(() => {
    if (!running) return;

    const startTime = performance.now();
    let frameId = 0;

    function update(now) {
      const nextProgress = Math.min((now - startTime) / 900, 1);
      setProgress(nextProgress);

      if (nextProgress < 1) {
        frameId = requestAnimationFrame(update);
      } else {
        setRunning(false);
      }
    }

    frameId = requestAnimationFrame(update);
    return () => cancelAnimationFrame(frameId);
  }, [running]);

  function start() {
    setProgress(0);

    if (shouldReduceMotion) {
      setProgress(1);
      setRunning(false);
      return;
    }

    setRunning(true);
  }

  return (
    <section className="demo-card">
      <h2>React state update</h2>
      <p>
        This diagnostic version writes progress to React state on each
        animation frame.
      </p>
      <button type="button" onClick={start} disabled={running}>
        {running ? "Animating" : "Start state version"}
      </button>
      <div className="track" role="img" aria-label="React state animated indicator">
        <div
          className="dot state-dot"
          aria-hidden="true"
          style={{ transform: "translateX(" + progress * 220 + "px)" }}
        />
      </div>
      <p className="track-note">
        React state progress: {Math.round(progress * 100)}%
      </p>
    </section>
  );
}

function MotionValueTrack() {
  const x = useMotionValue(0);
  const shouldReduceMotion = useReducedMotion();

  function start() {
    x.set(0);
    animate(x, 220, {
      duration: shouldReduceMotion ? 0 : 0.9,
      ease: "linear",
    });
  }

  return (
    <section className="demo-card">
      <h2>Motion Value</h2>
      <p>
        This version animates the visual x value without setting React state
        on each frame.
      </p>
      <button type="button" onClick={start}>
        Start Motion Value version
      </button>
      <div className="track" role="img" aria-label="Motion Value animated indicator">
        <motion.div className="dot motion-dot" aria-hidden="true" style={{ x }} />
      </div>
      <p className="track-note">The visual value is managed by Motion.</p>
    </section>
  );
}

export default function PerformanceLab() {
  return (
    <main className="lab">
      <p className="lab-label">Motion performance lab</p>
      <h1>Measure the update path</h1>
      <p className="lab-intro">
        Record one version at a time in the browser Performance panel. Use a
        React profile to compare component renders.
      </p>
      <div className="demo-grid">
        <StateDrivenTrack />
        <MotionValueTrack />
      </div>
    </main>
  );
}
~~~

## src/performance-lab.css

~~~css
:root {
  color-scheme: light;
  font-family: system-ui, sans-serif;
  color: #172033;
  background: #f3f6fa;
}

* {
  box-sizing: border-box;
}

body {
  min-width: 20rem;
  min-height: 100vh;
  margin: 0;
}

.lab {
  width: min(100% - 2rem, 60rem);
  margin: 0 auto;
  padding: 3rem 0;
}

.lab-label {
  color: #475569;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.lab-intro,
.demo-card p {
  line-height: 1.6;
}

.demo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
  gap: 1rem;
  margin-top: 1.5rem;
}

.demo-card {
  padding: 1.25rem;
  border: 1px solid #b8c5d6;
  border-radius: 0.75rem;
  background: #fff;
}

.demo-card button {
  min-height: 2.75rem;
  padding: 0.65rem 0.9rem;
  border: 2px solid #174ea6;
  border-radius: 0.5rem;
  color: #fff;
  background: #174ea6;
  font: inherit;
  font-weight: 700;
  cursor: pointer;
}

.demo-card button:disabled {
  cursor: wait;
  opacity: 0.7;
}

.demo-card button:focus-visible {
  outline: 3px solid #f2a900;
  outline-offset: 3px;
}

.track {
  position: relative;
  height: 3.5rem;
  margin-top: 1.5rem;
  border: 1px solid #b8c5d6;
  border-radius: 0.5rem;
  background: #f3f6fa;
}

.dot {
  position: absolute;
  top: 0.6rem;
  left: 0.6rem;
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
}

.state-dot {
  background: #b42318;
}

.motion-dot {
  background: #174ea6;
}

.track-note {
  color: #475569;
  font-size: 0.9rem;
}
~~~

## Profile the comparison

1. Open the browser Performance panel and clear any previous recording.
2. Start a runtime recording and activate only the React state version.
3. Stop the recording and inspect frame activity, scripting, and any missed frames.
4. Save or note the result, then reload or reset the page.
5. Record the Motion Value version separately under the same conditions.
6. Use the React profiler to see whether the state-driven component rendered repeatedly.
7. Repeat with CPU throttling if the difference is not visible on the current machine.
8. Compare the traces instead of expecting a fixed frame rate or a universal number of milliseconds.

The state-driven component updates its progress text as well as its dot, so that example intentionally includes React rendering and text updates on each frame. The Motion Value example updates only its visual dot. This makes the difference easy to inspect, but it is not an apples-to-apples benchmark of the Motion engine. For a production decision, profile the actual component and interaction.

## Change one rendering property

After the first comparison, create a separate version that moves an element by changing its top position. Compare it with the transform version. Keep the same content, viewport, recording settings, and interaction. Observe layout and paint work in the trace.

Do not conclude that one property is always faster from a single small demo. Page complexity, effects, and browser behavior change the result.

## Troubleshooting

- **The state version does not stop:** Confirm the animation reaches progress 1 and the effect cancels its last scheduled animation frame when it ends or unmounts.
- **The Motion Value dot does not move:** Confirm the Motion Value is bound to a Motion component's style and that the click handler runs.
- **The traces look similar:** The example is intentionally small. Use CPU throttling or profile a representative page where rendering work is measurable.
- **The React profile includes extra development renders:** Development Strict Mode can add diagnostic renders. Compare the same mode for both versions and use production-like conditions for final measurements.
- **The reduced-motion setting removes the animation:** That is expected. The content and controls remain usable without movement.
- **The animation is still slow after replacing state:** Inspect layout, paint, image loading, and synchronous work. React render frequency is only one possible cause.

## Safety note for applying the pattern

The state-driven loop is intentionally a profiling comparison. Do not copy its per-frame React state update into a production visual animation. Use React state for meaningful application state and Motion Values for high-frequency visual values when they fit the interface.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion example</a></td><td><a href="../lessons/030-performance-and-debugging.md">030 - Performance and debugging lesson</a></td><td><a href="../qnas/030-performance-and-debugging.md">030 - Performance and debugging Q&amp;A</a></td><td><a href="031-testing-motion-components.md">031 - Testing Motion components example</a></td></tr></tbody>
</table>

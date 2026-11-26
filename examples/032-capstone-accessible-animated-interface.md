# Capstone: accessible animated interface example

This example builds a small resource page with an animated Quick guide disclosure. The control stays a native button, its state is exposed to assistive technology, and the guide remains available when reduced motion is enabled.

## What you will build

- A stable disclosure region controlled by a native button.
- A short entrance and exit using AnimatePresence.
- Escape-to-close behavior while focus is on the disclosure button.
- An exit state hidden from the accessibility tree.
- A reduced-motion version with no vertical movement and no transition delay.
- Component tests for state, keyboard use, and the aria-controls relationship.

## Requirements

Use a React application with Motion installed. For the example tests, install Vitest and React Testing Library packages:

~~~bash
npm install motion
npm install -D vitest jsdom @vitejs/plugin-react @testing-library/react @testing-library/user-event @testing-library/jest-dom
~~~

Merge the test setup into the existing Vite configuration if the project already has one.

## src/AccessibleGuide.jsx

~~~jsx
import { useState } from "react";
import {
  AnimatePresence,
  motion,
  useIsPresent,
  useReducedMotion,
} from "motion/react";
import "./accessible-guide.css";

function GuideContent({ reduceMotion }) {
  const isPresent = useIsPresent();
  const hidden = reduceMotion
    ? { opacity: 1, y: 0 }
    : { opacity: 0, y: -12 };

  return (
    <motion.div
      className="guide-card"
      aria-hidden={!isPresent}
      initial={hidden}
      animate={{ opacity: 1, y: 0 }}
      exit={hidden}
      transition={{
        duration: reduceMotion ? 0 : 0.2,
        ease: "easeOut",
      }}
    >
      <p className="guide-eyebrow">A simple review</p>
      <ol>
        <li>Decide what the motion should help the user understand.</li>
        <li>Keep the transition short and tied to a real state change.</li>
        <li>Check the interface with reduced motion enabled.</li>
      </ol>
    </motion.div>
  );
}

export function AccessibleGuide() {
  const [isOpen, setIsOpen] = useState(false);
  const shouldReduceMotion = useReducedMotion();
  const reduceMotion = shouldReduceMotion ?? false;

  function handleKeyDown(event) {
    if (event.key === "Escape" && isOpen) {
      event.preventDefault();
      setIsOpen(false);
    }
  }

  return (
    <main className="page-shell">
      <header className="page-header">
        <p className="eyebrow">Motion design</p>
        <h1>Build a clear animated interface</h1>
        <p className="intro">
          Use motion to clarify a change of state, then make sure the same
          content and action remain useful without movement.
        </p>

        <button
          className="guide-toggle"
          type="button"
          aria-expanded={isOpen}
          aria-controls="quick-guide"
          onClick={() => setIsOpen((current) => !current)}
          onKeyDown={handleKeyDown}
        >
          {isOpen ? "Hide quick guide" : "Show quick guide"}
        </button>
      </header>

      <section
        className="guide-region"
        id="quick-guide"
        role="region"
        aria-labelledby="quick-guide-title"
        aria-hidden={!isOpen}
      >
        <h2 className="visually-hidden" id="quick-guide-title">
          Quick guide
        </h2>
        <AnimatePresence initial={false}>
          {isOpen && (
            <GuideContent key="quick-guide-content" reduceMotion={reduceMotion} />
          )}
        </AnimatePresence>
      </section>

      <section className="resources" aria-labelledby="resources-title">
        <div className="section-heading">
          <p className="eyebrow">Your checklist</p>
          <h2 id="resources-title">Three decisions to make</h2>
        </div>

        <div className="resource-grid">
          <article className="resource-card">
            <p className="resource-number">01</p>
            <h3>Purpose</h3>
            <p>Connect each animation to a meaningful interface state.</p>
          </article>

          <article className="resource-card">
            <p className="resource-number">02</p>
            <h3>Motion policy</h3>
            <p>Respect the user's preference and remove unnecessary travel.</p>
          </article>

          <article className="resource-card">
            <p className="resource-number">03</p>
            <h3>Verification</h3>
            <p>Review content, focus, keyboard use, and narrow layouts.</p>
          </article>
        </div>
      </section>
    </main>
  );
}
~~~

The Quick guide container stays mounted, so the button always controls the same element. AnimatePresence only adds or removes the animated child. The container becomes aria-hidden as soon as it closes. The exiting child also uses useIsPresent to leave the accessibility tree during its exit.

The guide contains reading content but no links or buttons. That keeps keyboard focus on the disclosure button while it opens and closes. If interactive items are added to the guide, add a focus-management plan before using the same exit behavior.

## src/main.jsx

Set the user's reduced-motion policy around the application:

~~~jsx
import { createRoot } from "react-dom/client";
import { MotionConfig } from "motion/react";
import { AccessibleGuide } from "./AccessibleGuide";

createRoot(document.getElementById("root")).render(
  <MotionConfig reducedMotion="user">
    <AccessibleGuide />
  </MotionConfig>
);
~~~

The global policy is a baseline. The component also uses useReducedMotion to remove its custom vertical offset and make this particular transition immediate.

## src/accessible-guide.css

~~~css
:root {
  color-scheme: dark;
  font-family: Inter, ui-sans-serif, system-ui, sans-serif;
  color: #edf2f8;
  background: #101722;
  font-synthesis: none;
  text-rendering: optimizeLegibility;
}

* {
  box-sizing: border-box;
}

body {
  min-width: 320px;
  min-height: 100vh;
  margin: 0;
}

button {
  font: inherit;
}

.page-shell {
  width: min(100% - 2rem, 68rem);
  margin-inline: auto;
  padding-block: clamp(2rem, 7vw, 5.5rem);
}

.page-header {
  max-width: 48rem;
}

.eyebrow {
  margin: 0 0 0.65rem;
  color: #9ac9ff;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

h1,
h2,
h3,
p {
  margin-top: 0;
}

h1 {
  max-width: 15ch;
  margin-bottom: 1rem;
  font-size: clamp(2.2rem, 6vw, 4.5rem);
  line-height: 1.04;
  letter-spacing: -0.04em;
}

.intro {
  max-width: 58ch;
  margin-bottom: 1.5rem;
  color: #c1ccda;
  font-size: 1.1rem;
  line-height: 1.65;
}

.guide-toggle {
  min-height: 2.9rem;
  padding: 0.65rem 1rem;
  border: 1px solid #83bfff;
  border-radius: 0.55rem;
  background: #18304a;
  color: #f7fbff;
  font-weight: 700;
  cursor: pointer;
}

.guide-toggle:hover {
  background: #234365;
}

.guide-toggle:focus-visible {
  outline: 3px solid #f4c96b;
  outline-offset: 4px;
}

.guide-region {
  margin-top: 1.25rem;
}

.guide-card {
  max-width: 48rem;
  padding: clamp(1rem, 3vw, 1.5rem);
  border: 1px solid #46627e;
  border-radius: 0.8rem;
  background: #172536;
  box-shadow: 0 1rem 2.5rem rgb(0 0 0 / 20%);
}

.guide-card ol {
  margin-bottom: 0;
  padding-left: 1.35rem;
  color: #d9e4f0;
  line-height: 1.7;
}

.guide-eyebrow {
  margin-bottom: 0.5rem;
  color: #9ac9ff;
  font-weight: 700;
}

.resources {
  margin-top: clamp(3rem, 8vw, 6rem);
}

.section-heading {
  margin-bottom: 1.25rem;
}

.section-heading h2 {
  margin-bottom: 0;
  font-size: clamp(1.5rem, 3vw, 2.2rem);
}

.resource-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
}

.resource-card {
  min-width: 0;
  padding: 1.25rem;
  border: 1px solid #35485d;
  border-radius: 0.75rem;
  background: #151f2c;
}

.resource-card h3 {
  margin-bottom: 0.55rem;
  font-size: 1.15rem;
}

.resource-card p:last-child {
  margin-bottom: 0;
  color: #c1ccda;
  line-height: 1.55;
}

.resource-number {
  margin-bottom: 0.8rem;
  color: #9ac9ff;
  font-size: 0.85rem;
  font-weight: 700;
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

@media (max-width: 42rem) {
  .resource-grid {
    grid-template-columns: 1fr;
  }
}
~~~

## Component tests

These tests protect semantics and keyboard behavior. They do not attempt to sample an intermediate transform.

### src/test/setup.js

~~~js
import "@testing-library/jest-dom/vitest";

let reducedMotion = false;

Object.defineProperty(window, "matchMedia", {
  configurable: true,
  writable: true,
  value: (query) => ({
    matches:
      reducedMotion && query.includes("prefers-reduced-motion"),
    media: query,
    onchange: null,
    addListener() {},
    removeListener() {},
    addEventListener() {},
    removeEventListener() {},
    dispatchEvent() {
      return false;
    },
  }),
});
~~~

The mock supplies a predictable no-preference response for this component suite. A browser check below covers the real operating-system preference. Keep media-query mocks in test setup rather than production code.

### vitest.config.js

~~~js
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./src/test/setup.js"],
    restoreMocks: true,
  },
});
~~~

Merge the test settings into an existing Vite configuration if one is already present.

### src/AccessibleGuide.test.jsx

~~~jsx
import { afterEach, describe, expect, it } from "vitest";
import { cleanup, render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { MotionConfig } from "motion/react";
import { AccessibleGuide } from "./AccessibleGuide";

afterEach(() => {
  cleanup();
});

function renderGuide() {
  return render(
    <MotionConfig reducedMotion="always">
      <AccessibleGuide />
    </MotionConfig>
  );
}

describe("AccessibleGuide", () => {
  it("starts collapsed and opens the controlled region", async () => {
    const user = userEvent.setup();
    renderGuide();

    const button = screen.getByRole("button", { name: "Show quick guide" });
    expect(button).toHaveAttribute("aria-expanded", "false");
    expect(button).toHaveAttribute("aria-controls", "quick-guide");
    expect(document.getElementById("quick-guide")).toHaveAttribute(
      "aria-hidden",
      "true"
    );

    await user.click(button);

    const openButton = screen.getByRole("button", { name: "Hide quick guide" });
    expect(openButton).toHaveAttribute("aria-expanded", "true");
    expect(document.getElementById("quick-guide")).toHaveAttribute(
      "aria-hidden",
      "false"
    );
    expect(screen.getByText(/Decide what the motion should help/)).toBeVisible();
  });

  it("toggles with the keyboard and closes with Escape", async () => {
    const user = userEvent.setup();
    renderGuide();

    await user.tab();
    const button = screen.getByRole("button", { name: "Show quick guide" });
    expect(button).toHaveFocus();

    await user.keyboard("{Enter}");
    const openButton = screen.getByRole("button", { name: "Hide quick guide" });
    expect(openButton).toHaveAttribute("aria-expanded", "true");
    expect(openButton).toHaveFocus();

    await user.keyboard("{Escape}");
    const closedButton = screen.getByRole("button", { name: "Show quick guide" });
    expect(closedButton).toHaveAttribute("aria-expanded", "false");
    expect(closedButton).toHaveFocus();
  });

  it("also supports Space activation", async () => {
    const user = userEvent.setup();
    renderGuide();

    await user.tab();
    await user.keyboard(" ");

    expect(
      screen.getByRole("button", { name: "Hide quick guide" })
    ).toHaveAttribute("aria-expanded", "true");
  });
});
~~~

Add or merge the following test script in the project's package.json:

~~~json
{
  "scripts": {
    "test": "vitest run"
  }
}
~~~

Run the suite:

~~~bash
npm run test
~~~

The component tests verify the state contract, the persistent aria-controls target, keyboard activation, and Escape behavior. The reduced-motion provider removes transform and layout animation from this semantic test. The tests do not prove the rendered appearance or browser preference integration.

## Browser review

Run the page in a browser and review each item:

1. Tab to the disclosure and confirm the focus outline is visible.
2. Open it with Enter, then close it with Escape.
3. Open it with Space and close it with a pointer.
4. Enable the operating system or browser reduced-motion preference and reload the page.
5. Repeat the interaction. The guide must still open, while its vertical travel and transition delay are removed.
6. Check a narrow viewport and browser zoom. The cards should stack and text should remain readable.
7. Confirm the closed guide is not exposed to a screen reader and that the open guide has the name "Quick guide".
8. Confirm repeated open and close actions do not leave stale content or focus.

## Why the example is structured this way

- The native button supplies expected activation behavior.
- aria-expanded describes the current disclosure state.
- aria-controls points at a container that remains mounted.
- aria-hidden changes immediately with the disclosure state.
- useIsPresent marks the exiting animated child as unavailable to assistive technology.
- The guide has no focusable descendants, so focus stays on its trigger through the exit.
- MotionConfig provides a global reduced-motion policy.
- useReducedMotion supplies the component-specific no-travel, zero-duration version.
- CSS keeps layout, readable type, and visible focus independent of animation.

If the guide later gains links or buttons, update the focus and exit design before adding those elements. A hidden or exiting subtree must not keep keyboard focus.

## Troubleshooting

- **The guide region cannot be found:** Check that the section with id quick-guide is always rendered and the button's aria-controls matches it exactly.
- **The control state is stale:** Derive aria-expanded and the button name from the same React state that controls the guide.
- **Escape does not close the guide:** Keep focus on the trigger while open, or move the handler to a deliberate focus-managed container if the interface changes.
- **An exiting item is announced after close:** Confirm the wrapper becomes aria-hidden immediately and the child reads useIsPresent during exit.
- **Reduced motion still moves the guide:** Confirm the app root has MotionConfig reducedMotion="user" and the component uses the reduced-motion branch for its custom offset.
- **A test hangs on animation:** Assert the semantic state in component tests. Reserve actual transition timing for a browser test.
- **The layout overflows on mobile:** Check the grid's minmax(0, 1fr), page width, and long unbroken text.
- **Focus is hard to see:** Keep the focus-visible outline and verify its contrast against the button and surrounding background.

## Completion checklist

- The disclosure works with pointer, Enter, and Space.
- The current state is communicated with aria-expanded.
- The controlled region remains a stable aria-controls target.
- Escape closes the guide while focus is on its button.
- The exiting child is removed from the accessibility tree.
- Reduced motion preserves all content and actions with no vertical movement or delay.
- The static page remains readable at narrow widths and zoom.
- Tests cover user-facing semantics, while a browser review covers rendering and preferences.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="../examples/031-testing-motion-components.md">031 - Testing Motion components example</a></td><td><a href="../lessons/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface lesson</a></td><td><a href="../qnas/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface Q&amp;A</a></td><td>End of roadmap</td></tr></tbody>
</table>
# Testing Motion components example

This example tests a favorite button whose decorative icon animates when the user saves an item. The tests verify the button's accessible name, pressed state, status message, and keyboard operation. They do not assert an intermediate transform value.

## What you will build

- A real Motion component with a decorative animated icon.
- A Vitest and React Testing Library setup.
- Tests for initial state, pointer activation, keyboard activation, and toggling back.
- A browser checklist for reduced motion and visual behavior a DOM test cannot prove.

## Requirements

Use a React project with Motion installed. This sample uses Vitest, jsdom, React Testing Library, user-event, and jest-dom matchers.

~~~bash
npm install motion
npm install -D vitest jsdom @vitejs/plugin-react @testing-library/react @testing-library/user-event @testing-library/jest-dom
~~~

If the project already has Vite, merge the test configuration into its existing config instead of creating a second Vite configuration.

## src/FavoriteButton.jsx

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

export function FavoriteButton() {
  const [isSaved, setIsSaved] = useState(false);

  return (
    <div className="favorite-control">
      <button
        type="button"
        aria-pressed={isSaved}
        onClick={() => setIsSaved((current) => !current)}
      >
        <motion.span
          className="favorite-icon"
          aria-hidden="true"
          initial={false}
          animate={{ scale: isSaved ? 1.18 : 1 }}
          transition={{ duration: 0.18, ease: "easeOut" }}
        >
          ♥
        </motion.span>
        <span>{isSaved ? "Saved" : "Save"}</span>
      </button>

      <p className="sr-only" role="status" aria-live="polite">
        {isSaved ? "Saved to favorites." : ""}
      </p>
    </div>
  );
}
~~~

The native button provides keyboard activation and focus behavior. The changing label and aria-pressed communicate the toggle state. The heart is decorative, so it is hidden from the accessibility tree. The status message announces the result without requiring the user to move focus.

## src/App.jsx

Set the reduced-motion policy once near the application root. The button still works when movement is reduced.

~~~jsx
import { MotionConfig } from "motion/react";
import { FavoriteButton } from "./FavoriteButton";

export default function App() {
  return (
    <MotionConfig reducedMotion="user">
      <main>
        <h1>Saved items</h1>
        <FavoriteButton />
      </main>
    </MotionConfig>
  );
}
~~~

## src/favorite-button.css

~~~css
.favorite-control {
  display: grid;
  justify-items: start;
  gap: 0.5rem;
}

.favorite-control button {
  display: inline-flex;
  align-items: center;
  gap: 0.55rem;
  min-height: 2.75rem;
  padding: 0.55rem 0.9rem;
  border: 1px solid #536174;
  border-radius: 0.5rem;
  background: #172033;
  color: #f7f9fc;
  font: inherit;
  cursor: pointer;
}

.favorite-control button:focus-visible {
  outline: 3px solid #7dc4ff;
  outline-offset: 3px;
}

.favorite-icon {
  color: #ff8aa2;
}

.sr-only {
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
~~~

Import the stylesheet from the application entry point or directly from the component.

## vitest.config.js

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

If the project already has a Vite config, add the test settings to that config. Keep the environment explicit so the suite does not accidentally run with a Node-only environment.

## src/test/setup.js

~~~js
import "@testing-library/jest-dom/vitest";
~~~

## package.json test script

Add or merge this script in the existing scripts object:

~~~json
{
  "scripts": {
    "test": "vitest run"
  }
}
~~~

Keep the other scripts already in the project. The snippet shows only the relevant entry.

## src/FavoriteButton.test.jsx

~~~jsx
import { afterEach, describe, expect, it } from "vitest";
import { cleanup, render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { MotionConfig } from "motion/react";
import { FavoriteButton } from "./FavoriteButton";

afterEach(() => {
  cleanup();
});

function renderWithReducedMotion(ui) {
  return render(
    <MotionConfig reducedMotion="always">
      {ui}
    </MotionConfig>
  );
}

describe("FavoriteButton", () => {
  it("starts unsaved and announces a save after activation", async () => {
    const user = userEvent.setup();
    renderWithReducedMotion(<FavoriteButton />);

    const saveButton = screen.getByRole("button", { name: "Save" });
    expect(saveButton).toHaveAttribute("aria-pressed", "false");
    expect(screen.getByRole("status")).toHaveTextContent("");

    await user.click(saveButton);

    const savedButton = screen.getByRole("button", { name: "Saved" });
    expect(savedButton).toHaveAttribute("aria-pressed", "true");
    expect(screen.getByRole("status")).toHaveTextContent(
      "Saved to favorites."
    );
  });

  it("can be activated with the keyboard and toggled off", async () => {
    const user = userEvent.setup();
    renderWithReducedMotion(<FavoriteButton />);

    await user.tab();
    const saveButton = screen.getByRole("button", { name: "Save" });
    expect(saveButton).toHaveFocus();

    await user.keyboard("{Enter}");

    const savedButton = screen.getByRole("button", { name: "Saved" });
    expect(savedButton).toHaveAttribute("aria-pressed", "true");
    expect(savedButton).toHaveFocus();

    await user.keyboard(" ");

    const restoredButton = screen.getByRole("button", { name: "Save" });
    expect(restoredButton).toHaveAttribute("aria-pressed", "false");
    expect(screen.getByRole("status")).toHaveTextContent("");
  });
});
~~~

The provider makes the component test independent of transform timing. The assertions still exercise the real React component and Motion element. They check behavior a person can observe, not an implementation-specific style string.

## Add the test script and run it

~~~bash
npm run test
~~~

A successful run confirms the component rendered in the configured DOM environment, user interactions changed the toggle state, and the accessibility contract stayed intact. It does not confirm exact rendered animation, page performance, or real browser focus behavior.

## Browser verification

Run the application in a real browser and check the parts the component suite cannot prove:

1. Tab to the button and confirm the visible focus outline.
2. Activate it with Enter and Space.
3. Confirm the label, pressed state, and status announcement make sense with a screen reader.
4. Enable the operating system's reduced-motion preference and reload the page.
5. Activate the button again. Saving must still work, while the scale movement should be removed or restrained by the global Motion policy.
6. Check the interaction at a narrow viewport and at browser zoom.
7. If this control is part of a larger page, profile a representative interaction before making performance claims.

Do not require the icon to reach a particular computed transform in a component test. Inspect browser output when the visible animation itself is under review.

## Troubleshooting

- **The matcher toHaveAttribute is missing:** Confirm the setup file imports the Vitest entry point from jest-dom and that its path matches the config.
- **JSX fails to transform:** Confirm the React plugin is installed and included in the existing Vite configuration.
- **The suite has no DOM:** Confirm the Vitest environment is jsdom and the package is installed.
- **A test cannot find the button:** Keep the visible label and accessible name synchronized. The decorative heart should remain aria-hidden.
- **The test waits for an animation:** Keep the assertion on semantic state. Use a real browser if transition completion is itself required.
- **The icon still moves in the test:** Confirm the component is rendered inside the test's MotionConfig reducedMotion="always" provider.
- **Keyboard focus disappears:** Do not replace the native button with a generic element. Preserve a visible focus style.
- **Reduced motion does not affect the app:** Confirm the root MotionConfig is mounted around the animated component and the system or browser preference is enabled.

## Review checklist

- The control is a native button with a changing accessible name.
- aria-pressed reflects the current toggle state.
- The icon is decorative and excluded from the accessible name.
- A status message communicates the result.
- Pointer and keyboard tests cover the same state transition.
- The tests do not inspect arbitrary intermediate transforms.
- Reduced-motion behavior is checked in a browser as well as protected by the app policy.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="030-performance-and-debugging.md">030 - Performance and debugging example</a></td><td><a href="../lessons/031-testing-motion-components.md">031 - Testing Motion components lesson</a></td><td><a href="../qnas/031-testing-motion-components.md">031 - Testing Motion components Q&amp;A</a></td><td><a href="032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface example</a></td></tr></tbody>
</table>
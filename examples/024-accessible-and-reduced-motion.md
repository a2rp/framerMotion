# Accessible and reduced motion example

This notice uses a global user preference policy and a custom `useReducedMotion` choice. It keeps a semantic button, exposes its expanded state, and uses a short fade instead of spatial movement when reduced motion is requested.

## Setup

Install Motion with `npm install motion`. Save as `src/ReducedMotionNotice.jsx` and render `<ReducedMotionNotice />`.

## `src/ReducedMotionNotice.jsx`

~~~jsx
import { useState } from "react";
import {
  AnimatePresence,
  MotionConfig,
  motion,
  useReducedMotion,
} from "motion/react";

function NoticeControl() {
  const [open, setOpen] = useState(false);
  const shouldReduceMotion = useReducedMotion();
  const travel = shouldReduceMotion ? 0 : 14;

  return (
    <main
      style={{
        maxWidth: 640,
        margin: "64px auto",
        padding: 24,
        color: "#172033",
        fontFamily: "system-ui, sans-serif",
        lineHeight: 1.6,
      }}
    >
      <h1>Reduced-motion friendly notice</h1>
      <p>
        Open and close the notice. With reduced motion enabled, it fades in
        place instead of moving across the screen.
      </p>

      <button
        type="button"
        aria-expanded={open}
        aria-controls="reading-notice"
        onClick={() => setOpen((value) => !value)}
      >
        {open ? "Hide notice" : "Show notice"}
      </button>

      <div id="reading-notice">
        <AnimatePresence initial={false}>
        {open && (
          <motion.aside
            key="reading-notice"
            role="status"
            initial={{ opacity: 0, y: travel }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: shouldReduceMotion ? 0 : -travel }}
            transition={{
              duration: shouldReduceMotion ? 0 : 0.24,
              ease: "easeOut",
            }}
            style={{
              marginTop: 20,
              padding: 20,
              borderRadius: 14,
              border: "1px solid #c4b5fd",
              background: "#f5f3ff",
            }}
          >
            <h2>Reading preference saved</h2>
            <p>
              This notice remains understandable without its transition. The
              button retains focus and reports whether the notice is expanded.
            </p>
          </motion.aside>
        )}
      </AnimatePresence>
      </div>
    </main>
  );
}

export function ReducedMotionNotice() {
  return (
    <MotionConfig reducedMotion="user">
      <NoticeControl />
    </MotionConfig>
  );
}
~~~

## How it works

The outer MotionConfig sets a site policy that respects the operating-system preference. The component also reads `useReducedMotion` to choose a zero travel distance and immediate timing when reduced motion is enabled. This explicit choice adapts the opacity transition too, since opacity can otherwise continue under Motion's automatic reduced-motion policy.

The button communicates the open state through `aria-expanded` and toggles the notice without moving focus. The notice's text exists independently of animation; motion only changes its entrance and exit presentation.

## Expected behavior

- The button opens and closes the notice.
- The button exposes the current state with `aria-expanded`.
- With normal motion, the notice moves a small distance and fades.
- With reduced motion, it fades in place immediately.
- The button keeps keyboard focus after activation.

## Adapt the example

- Replace the notice's transition with an immediate state update and compare clarity.
- Add a close button inside the notice only if the interaction needs one.
- Apply the same preference to any CSS animation or auto-playing media used nearby.
- Keep focus management within the dialog/menu logic if the notice becomes an interactive modal.
- Use a visible text state if color or animation currently carries all the meaning.

## Common mistakes

- **Only wrapping in MotionConfig:** remember custom CSS, video, and other effects need separate handling.
- **Assuming all properties stop:** opacity may continue unless explicitly adapted.
- **Moving focus for visual reasons:** keep focus with the activated control unless interaction semantics require otherwise.
- **Content depends on entrance:** keep information meaningful without the transition.
- **State not announced:** use suitable semantics for meaningful state, not frame-by-frame live updates.
- **Preference ignored in custom sequence:** read `useReducedMotion` where explicit adaptation is needed.

## Practice

1. Test both system preference modes.
2. Remove y movement and compare the interaction.
3. Make opacity immediate under reduced motion.
4. Add a CSS animation and respond to the media query separately.
5. Confirm keyboard focus and expanded state remain correct after opening and closing.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig example</a></td><td><a href="../lessons/024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion lesson</a></td><td><a href="../qnas/024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion Q&amp;A</a></td><td><a href="025-responsive-motion-patterns.md">025 - Responsive motion patterns example</a></td></tr></tbody>
</table>

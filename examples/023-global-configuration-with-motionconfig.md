# Global configuration with MotionConfig example

This app wraps a small interface in MotionConfig. Child components share a spring fallback and respect the system reduced-motion preference. One card uses a local opacity transition that inherits the global default for other values.

## Setup

Install Motion with `npm install motion`. Save as `src/MotionConfigDemo.jsx` and render `<MotionConfigDemo />`.

## `src/MotionConfigDemo.jsx`

~~~jsx
import { useState } from "react";
import { motion, MotionConfig } from "motion/react";

function DemoContent() {
  const [open, setOpen] = useState(false);

  return (
    <main
      style={{
        maxWidth: 680,
        margin: "64px auto",
        padding: 24,
        color: "#172033",
        fontFamily: "system-ui, sans-serif",
        lineHeight: 1.6,
      }}
    >
      <h1>Shared Motion defaults</h1>
      <p>
        The card and button inherit the app's fallback transition and reduced
        motion policy.
      </p>

      <motion.section
        initial={{ opacity: 0, y: 18 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{
          inherit: true,
          opacity: { duration: 0.2 },
        }}
        style={{
          padding: 24,
          borderRadius: 16,
          border: "1px solid #cbd5e1",
          background: "#f8fafc",
        }}
      >
        <h2>Configured card</h2>
        <p>
          The local opacity duration is shorter. The y animation inherits the
          global spring transition because inherit is enabled.
        </p>
        <motion.button
          type="button"
          aria-expanded={open}
          onClick={() => setOpen((value) => !value)}
          whileTap={{ scale: 0.97 }}
          style={{
            padding: "10px 14px",
            border: 0,
            borderRadius: 8,
            background: "#6d28d9",
            color: "white",
            font: "inherit",
            cursor: "pointer",
          }}
        >
          {open ? "Hide details" : "Show details"}
        </motion.button>
        {open && (
          <motion.p
            initial={{ opacity: 0, y: 8 }}
            animate={{ opacity: 1, y: 0 }}
          >
            This content uses the same fallback transition.
          </motion.p>
        )}
      </motion.section>
    </main>
  );
}

export function MotionConfigDemo() {
  return (
    <MotionConfig
      transition={{ type: "spring", stiffness: 280, damping: 26 }}
      reducedMotion="user"
    >
      <DemoContent />
    </MotionConfig>
  );
}
~~~

## How it works

`MotionConfig` provides one transition fallback and a reduced-motion policy to the descendants. The card's local transition sets a short opacity duration and enables `inherit` so other properties retain the global spring. The detail paragraph has no local transition and uses the fallback.

The user preference policy is `"user"`. Motion automatically reduces transform and layout animation when the system preference requests it, while opacity can continue. The button remains a normal semantic control with an expanded state.

## Content Security Policy nonce

If the application uses a CSP nonce for inline styles, pass the page's actual nonce to MotionConfig:

~~~jsx
<MotionConfig nonce={styleNonce}>
  <App />
</MotionConfig>
~~~

Do not invent a nonce value. It must match the site's security policy.

## Expected behavior

- The card enters with the shared spring for vertical movement.
- Its opacity uses the shorter local duration.
- The button and revealed detail use the shared fallback.
- Reduced-motion preferences suppress transform movement while preserving the interface.
- The button remains keyboard operable and reports its expanded state.

## Adapt the example

- Remove `inherit: true` and compare how the local transition behaves.
- Set `reducedMotion="always"` temporarily to inspect that mode.
- Add another local transition for a specific value and document why it differs.
- Add `nonce` only if the app uses a matching CSP setup.
- Keep non-Motion effects under their own reduced-motion handling.

## Common mistakes

- **Provider does not apply:** make sure the Motion components are descendants of MotionConfig.
- **Local default is missing:** add `inherit: true` when local and global transitions should combine.
- **All motion expected to stop:** transform/layout suppression does not automatically remove every property animation.
- **CSP still blocks a style:** use the same valid nonce as the page policy.
- **Pointer math is wrong:** avoid a guessed transformPagePoint scale.
- **Too many boundaries:** keep configuration providers limited to meaningful subtree differences.

## Practice

1. Change the fallback spring and compare the card and detail.
2. Remove inheritance and observe the local transition.
3. Inspect the interface with reduced motion enabled.
4. Add a CSS animation and decide how it should respond independently.
5. If using CSP, verify the correct nonce is passed rather than inserting a placeholder.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() example</a></td><td><a href="../lessons/023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig lesson</a></td><td><a href="../qnas/023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig Q&amp;A</a></td><td><a href="024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion example</a></td></tr></tbody>
</table>

# Custom React components with motion.create() example

This example wraps a reusable button component. It forwards its ref and style to the native button, then a Motion wrapper animates its selected state and press feedback.

## Setup

Install Motion with `npm install motion`. Save as `src/MotionActionButton.jsx` and render `<MotionActionButtonDemo />`.

## `src/MotionActionButton.jsx`

~~~jsx
import { forwardRef, useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const buttonBaseStyle = {
  padding: "12px 18px",
  border: 0,
  borderRadius: 10,
  color: "white",
  font: "inherit",
  fontWeight: 700,
  cursor: "pointer",
};

const ActionButton = forwardRef(function ActionButton(
  { style, className, ...props },
  ref
) {
  return (
    <button
      ref={ref}
      className={className}
      style={{ ...buttonBaseStyle, ...style }}
      {...props}
    />
  );
});

// Create once at module scope, not inside the component.
const MotionActionButton = motion.create(ActionButton);

export function MotionActionButtonDemo() {
  const [selected, setSelected] = useState(false);
  const shouldReduceMotion = useReducedMotion();

  return (
    <main
      style={{
        maxWidth: 600,
        margin: "72px auto",
        padding: 24,
        fontFamily: "system-ui, sans-serif",
        color: "#172033",
      }}
    >
      <h1>A reusable motion button</h1>
      <p>
        The button's React component keeps its semantic behavior while Motion
        adds animated visual states.
      </p>

      <MotionActionButton
        type="button"
        aria-pressed={selected}
        onClick={() => setSelected((value) => !value)}
        initial={false}
        animate={{
          backgroundColor: selected ? "#0f766e" : "#6d28d9",
          scale: selected ? 1.03 : 1,
        }}
        whileTap={{ scale: shouldReduceMotion ? 1 : 0.96 }}
        transition={{
          duration: shouldReduceMotion ? 0 : 0.25,
          ease: "easeOut",
        }}
        style={{
          boxShadow: "0 8px 20px rgb(15 23 42 / 18%)",
        }}
      >
        {selected ? "Following" : "Follow"}
      </MotionActionButton>

      <p>
        State: <strong>{selected ? "Following" : "Not following"}</strong>
      </p>
      <p>
        The button remains keyboard operable and exposes its selected state
        through aria-pressed.
      </p>
    </main>
  );
}
~~~

## How it works

`ActionButton` receives ordinary button props, a ref, and a style object. It attaches the ref to the native button and merges Motion's style after its base styles. `motion.create(ActionButton)` is evaluated once at module scope, preserving a stable component identity through renders.

React state selects the button's label, pressed state, color, and scale. Motion animates the visual values. Motion-specific props are filtered by default, while native props such as `type`, `aria-pressed`, and `onClick` remain part of the button behavior.

## Expected behavior

- The control renders as a semantic button.
- Activating it toggles the Follow/Following label and pressed state.
- Motion transitions the background color and scale.
- The style prop reaches the same DOM button as the forwarded ref.
- The button works with keyboard activation.

## Adapt the example

- Add a disabled prop and style a disabled state without forwarding custom props to the DOM.
- Add a className and confirm it reaches the button.
- Add a second variant prop, consume it in ActionButton, and avoid spreading it to HTML.
- Replace the custom component with `motion.button` and compare the amount of wrapper code.
- Respect reduced motion for larger travel while preserving the same state.

## Common mistakes

- **Ref points to a component instance:** forward it to the actual button or SVG element.
- **Motion style disappears:** merge the incoming style onto the referenced element.
- **Wrapper recreates on render:** define `motion.create()` outside the component.
- **Custom prop leaks to HTML:** consume design-system props before spreading native props.
- **Motion prop leaks to HTML:** avoid `forwardMotionProps` unless it is needed.
- **Built-in element would suffice:** use `motion.button` for a one-off button.
- **Control lacks semantics:** preserve name, type, keyboard behavior, and pressed/disabled state.

## Practice

1. Remove the ref forwarding and observe why Motion cannot target the button correctly.
2. Remove style forwarding and inspect which animated styles stop applying.
3. Add a custom visual prop and consume it without passing it to the DOM.
4. Compare against a built-in `motion.button`.
5. Enable reduced motion and confirm state and keyboard operation still work.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="021-css-variables-and-complex-values.md">021 - CSS variables and complex values example</a></td><td><a href="../lessons/022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() lesson</a></td><td><a href="../qnas/022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() Q&amp;A</a></td><td><a href="023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig example</a></td></tr></tbody>
</table>

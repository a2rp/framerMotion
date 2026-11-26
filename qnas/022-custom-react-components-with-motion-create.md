# Custom React components with motion.create() Q&A

## 1. What does motion.create() do?

It wraps a React component so it can accept Motion animation, gesture, and layout props alongside its original props.

## 2. What must the wrapped component forward?

It must forward a ref to the HTML or SVG element Motion should animate and apply the provided style prop to that same element.

## 3. How do I forward a ref in React 18?

Use `forwardRef` and attach the received ref to the target DOM element.

## 4. How does React 19 pass ref?

React 19 can pass ref through props. `forwardRef` remains useful when supporting React 18 and makes the target explicit.

## 5. Can the ref point to another React component?

Only if that component ultimately forwards it to an HTML or SVG element. Motion needs an element to style, measure, and attach interactions to.

## 6. Why is style required?

Motion applies animated visual values through style. If the component drops or replaces the style prop, Motion cannot reliably render those values.

## 7. Where should I call motion.create()?

At module scope, outside React render. Recreating the wrapper on every render changes component identity and can remount its contents.

## 8. What happens to Motion props by default?

The wrapper filters Motion-specific props from those forwarded to the wrapped component. Ordinary props such as className and onClick still belong to the component API.

## 9. What does forwardMotionProps do?

It asks the wrapper to pass Motion props to the custom component. Use it only if that component needs to inspect those props, and filter them before they reach a native DOM element.

## 10. Can I wrap a built-in button?

You can, but `motion.button` is simpler unless a reusable component boundary or design-system API is needed.

## 11. What if a third-party component does not forward a DOM ref?

Animate a wrapper element you control, or choose a component that exposes the needed ref and style behavior.

## 12. Should a custom prop be spread to the DOM?

Only if it is a valid DOM attribute. Consume custom design-system props inside the component and pass valid HTML attributes onward.

## 13. How should default and incoming styles be merged?

Apply base styles first and the incoming Motion style afterward when the incoming value should take precedence.

## 14. Can the wrapper use variants and gestures?

Yes. The wrapped component receives Motion props such as animate, variants, whileHover, whileTap, and layout.

## 15. Why does layout animation use the wrong dimensions?

The forwarded ref may be attached to a different element from the one whose layout should animate. Forward ref and style to the intended measured node.

## 16. What ref type should I use in TypeScript?

Use the DOM element type corresponding to the rendered element, such as `HTMLButtonElement` for a button. Describe the component's ordinary props and style prop.

## 17. Should I use any to fix a TypeScript error?

No. First inspect the component's ref and prop types. A cast does not fix dropped refs or styles at runtime.

## 18. Does a motion wrapper add accessibility?

No. The underlying control still needs correct semantics, an accessible name, keyboard behavior, and appropriate state.

## 19. How should reduced motion affect a custom component?

Reduce or remove large movement and scale while preserving the control's final state, semantics, and keyboard behavior.

## 20. Why does the animation restart after every state update?

The wrapped component may be recreated during render. Move `motion.create()` to module scope and keep keys stable.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="021-css-variables-and-complex-values.md">021 - CSS variables and complex values Q&amp;A</a></td><td><a href="../lessons/022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() lesson</a></td><td><a href="../examples/022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() example</a></td><td><a href="023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig Q&amp;A</a></td></tr></tbody>
</table>

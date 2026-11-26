# Custom React components with motion.create()

A reusable React component can receive Motion animation props when it forwards the ref and style to the HTML or SVG element Motion should animate. Motion's `motion.create()` wraps that component and adds the Motion component API.

This is useful when a design system component should keep its existing React props while also supporting animation, gestures, or layout measurement. For a one-off element, a built-in element such as `motion.button` is simpler.

## Build a ref-forwarding component

With React 18, use `forwardRef` so Motion receives the actual DOM element:

~~~jsx
import { forwardRef } from "react";

const ActionButton = forwardRef(function ActionButton(
  { style, ...props },
  ref
) {
  return <button ref={ref} style={style} {...props} />;
});
~~~

The ref must reach the HTML or SVG element Motion will animate. Do not forward it only to another custom component unless that component also resolves the ref to the DOM node.

The component must accept the `style` prop and apply it to that same element. Motion uses style values to render animated styles. If the component drops or overwrites `style`, Motion may be unable to apply animated values.

Spread ordinary props onto the element so `className`, accessibility props, event handlers, and button attributes work:

~~~jsx
return (
  <button
    ref={ref}
    className={className}
    style={{ ...baseStyle, ...style }}
    {...props}
  />
);
~~~

When combining defaults, put the incoming style after defaults so it can override them intentionally.

React 19 can pass ref through props. A `forwardRef` component remains useful for React 18 compatibility and makes the ref destination explicit.

## Create the Motion wrapper once

Create the wrapped component at module scope:

~~~jsx
const MotionActionButton = motion.create(ActionButton);
~~~

Do not call `motion.create()` inside a component render. It creates a new component identity; recreating it on each render can remount the subtree and interrupt animation state.

The wrapper accepts the original component's props plus Motion props such as `animate`, `initial`, `whileHover`, `whileTap`, and `layout`:

~~~jsx
<MotionActionButton
  type="button"
  whileTap={{ scale: 0.96 }}
  animate={{ opacity: 1 }}
>
  Save
</MotionActionButton>
~~~

Motion filters its animation props from the wrapped component by default. The wrapper still needs to pass ordinary React props and Motion's injected `style` and `ref` to its target element. Use `forwardMotionProps: true` only when the wrapped component itself must receive Motion-specific props; if you do, prevent those props from leaking as invalid HTML attributes.

## Style ownership and prop boundaries

A custom component has two APIs to manage:

1. **Its public component props:** for example, label, disabled, variant, and className.
2. **Motion props:** for example, animate, transition, and whileHover.

Keep the public API intentional. Do not spread custom design-system props onto a native element if they are not valid DOM attributes. Consume a custom prop inside the component, then pass only valid DOM props to the element.

Apply Motion's `style` to the DOM node that receives the ref. If the component applies inline styles to a different descendant, the animated value and the measured element can diverge.

Use stable wrapper definitions so React preserves the component instance. For third-party components that do not forward a DOM ref or style, animate a wrapper element that you own instead.

## Choose a built-in element or a custom wrapper

| Need | Approach |
| --- | --- |
| Animate a standard button or div | `motion.button` or `motion.div` |
| Reuse a styled component with existing props | `motion.create(Component)` |
| Custom component exposes no DOM ref | Animate a DOM wrapper you control |
| Custom component must inspect Motion-only props | Consider `forwardMotionProps` and filter before DOM |
| Build a custom HTML tag | `motion.create("custom-element")`, when custom-element behavior is intended |

Wrapping a built-in element just to rename it may add unnecessary complexity. Use `motion.create()` when a real component boundary or reusable API makes it valuable.

## TypeScript considerations

For TypeScript, describe the component's ordinary props and DOM ref type so the ref points to the correct element. A button generally forwards `HTMLButtonElement`; a div forwards `HTMLDivElement`. Include `style` in the component props. Let Motion's wrapper infer animation props when possible, and avoid widening everything to `any`.

If the library's types reject a valid Motion prop, inspect the underlying component's ref and prop types before casting. A cast cannot repair a component that drops the ref or style at runtime.

## Accessibility and behavior

Motion does not add semantics to the component. A wrapped button still needs a type, accessible name, keyboard operation, and appropriate disabled or pressed state. Do not replace a semantic button with a styled div for convenience.

Respect reduced motion for large movement or scale. Keep focus indicators and hit targets stable, and ensure an animation does not change the meaning of props. A custom component should work before the animation starts and when animation is disabled.

## Troubleshooting

1. **Motion reports the ref is not an element:** forward the ref to an actual HTML or SVG node.
2. **The ref points to a class instance:** use a function component that forwards to a DOM element or animate a wrapper.
3. **Styles do not animate:** accept `style` and apply it to the referenced element.
4. **A prop disappears:** check whether it is a normal component prop or a Motion prop filtered by the wrapper.
5. **Motion props appear on the DOM:** avoid enabling `forwardMotionProps` unnecessarily and filter non-DOM props.
6. **Animation resets on rerender:** move `motion.create()` outside render.
7. **Layout animation measures the wrong node:** ensure ref and style reach the element whose layout should be measured.
8. **A third-party component refuses the ref:** animate a wrapper element that you control.

## Recap and practice

`motion.create()` adapts a reusable React component, provided its ref and style reach the intended DOM element. Define the wrapper at module scope, keep ordinary and Motion props distinct, and prefer a built-in Motion element when no custom boundary is needed.

Practice by wrapping a small button component. Animate its pressed state, inspect the forwarded style and ref path, then remove ref forwarding to understand the failure. Finally, test the same control with reduced motion.

Continue with [global configuration with MotionConfig](023-global-configuration-with-motionconfig.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="021-css-variables-and-complex-values.md">021 - CSS variables and complex values</a></td><td><a href="../qnas/022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() Q&amp;A</a></td><td><a href="../examples/022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() example</a></td><td><a href="023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig</a></td></tr></tbody>
</table>

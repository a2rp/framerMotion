# CSS variables and complex values

Motion can animate CSS custom properties and CSS strings containing numbers, colors, and units. Use a custom property when one shared value should affect several descendants; use complex interpolation for values such as shadows or filters.

Use direct Motion props or Motion values for ordinary visual changes. Keep string endpoints structurally compatible and consider the paint cost of custom properties.

## Animate a CSS custom property

Custom properties begin with two hyphens and are read through `var()`:

~~~jsx
<motion.ul
  initial={{ "--turn": "0deg" }}
  animate={{ "--turn": "360deg" }}
  transition={{ duration: 0.8 }}
>
  <li style={{ transform: "rotate(var(--turn))" }}>First</li>
  <li style={{ transform: "rotate(var(--turn))" }}>Second</li>
</motion.ul>
~~~

The parent owns one value and its descendants consume it. This is useful when a theme or shared style should change together. Use the exact property name, including its leading `--`, in Motion targets. Keep corresponding units compatible, such as degrees to degrees.

## Use a variable as a target

A CSS variable can also supply a normal property target:

~~~jsx
<motion.div animate={{ backgroundColor: "var(--surface-active)" }} />
~~~

Define the variable on the element or an ancestor. If a variable can be missing, provide a CSS fallback:

~~~css
.card {
  background: var(--card-surface, white);
}
~~~

Use CSS for design tokens and Motion for transitions between valid states. Keep one clear source of truth for a theme choice rather than duplicating it across unrelated state and styles.

## Choose between state, Motion values, and variables

| Need | Useful tool |
| --- | --- |
| Discrete choice such as active theme | React state |
| Derived value from continuous input | Motion value and `useTransform` |
| Shared style value across descendants | CSS custom property |
| High-frequency transform or opacity | Motion value attached directly to a Motion style |

Animating a CSS variable always triggers paint. Using that variable inside a transform does not make the animation compositor-only. For one frequently moving element, prefer direct Motion values such as `x`, `y`, `scale`, or `opacity`. Use a custom property when the shared cascade is worth the paint cost.

## Interpolate complex strings

A complex value is a string containing numbers, colors, and units. Examples include `boxShadow`, `filter`, gradients, and multi-value properties.

~~~jsx
<motion.div
  animate={{
    boxShadow: [
      "0 8px 24px rgba(124, 58, 237, 0.16)",
      "0 12px 32px rgba(15, 118, 110, 0.2)",
    ],
  }}
/>
~~~

For reliable interpolation, endpoints should have the same structure: matching numbers in corresponding positions, compatible units, and the same number and kind of colors. A filter from `"blur(0px) saturate(1)"` to `"blur(4px) saturate(1.2)"` keeps its functions and numeric positions. Adding a filter function at only one end can make interpolation fail.

Some values can be padded with extra numbers, but matching the structures explicitly is clearer. If a value should change instantly, treat it as a discrete state change instead of forcing interpolation.

## Diagnose a jump

These structures do not match:

~~~text
"0px 100px 0px 100px" -> "0px"
"blur(0px)" -> "blur(10px) hue-rotate(20deg)"
~~~

Use corresponding structures instead:

~~~text
"0px 100px 0px 100px" -> "0px 0px 0px 0px"
"blur(0px) hue-rotate(0deg)" -> "blur(10px) hue-rotate(20deg)"
~~~

When interpolation looks wrong, check that both endpoints are valid for the same property, numeric values use compatible units, colors have matching structure, and variables resolve to the expected values.

## Keep custom properties maintainable

- Choose semantic names such as `--card-accent`, not names tied to the current color.
- Define a variable near the component that owns it unless it is a global design token.
- Give inherited variables intentional defaults.
- Keep complex target strings short enough to compare.
- Avoid animating a shared variable every frame when a direct Motion value is simpler.
- Check contrast at both theme endpoints.

## Accessibility and reduced motion

Color should not be the only cue for a selected state. Pair it with text, iconography, or semantics such as `aria-pressed`. Keep focus indicators visible across themes.

For a theme change, an immediate color swap may be preferable to a long transition. Avoid long, repeated shadow or filter changes that distract or cause repeated paint.

## Troubleshooting

1. **A variable is ignored:** check its `--` prefix, cascade owner, and `var()` consumer.
2. **A variable target is unresolved:** define it before animation and add a fallback when needed.
3. **A complex value jumps:** compare numbers, colors, layers, order, and units.
4. **A filter looks wrong:** keep the same functions in the same order.
5. **A shadow interpolates unexpectedly:** match the number of layers and color values.
6. **Painting is too frequent:** avoid high-frequency custom properties when a direct Motion value works.
7. **A theme is communicated only by color:** add a visible state cue and check contrast.
8. **Reduced motion is ignored:** make nonessential transitions immediate or remove them.

## Recap and practice

Custom properties share animated values through the CSS cascade. Complex strings interpolate when their structures match. Custom properties trigger paint, so use direct Motion values for frequent transforms and opacity changes.

Animate one accent variable used by a title, border, and button. Then animate a shadow with matching endpoints. Change one endpoint's structure, inspect the result, and restore a compatible value.

Continue with [custom React components with motion.create()](022-custom-react-components-with-motion-create.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="020-svg-and-path-animations.md">020 - SVG and path animations</a></td><td><a href="../qnas/021-css-variables-and-complex-values.md">021 - CSS variables and complex values Q&amp;A</a></td><td><a href="../examples/021-css-variables-and-complex-values.md">021 - CSS variables and complex values example</a></td><td><a href="022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create()</a></td></tr></tbody>
</table>

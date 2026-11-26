# CSS variables and complex values Q&A

## 1. Can Motion animate CSS custom properties?

Yes. Use the property name, including its two leading hyphens, as a Motion target.

## 2. How does CSS read a custom property?

Use `var(--name)` in a declaration. Define the variable on the element or an ancestor in its cascade.

## 3. Can one variable affect several children?

Yes. Descendants can inherit a shared custom property for a theme or coordinated style.

## 4. Can a normal Motion target use a CSS variable?

Yes. A property such as `backgroundColor` can target `"var(--surface-active)"` when that variable is defined.

## 5. When should I use a custom property?

Use it when several descendants share a changing style value or when the CSS cascade makes the relationship clearer.

## 6. When is a Motion value better?

Use a Motion value for continuous or high-frequency visuals, especially direct transforms and opacity.

## 7. Is animating a CSS variable compositor-only?

No. Animating a CSS variable triggers paint, even if CSS uses it inside a transform.

## 8. What is a complex CSS value?

A string containing numbers, colors, and units, such as a box shadow, filter, gradient, or multi-value property.

## 9. What makes two complex values compatible?

They should have matching structures: corresponding numeric positions and units, and compatible color values with the same count.

## 10. Why did a complex value jump instead of interpolate?

The endpoints may have different functions, number counts, units, colors, or shadow layers. Make their structures correspond.

## 11. How do I animate a filter reliably?

Keep the same filter functions in the same order at both endpoints and change corresponding numbers.

## 12. How do I animate a shadow reliably?

Use the same number of layers and corresponding lengths, colors, and alpha values.

## 13. What if a variable is undefined?

The consuming CSS value can be invalid. Define it before animation and use a fallback such as `var(--surface, white)`.

## 14. Should a variable name describe its current color?

No. Prefer semantic names such as `--accent` or `--card-surface` so themes can change without renaming tokens.

## 15. Can React state choose the variable target?

Yes. React state can select a discrete theme while Motion interpolates the corresponding variable targets.

## 16. Can variants contain CSS variables?

Yes. A variant can target custom properties like other animatable values. Keep the endpoints compatible.

## 17. How do I keep color changes accessible?

Do not communicate selection only with color. Add text or a semantic state, preserve focus visibility, and check contrast.

## 18. How should reduced motion affect a theme transition?

Make it immediate or remove nonessential effects when appropriate. Avoid long shadow or filter changes.

## 19. Should I use CSS variables for every style?

No. Use variables when sharing helps. Direct Motion targets are simpler for one element's transform or opacity.

## 20. What should I check if the result is still unexpected?

Check CSS validity, variable resolution, endpoint structure, compatible units, and whether a discrete value is being forced to interpolate.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="020-svg-and-path-animations.md">020 - SVG and path animations Q&amp;A</a></td><td><a href="../lessons/021-css-variables-and-complex-values.md">021 - CSS variables and complex values lesson</a></td><td><a href="../examples/021-css-variables-and-complex-values.md">021 - CSS variables and complex values example</a></td><td><a href="022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() Q&amp;A</a></td></tr></tbody>
</table>

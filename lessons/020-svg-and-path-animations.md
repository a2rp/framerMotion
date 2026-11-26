# SVG and path animations

Scalable Vector Graphics are part of the DOM, so Motion can animate SVG elements with the same React animation model used for HTML. Motion components exist for elements such as `svg`, `path`, `circle`, `line`, `rect`, and text.

SVG is useful for icons, diagrams, illustrations, charts, and path drawing. Start with a clear `viewBox` and geometry, then animate only the attributes or styles needed to communicate the change.

## Create Motion SVG elements

Prefix the SVG tag with `motion.` and use normal SVG attributes:

~~~jsx
import { motion } from "motion/react";

<motion.svg viewBox="0 0 100 100" role="img" aria-label="Animated circle">
  <motion.circle
    cx="50"
    cy="50"
    r="28"
    fill="none"
    stroke="currentColor"
    animate={{ r: 36, opacity: 0.6 }}
  />
</motion.svg>
~~~

Motion can animate style properties such as `fill` and `opacity`, as well as SVG attributes such as `cx`, `cy`, and `r`. When using Motion values, provide them in `style` for style properties or in the appropriate SVG attribute for attributes.

Some SVG attributes overlap with CSS transform names. For SVG attributes named `x`, `y`, or `scale`, use Motion's `attrX`, `attrY`, or `attrScale` when you intend to animate the SVG attribute. The shorter `x`/`y` values on a Motion component are transform shorthands and are rendered as styles.

## Set up the coordinate system

The SVG `viewBox` defines the internal coordinate system: its minimum x/y coordinates, width, and height. The rendered width and height scale that drawing into the page. Keep the geometry inside the viewBox and use a responsive rendered size when the graphic should adapt to the available width.

~~~jsx
<motion.svg
  viewBox="0 0 600 300"
  style={{ width: "100%", height: "auto" }}
>
  ...
</motion.svg>
~~~

A `motion.svg` can animate its `viewBox` to pan or zoom. Because this changes the visible framing of the whole drawing, use it with a clear purpose and avoid rapid or disorienting zooms. If only one SVG element needs to move, animate that element rather than changing the entire coordinate system.

## Draw a line with pathLength

Motion provides normalized SVG path-drawing values:

| Property | Meaning |
| --- | --- |
| `pathLength` | Fraction of the path drawn |
| `pathSpacing` | Space between drawn segments |
| `pathOffset` | Position of the drawn segment along the path |

These values use a 0-to-1 progress scale, where 1 represents the full path. They work with SVG shapes including path, circle, ellipse, line, polygon, polyline, and rect.

A line draw is usually just:

~~~jsx
<motion.path
  d="M 20 80 C 80 10, 140 10, 200 80"
  fill="none"
  stroke="currentColor"
  initial={{ pathLength: 0 }}
  animate={{ pathLength: 1 }}
  transition={{ duration: 1, ease: "easeInOut" }}
/>
~~~

The SVG must have a visible stroke for the drawing to appear. Set `fill="none"` for an outline path when a fill would obscure the interior. Stroke width, line caps, and joins are ordinary SVG presentation attributes.

Use `pathSpacing` and `pathOffset` when a partial segment should travel along a shape, such as a progress arc or loader. Keep values within the intended range and test the result at the actual rendered SVG size.

## Animate path shape and other values

Motion can interpolate an SVG path's `d` attribute when the start and end path descriptions have compatible structure. Paths with corresponding commands and points are more likely to interpolate cleanly. If two shapes have very different geometry, the result may look odd or be incompatible; redesign the paths with matching structure or use a path-morphing tool intentionally.

Other SVG values can be animated declaratively:

~~~jsx
<motion.path
  d="M 30 70 L 70 30 L 110 70"
  animate={{ stroke: "#7c3aed", strokeWidth: 6 }}
/>
~~~

Colors and numeric attributes can transition. Keep path geometry and fill/stroke choices understandable at every point. For an SVG graphic that is meaningful, provide accessible text using a title and description or an appropriate accessible name.

## Transform origins

SVG and HTML have different default transform-origin behavior. Motion makes SVG transforms behave more like HTML by using the SVG element's bounding box as the transform reference. Set `transformBox: "view-box"` when a transform should use the full SVG coordinate system instead. Be explicit when rotation or scaling appears to pivot around an unexpected point.

## Compose with React state and variants

Use React state to choose a target visual state, such as which icon is selected. Use variants to coordinate multiple SVG children. Use Motion values when an SVG style should follow continuous input such as scroll or pointer position.

Avoid recreating the SVG tree or storing every animation frame in React state. Keep static geometry stable and animate only the values that need to change. This keeps the drawing easier to inspect and the interaction easier to maintain.

## Accessibility and reduced motion

A decorative SVG can be hidden from assistive technology with `aria-hidden="true"`. A meaningful graphic should have a suitable accessible name, such as a `title` referenced by `aria-labelledby`, and a concise description when needed. Do not depend on line drawing to reveal the only copy of important information.

Respect reduced-motion preferences for large pan, zoom, repeated movement, or long path reveals. A simple static graphic is often a complete alternative. Keep the final state visible when animation is skipped, and ensure surrounding controls have accessible names and keyboard behavior.

## Troubleshooting

1. **The line does not draw:** verify the path has a visible stroke and that `pathLength` changes from 0 to 1.
2. **The path is filled unexpectedly:** set `fill="none"` for a stroke-only drawing.
3. **The graphic is clipped:** inspect the `viewBox` bounds and the path coordinates.
4. **The whole SVG moves instead of the shape:** distinguish a CSS transform from an SVG attribute such as `attrX`.
5. **Rotation pivots oddly:** check the SVG transform origin and `transformBox`.
6. **Path morphing jumps:** ensure the start/end `d` values have compatible command structure.
7. **A progress dash is backwards:** adjust `pathOffset` and verify the path direction.
8. **The graphic is inaccessible:** name meaningful SVGs and hide purely decorative graphics from assistive technology.
9. **Reduced motion is ignored:** render the final static state or shorten/remove the reveal.

## Recap and practice

Motion supports SVG elements, styles, attributes, normalized path drawing, compatible path morphing, and `viewBox` animation. Make the coordinate system and accessibility clear first; animate only the visual properties that support the interface.

Practice with a route drawing: animate two strokes, then animate a point along the route. Resize the SVG and inspect the viewBox. Finally, disable the line reveal for reduced motion and verify that the route remains understandable.

Continue with [CSS variables and complex values](021-css-variables-and-complex-values.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate</a></td><td><a href="../qnas/020-svg-and-path-animations.md">020 - SVG and path animations Q&amp;A</a></td><td><a href="../examples/020-svg-and-path-animations.md">020 - SVG and path animations example</a></td><td><a href="021-css-variables-and-complex-values.md">021 - CSS variables and complex values</a></td></tr></tbody>
</table>

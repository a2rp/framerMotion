# SVG and path animations Q&A

## 1. Can Motion animate SVG elements?

Yes. Motion provides components such as `motion.svg`, `motion.path`, `motion.circle`, and `motion.rect`.

## 2. What does viewBox do?

It defines the SVG's internal coordinate system and visible region. It is independent from the rendered CSS width and height.

## 3. Can viewBox be animated?

Yes. Animating `viewBox` can pan or zoom the full graphic. Use it deliberately because it reframes the whole drawing.

## 4. How do I draw a path progressively?

Animate `pathLength` from 0 to 1 on a stroked SVG shape. The value is normalized, and 1 represents the full path length.

## 5. Does the path need a stroke?

Yes, a line drawing needs a visible stroke. Set `fill="none"` for an outline path if a fill would hide the interior.

## 6. Which SVG elements support path drawing?

Motion supports path drawing for path, circle, ellipse, line, polygon, polyline, and rect.

## 7. What do pathSpacing and pathOffset do?

`pathSpacing` controls spacing between drawn segments. `pathOffset` controls where a partial segment begins along the path.

## 8. Are pathLength values measured in pixels?

No. Motion uses a normalized 0-to-1 progress scale for pathLength, pathSpacing, and pathOffset.

## 9. Can Motion animate SVG attributes?

Yes. It can animate numeric attributes such as `cx`, `cy`, and `r` as well as styles such as opacity, fill, and stroke.

## 10. What are attrX and attrY for?

They target SVG attributes named x and y when the shorter Motion values would mean CSS transforms. Use them when you specifically intend to animate the SVG coordinate attributes.

## 11. How does Motion handle SVG transform origin?

Motion makes SVG transform origins behave like HTML by using the element's box. Set `transformBox: "view-box"` when the SVG viewBox should be the reference instead.

## 12. Can a path morph from any shape to any other shape?

Not reliably. Interpolation works best when path descriptions have compatible command structure and corresponding points. Reshape paths or use an intentional path-morphing utility for dissimilar geometry.

## 13. Why is my path invisible?

Check that its coordinates fall inside the viewBox, it has a non-transparent stroke, the stroke width is nonzero, and its pathLength animation reaches a visible value.

## 14. Why is the drawing clipped?

The path may extend beyond the viewBox or the SVG may have an unexpected aspect ratio. Inspect the coordinate bounds and rendered dimensions.

## 15. Why does rotation pivot around an unexpected point?

Check the element's bounding box, the SVG viewBox, and transformBox. SVG transform references differ from normal HTML assumptions.

## 16. How should I make a meaningful SVG accessible?

Give it an accessible name with a title/label and provide a concise description when the graphic needs explanation. Do not use path animation as the only way to reveal essential meaning.

## 17. Should decorative SVGs be announced?

Usually not. Hide a purely decorative SVG from assistive technology with `aria-hidden="true"`. A meaningful graphic needs a suitable accessible name.

## 18. What should happen when reduced motion is preferred?

Avoid or shorten large pan, zoom, repeated movement, or line-reveal motion. Keep a static final graphic available.

## 19. Can React state choose which SVG state to animate?

Yes. Use React state for discrete choices such as active tab or selected icon, and let Motion animate between those target values.

## 20. Should every animated SVG frame be stored in React state?

No. Animate the SVG styles or attributes directly with Motion. React state should represent application choices, not high-frequency animation frames.

## 21. When should I animate the whole viewBox versus one element?

Animate viewBox for a deliberate pan or zoom of the entire graphic. Animate one shape when only that shape needs to move, scale, or change color.

## 22. How can I diagnose a backwards progress arc?

Check the path's direction, the initial pathLength, and pathOffset. A different starting point or reversed path direction may be needed.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate Q&amp;A</a></td><td><a href="../lessons/020-svg-and-path-animations.md">020 - SVG and path animations lesson</a></td><td><a href="../examples/020-svg-and-path-animations.md">020 - SVG and path animations example</a></td><td><a href="021-css-variables-and-complex-values.md">021 - CSS variables and complex values Q&amp;A</a></td></tr></tbody>
</table>

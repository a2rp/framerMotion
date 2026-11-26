# Layout animations

A layout animation smoothly follows a change in an element's measured size or position after a React render. Add the `layout` prop to a Motion component when its layout should animate automatically.

## Opt in to layout measurement

React and CSS still determine the final layout. Motion measures the previous and next layout, then animates between them using transforms.

~~~jsx
<motion.article layout style={{ width: expanded ? 360 : 220 }}>
  Card content
</motion.article>
~~~

Change the layout through React state, props, a class, or a style value. The layout prop observes the resulting render. Do not use animate or whileHover to change the same layout dimension and expect layout measurement to replace that animation; use ordinary style/class changes for the layout and let layout handle the transition.

## What layout can animate

Layout animation can follow changes in position and size caused by many React-driven changes:

- Reordering or filtering a list.
- Expanding an accordion or card.
- Changing grid columns or alignment.
- Adding and removing content.
- Switching flex alignment values that cannot be interpolated directly.

It can also be combined with ordinary animation targets. For example, opacity can have its own duration while layout uses a spring.

~~~jsx
<motion.div
  layout
  animate={{ opacity: active ? 1 : 0.7 }}
  transition={{
    duration: 0.2,
    layout: { type: "spring", stiffness: 360, damping: 30 },
  }}
/>
~~~

## Transform-based animation and scale correction

Motion performs layout animation with transforms for performance. When an element changes size, a scale transform may stretch its contents, border radius, or shadow. Add layout to children that should be independently corrected. For text or images changing aspect ratio, `layout="position"` can animate position while letting the new size snap to its final value.

Borders can appear stretched because border width affects layout and cannot be scaled below one device pixel. A nested element or pseudo-element can act as a border if that distortion is visible. Avoid unnecessary nested layout props; each measured element adds work.

Inline elements do not receive transforms as expected. Use an element with a layout box, such as inline-block or block, and confirm that it re-renders when the layout changes.

## Scroll and fixed positioning

For layout animation inside a scrollable Motion container, use `layoutScroll` on the scrolling ancestor so Motion can account for its scroll offset. For children in a fixed-position Motion root, use `layoutRoot` so page scroll is accounted for while measuring the descendants. Later chapters connect layout measurement with shared elements and groups.

## Choose the right layout behavior

| Desired behavior | Approach |
| --- | --- |
| Animate size and position | `layout` |
| Animate position but let size update immediately | `layout="position"` |
| Keep children from appearing stretched | Add `layout` to affected children |
| Animate a transform or opacity target directly | Use `animate` or a gesture target |
| Measure within a scroll container | `layoutScroll` on the scrollable Motion ancestor |
| Measure fixed-position descendants | `layoutRoot` on a fixed Motion root |

Layout animation does not mean every CSS property is directly interpolated. Motion measures the geometry and uses transforms to move between states. Use direct animation for properties such as opacity or color, and layout for the geometry produced by the render.

## Common mistakes and limits

- **No animation:** the component with layout must re-render when its geometry changes.
- **Inline element issue:** use a transformable layout box.
- **Stretching text or images:** add layout to the affected child or choose position-only behavior.
- **Changing dimensions through animate:** update layout through style, className, or React output.
- **Scrollable ancestor mismatch:** add layoutScroll to the relevant Motion scroller.
- **Fixed root drift:** mark the fixed Motion container with layoutRoot.
- **Unnecessary measurement:** opt in only on components whose layout should animate.
- **SVG:** SVG does not use the same CSS layout model; animate SVG attributes directly.
- **Resize surprises:** horizontal window resize may suppress layout animations for performance.

## Recap and practice

The layout prop measures React-driven geometry changes and animates them with transforms. Use position-only mode when scaling content looks wrong, add layout to children that need scale correction, and account for scrolling or fixed roots.

Practice by filtering a grid of cards. Give each item a stable key and layout prop. Then expand one card and compare full size interpolation with position-only animation. Check text wrapping and shadows during the transition.

Continue with [shared layout and layout groups](014-shared-layout-and-layout-groups.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data</a></td><td><a href="../qnas/013-layout-animations.md">013 - Layout animations Q&amp;A</a></td><td><a href="../examples/013-layout-animations.md">013 - Layout animations example</a></td><td><a href="014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups</a></td></tr></tbody>
</table>
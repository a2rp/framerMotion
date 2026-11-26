# Layout animations Q&A

## 1. What does the layout prop do?

It opts a Motion component into automatic animation when its measured size or position changes after a React render.

## 2. Do I provide start and end coordinates?

No. React and CSS produce the old and new layouts. Motion measures them and animates between the geometries.

## 3. What kinds of changes can layout follow?

Position, size, reordering, filtering, content expansion, grid changes, and some alignment changes can produce layout animations.

## 4. How are layout animations performed?

Motion uses transforms to animate measured geometry. This can be efficient, but scale transforms can visually stretch content and certain styles.

## 5. How do I prevent child content from stretching?

Add layout to affected Motion children so they can correct their own scale. For content whose aspect ratio changes, use position-only layout animation when appropriate.

## 6. When should I use layout position?

Use §layout="position"§ when an element should move to its new position but its size should update directly instead of being scale-animated.

## 7. Can layout and opacity animation be combined?

Yes. Use layout for geometry and animate or a gesture target for opacity or other presentation values. A dedicated layout transition can give geometry different timing.

## 8. Should I change width using animate when layout is enabled?

For a React-driven layout transition, update width through style, className, or rendered structure and let layout measure the change. Avoid making layout and direct animation compete for the same dimension.

## 9. Why does an inline element not animate correctly?

Transforms do not work as expected on ordinary inline layout boxes. Use block or inline-block display for the Motion element.

## 10. What is layoutScroll for?

It tells Motion to measure a scrollable Motion element's scroll offset when calculating layout animations within that container.

## 11. What is layoutRoot for?

It marks a fixed-position Motion root so page scroll is accounted for when measuring its children's layout.

## 12. Are SVG layout animations supported the same way as HTML?

No. SVG does not have the same CSS layout system. Animate SVG attributes such as coordinates or path properties directly.

## 13. Why can a border or shadow look distorted?

Layout animation uses scale transforms. Radius and shadow correction is available for supported styles, while borders can still look stretched because they affect layout and cannot scale below a pixel.

## 14. Does every descendant need layout?

No. Add it only to elements that need their own measurement or scale correction. More measured components add work.

## 15. Why might nothing animate after a state change?

Check that the measured Motion component re-renders, that the layout actually changed, and that the element has a transformable box.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="012-presence-modes-and-dynamic-exit-data.md">012 - Presence modes and dynamic exit data Q&amp;A</a></td><td><a href="../lessons/013-layout-animations.md">013 - Layout animations lesson</a></td><td><a href="../examples/013-layout-animations.md">013 - Layout animations example</a></td><td><a href="014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups Q&amp;A</a></td></tr></tbody>
</table>
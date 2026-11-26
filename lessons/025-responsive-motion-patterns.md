# Responsive motion patterns

Responsive motion adapts to available space and input context while keeping the same content and interaction understandable. Let CSS handle responsive layout. Use Motion for transitions between visual states, and use CSS custom properties when an animation endpoint should vary at a breakpoint.

Avoid reading `window.innerWidth` during render to choose initial animation values. That can disagree between server rendering and the browser. CSS media queries and responsive variables let the browser choose the correct value without a React render.

## Separate layout from animation

Use CSS grid, flexbox, container rules, and media queries to define where content belongs. Use Motion for an entrance, state transition, or layout change that should animate.

~~~css
.responsive-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 48rem) {
  .responsive-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}
~~~

If a state change causes elements to reflow, Motion's `layout` prop can animate their measured layout changes:

~~~jsx
<motion.article layout>
  ...
</motion.article>
~~~

Use layout animation when the position or size change is meaningful. CSS should still define the final geometry. Avoid manually guessing x/y offsets for every breakpoint when the layout itself can determine them.

A CSS media query can change layout immediately without React rendering. If you expect Motion to animate a breakpoint-only reflow, verify that the layout change is observed in your actual component and browser. Use a state-driven update or a CSS transition when it is the more reliable fit.

## Use responsive CSS variables for endpoints

Motion can animate from or to a CSS variable. Define that variable at responsive breakpoints, then reference it in an animation target:

~~~css
.responsive-card {
  --entry-distance-y: 12px;
}

@media (min-width: 48rem) {
  .responsive-card {
    --entry-distance-y: 32px;
  }
}
~~~

~~~jsx
<motion.article
  className="responsive-card"
  initial={{ opacity: 0, y: "var(--entry-distance-y)" }}
  animate={{ opacity: 1, y: 0 }}
/>
~~~

The browser selects the responsive custom property, and Motion animates from that value to the target. This avoids hard-coding separate values in React for each screen size.

Test the breakpoint during an active animation. A CSS variable changing does not necessarily restart an animation already in progress; if the target must retarget live, model that change explicitly.

## Adapt animation amount and behavior

A smaller viewport may need less travel, a simpler entrance, or no parallax. A large desktop layout can support a little more visual movement, but the effect should remain proportional and restrained.

| Responsive need | Useful approach |
| --- | --- |
| Different layout columns | CSS media queries or grid |
| Different animation start distance | Responsive CSS custom property |
| Reflow after a React state change | Motion layout animation |
| Different interaction logic at a breakpoint | A deliberate media-query state with an SSR-safe initial strategy |
| Less movement for the system preference | `MotionConfig` and `useReducedMotion` |

Viewport size and reduced-motion preference are separate conditions. Do not assume a small screen means reduced motion, or that a large screen means motion is comfortable.

## Keep CSS and Motion from competing

Motion styles are applied inline or through browser animation mechanisms. Keep static responsive styling in CSS and animation targets in Motion. Avoid applying a generic CSS transition to the same properties Motion controls; classes such as `transition-all` can cause timing conflicts or stuttering.

If using a utility CSS framework, let its classes define static and responsive styles while Motion controls the animation. When a responsive utility defines a custom property, Motion can use that variable as an animation endpoint. Remember that animating the variable itself triggers paint; animating a transform directly is usually better for continuous movement.

## Server rendering and hydration

The server does not know the browser's current viewport width. Avoid producing one React tree on the server and a different one on the first client render based on an immediate width read.

Prefer CSS for responsive layout and CSS variables. If a real behavior difference needs JavaScript media-query state, use an SSR-safe subscription or fallback so hydration begins consistently. Keep the content order and semantics stable across sizes.

## Accessibility and input context

- Keep the same controls and meaningful content available across viewport sizes.
- Preserve visible focus and keyboard behavior.
- Use touch-friendly hit areas and do not rely only on hover.
- Keep responsive motion compatible with reduced-motion preferences.
- Avoid large parallax, zoom, or lateral travel on any screen when it can distract or disorient.
- Test text wrapping and focus order when the layout changes.

## Troubleshooting

1. **The wrong breakpoint value animates:** verify the class is on the target and the variable is defined in the active media query.
2. **Resize does not retarget a running animation:** CSS variable changes do not necessarily restart the current animation; define an explicit update if live retargeting is required.
3. **Motion and CSS fight:** remove generic CSS transitions from properties Motion controls.
4. **A layout jump is not animated:** confirm Motion observes a state-driven layout update; a CSS-only media query may not trigger the same React update.
5. **Hydration differs:** avoid reading viewport size during the initial render.
6. **Small screens feel crowded:** reduce travel, simplify the effect, or remove nonessential movement.
7. **Reduced motion only works on one breakpoint:** keep the preference policy separate from responsive CSS.
8. **Hover-only behavior fails on touch:** provide a click, focus, or direct control path.

## Recap and practice

Let CSS decide responsive layout and use Motion for state transitions. Responsive CSS variables can provide different animation endpoints without reading the viewport during render. Use layout animation for observed layout changes, and keep reduced-motion preference independent from screen width.

Practice with a card grid that changes columns at a breakpoint and uses a responsive variable for its entrance distance. Resize during and after the entrance, remove any generic transition class, and check reduced-motion behavior.

Continue with [bundle optimization with LazyMotion](026-bundle-optimization-with-lazymotion.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion</a></td><td><a href="../qnas/025-responsive-motion-patterns.md">025 - Responsive motion patterns Q&amp;A</a></td><td><a href="../examples/025-responsive-motion-patterns.md">025 - Responsive motion patterns example</a></td><td><a href="026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion</a></td></tr></tbody>
</table>

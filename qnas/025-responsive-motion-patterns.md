# Responsive motion patterns Q&A

## 1. What does responsive motion mean?

It adapts animation amount or behavior to the available layout while preserving the same content and interaction meaning.

## 2. Should Motion decide the responsive layout?

Usually no. Let CSS grid, flexbox, and media queries define the layout; use Motion for animated state changes.

## 3. How can animation values vary by breakpoint?

Define responsive CSS custom properties in media queries and use them as Motion animation endpoints.

## 4. Can Motion animate from var(--entry-distance)?

Yes. Motion supports CSS variables as animation targets. Ensure the variable exists on the element or an ancestor.

## 5. Should I read window.innerWidth during render?

Avoid it for the initial render. The server does not know the viewport and the first client render can disagree. Prefer CSS for layout and responsive values.

## 6. How does layout animate a responsive change?

The `layout` prop animates measured layout changes that Motion observes, commonly after React state changes. CSS-only breakpoint changes may need separate verification in the actual component.

## 7. Will layout always animate a media-query breakpoint change?

Do not assume it. CSS can reflow without a React render. Check the behavior; use a state-driven update or CSS transition if that is more reliable.

## 8. Does a CSS variable change retarget an animation already running?

Not necessarily. Test resizing during animation. If live retargeting is required, model the breakpoint change explicitly.

## 9. Can responsive CSS and Motion target the same property?

They can interact, but avoid competing declarations. Keep static responsive styles in CSS and let Motion own the animated property.

## 10. Why can transition-all cause stuttering?

A generic CSS transition may compete with Motion's animation on the same property. Remove it from Motion-controlled properties and retest.

## 11. Should a smaller screen always have less motion?

Not as a universal rule, but smaller layouts often benefit from shorter travel or simpler effects. Consider actual content and context rather than a rigid assumption.

## 12. Is viewport width the same as reduced-motion preference?

No. Screen size describes available space; reduced motion is a user preference. Handle them independently.

## 13. How should reduced motion work across breakpoints?

Use a global policy or `useReducedMotion` for the preference and responsive CSS for viewport-specific values. Check that both conditions work together.

## 14. How do responsive variables help server rendering?

CSS selects the breakpoint value in the browser without the server choosing a different React tree based on viewport width.

## 15. What if JavaScript behavior truly differs by breakpoint?

Use an SSR-safe media-query subscription or another deliberate state strategy. Avoid changing the first render unpredictably.

## 16. How should hover interactions adapt to touch?

Do not make essential actions hover-only. Provide a click or keyboard focus path and use controls that work on touch screens.

## 17. What should I check after changing columns?

Check text wrapping, card order, focus order, hit areas, and whether the animation still supports the layout.

## 18. When is a CSS transition better than Motion layout?

Use CSS when the change is a simple style transition that does not need Motion's state, orchestration, gestures, or layout features.

## 19. Are CSS variables efficient for every animation?

No. Animating a custom property triggers paint. Use direct transforms for high-frequency movement when possible.

## 20. What is a reliable responsive-motion test?

Check narrow and wide layouts, resize during an animation, inspect reduced motion separately, and verify keyboard and touch interaction.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion Q&amp;A</a></td><td><a href="../lessons/025-responsive-motion-patterns.md">025 - Responsive motion patterns lesson</a></td><td><a href="../examples/025-responsive-motion-patterns.md">025 - Responsive motion patterns example</a></td><td><a href="026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion Q&amp;A</a></td></tr></tbody>
</table>

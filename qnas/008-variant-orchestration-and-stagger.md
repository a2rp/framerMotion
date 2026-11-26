# Variant orchestration and stagger Q&A

## 1. What does orchestration control?

It controls timing between a parent target and variant animations on its descendants. The parent schedules the group while each child supplies its own visual target.

## 2. How do §delayChildren§ and §staggerChildren§ differ?

§delayChildren§ waits before the group starts. §staggerChildren§ offsets each child's start to create a cascade. Values are measured in seconds.

## 3. What does §when: "beforeChildren"§ mean?

The parent target runs before child animations start. Put it in the transition for the variant that needs this order.

## 4. What does §when: "afterChildren"§ mean?

The parent waits for child animations to finish. It is useful when contents should leave before a container collapses.

## 5. Can enter and exit use different orders?

Yes. Put transition settings on each variant. An entering group can stagger forward while the hidden state uses another origin or direction.

## 6. Why use §stagger()§?

It creates child start offsets with options for origin and distribution. Use it as a §delayChildren§ value when first-to-last order is not appropriate.

## 7. How do I stagger from the last item?

Set §delayChildren: stagger(0.06, { from: "last" })§. Confirm the rendered order matches the intended reading order.

## 8. How do I make a center-out reveal?

Set §delayChildren: stagger(0.06, { from: "center" })§. Items further from the center receive later offsets.

## 9. Do child delays add to the stagger?

They can. The parent schedule and each child's own delay both affect when it starts. Prefer one clear owner for timing.

## 10. Does a stagger wait for fetched data?

No. It schedules targets already in the motion tree. Render the right React state after data arrives; do not use transition duration to control a request.

## 11. What if state changes before the sequence completes?

Motion retargets active animations. If the app needs a strict workflow, model that workflow in React state rather than relying on animation timing.

## 12. Does orchestration animate reflow?

No. It controls when targets animate. Use layout animation when elements move because DOM layout changed.

## 13. Should essential content be staggered?

Keep the wait subtle. Do not make important information look unavailable while decorative siblings animate.

## 14. How should this behave for reduced motion?

Shorten or remove the cascade when the user asks for less movement. Keep the content immediately available.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="007-variants-and-reusable-states.md">007 - Variants and reusable states Q&amp;A</a></td><td><a href="../lessons/008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger lesson</a></td><td><a href="../examples/008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger example</a></td><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures Q&amp;A</a></td></tr></tbody>
</table>
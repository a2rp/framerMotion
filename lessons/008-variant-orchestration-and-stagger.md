# Variant orchestration and stagger

Variants name visual states; orchestration decides when parent and child targets run. Use it when a group should enter, leave, or change as a coordinated sequence. It is useful for menus, lists, disclosure content, and grouped cards. This builds on [variants and reusable states](007-variants-and-reusable-states.md).

## Parent and child variants

A parent selects a label. Motion descendants with matching labels can follow it. The parent transition controls the sequence, while each child defines its own target.

~~~jsx
const listVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { delayChildren: 0.15, staggerChildren: 0.07 },
  },
};
const itemVariants = {
  hidden: { opacity: 0, y: 10 },
  visible: { opacity: 1, y: 0 },
};
~~~

The parent does not repeat every child's target. This makes it possible to change group timing without copying delays into each item.

## Parent order with §when§

Add §when§ inside a variant transition to sequence the parent relative to descendants:

- §"beforeChildren"§ runs the parent target before child animations.
- §"afterChildren"§ waits for children before running the parent target.
- Without sequencing, parent and children can overlap.

Define transitions per variant when entering and exiting need different order. A panel can appear before its contents enter, then wait for those contents to leave before it collapses.

## Delay and stagger

§delayChildren§ adds a pause before the group starts. §staggerChildren§ offsets successive child start times. A stagger changes when each child begins, not the duration of its animation.

For an origin other than the first child, use §stagger()§ from §motion/react§:

~~~jsx
import { motion, stagger } from "motion/react";

const parent = {
  visible: {
    transition: {
      delayChildren: stagger(0.06, { from: "center" }),
    },
  },
};
~~~

The helper supports origins such as first, last, center, edges, and an item index. It also supports grid distribution and easing. A short fixed stagger is usually easier to understand.

Estimate the last child's start by adding the group delay to the stagger offset for that child. Add the child's own duration to estimate when the whole sequence finishes. A long list can therefore take much longer than one child transition.

## Reverse and state-specific sequences

A distinct sequence per label can make a group feel intentional. Use another §stagger()§ origin or §staggerDirection§ on the hidden state. Avoid stacking a long parent delay, a large stagger, and long child delays: all contribute to perceived waiting.

| Goal | Setting |
| --- | --- |
| Pause before the group | §delayChildren§ |
| Cascade children | §staggerChildren§ |
| Choose the sequence origin | §delayChildren: stagger(interval, { from: ... })§ |
| Parent before children | §when: "beforeChildren"§ |
| Children before parent | §when: "afterChildren"§ |
| Animate layout reflow | Layout animation, covered later |

Orchestration affects timing, not React state and not layout measurement. React still decides which state is active. Use layout animation separately when elements reflow.

## Common problems and good practice

- Children do not follow if they are not Motion components or their labels do not match.
- A parent waits unexpectedly because §when§ is set on the active variant.
- Long delays make a small group feel frozen. Keep timing in the parent where possible.
- A new React state retargets active animation. A stagger is not a workflow engine.
- Essential text should not wait behind decoration.
- Shorten or remove large cascades for reduced-motion preferences.
- A fade does not animate list reflow; use layout animation for position changes.

## Recap and practice

A parent transition coordinates descendants with §when§, §delayChildren§, §staggerChildren§, or §stagger()§. Keep the sequence brief and test the entire group.

Practice with six items: reveal from the first, then from the center, then reverse the exit order. Estimate when the final child begins. Try a reduced-motion setting and decide what timing to remove.

Continue with [hover, tap, and focus gestures](009-hover-tap-and-focus-gestures.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="007-variants-and-reusable-states.md">007 - Variants and reusable states</a></td><td><a href="../qnas/008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger Q&amp;A</a></td><td><a href="../examples/008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger example</a></td><td><a href="009-hover-tap-and-focus-gestures.md">009 - Hover, tap, and focus gestures</a></td></tr></tbody>
</table>
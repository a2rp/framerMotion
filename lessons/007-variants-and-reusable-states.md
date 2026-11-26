# Variants and reusable states

Variants are named animation targets that describe meaningful visual states such as `hidden`, `visible`, `open`, or `selected`. They let a component express intent with a label while keeping the actual property values in one reusable object. This chapter introduces the basic variant pattern. The next chapter focuses on coordinating parent and child variants with orchestration and staggering.

## Why use variants?

An inline target is clear for one simple element:

~~~jsx
<motion.div animate={{ opacity: 1, y: 0 }} />
~~~

As the same state must affect several elements, repeating objects becomes harder to maintain. Variants give those targets names and let a parent coordinate descendants:

~~~jsx
const panelVariants = {
  closed: { height: 0, opacity: 0 },
  open: { height: "auto", opacity: 1 },
};

<motion.section variants={panelVariants} animate="open" />
~~~

A variant is a plain object whose keys are state labels. Each label maps to an animation target, which can include animatable values and a transition. A motion component selects a label with props such as `initial`, `animate`, `whileHover`, `whileTap`, or `exit`.

## Define and select states

Keep the variant object outside the component when it does not depend on props. This avoids recreating it on each render and makes the state map easy to inspect.

~~~jsx
const cardVariants = {
  rest: { opacity: 0.82, y: 8 },
  active: {
    opacity: 1,
    y: 0,
    transition: { duration: 0.24, ease: "easeOut" },
  },
};

function Card() {
  return (
    <motion.article
      variants={cardVariants}
      initial="rest"
      whileHover="active"
    >
      Reusable animation state
    </motion.article>
  );
}
~~~

`initial` chooses the first rendered state; `animate` chooses the persistent target; a `while...` prop selects a temporary interaction state. When the interaction ends, Motion returns to the applicable underlying state. `initial={false}` skips an enter animation and renders the `animate` target immediately, which is useful when content should not animate on first paint.

## Share a state across descendants

A parent can animate to a variant label and pass that label to descendants that declare the matching variant. This is useful for a disclosure, navigation group, card with an icon, or menu. Each child can still define its own visual target for the shared state.

~~~jsx
const containerVariants = {
  closed: { opacity: 0.9 },
  open: { opacity: 1 },
};

const textVariants = {
  closed: { opacity: 0, y: 6 },
  open: { opacity: 1, y: 0 },
};

function Content({ open }) {
  return (
    <motion.section variants={containerVariants} animate={open ? "open" : "closed"}>
      <motion.p variants={textVariants}>The child follows the parent's label.</motion.p>
    </motion.section>
  );
}
~~~

Labels propagate through motion descendants when those descendants participate in the same variant system. A child with an independent `animate` target can own its state instead. Test the hierarchy when combining parent labels with child-level animation props; unclear ownership is a common source of surprising results.

## Variants can contain transitions

A state may include a `transition` beside its target values. Use per-state transitions when entering and leaving a state need different timing. Keep transitions close to the state they describe; use a shared `transition` prop when the same timing applies to every target.

~~~jsx
const statusVariants = {
  idle: { scale: 1, transition: { duration: 0.2 } },
  success: {
    scale: [1, 1.08, 1],
    transition: { duration: 0.35, ease: "easeOut" },
  },
};
~~~

A variant target may be a function when it needs data supplied through `custom`. Keep these functions deterministic and return ordinary animation targets. Dynamic variants are particularly useful for values that must be selected as a component exits; later presence chapters explain that case.

## Design states around meaning

Prefer labels that describe interface intent, not implementation details. `expanded` communicates more than `heightZero`; `unavailable` communicates more than `opacityPointTwo`. A good state map is small, mutually understandable, and reused where behavior is truly the same.

| Use | Example label | Typical target |
| --- | --- | --- |
| Entry | `hidden`, `visible` | opacity and a small offset |
| Disclosure | `closed`, `open` | height, opacity, child state |
| Interaction | `rest`, `hover`, `pressed` | subtle scale, color, or shadow |
| Status | `idle`, `success`, `error` | color and short feedback |

Do not make every property a named state. If one element has a one-off target, an inline object may be simpler. Variants organize state; they do not replace React state. React decides which state is active, and Motion interpolates between the selected targets.

## Common mistakes

- **Label mismatch:** `animate="visible"` has no effect on a child whose variants use `show`. Match names exactly.
- **Expecting React state from Motion:** variants animate presentation; they do not update application data or determine whether content is semantically open.
- **Unintentional first-paint movement:** use `initial={false}` when the first render should already match the target.
- **Duplicated definitions:** share a variant object only when elements should share the same meaning and target.
- **Conflicting animation ownership:** decide whether the parent or child selects a state, then use labels consistently.
- **Animating an inaccessible hidden panel:** keep `aria-expanded`, focus behavior, and DOM visibility aligned with the disclosure state. For interactive hidden content, conditionally render it with presence handling or otherwise prevent keyboard focus from entering invisible controls.

## Recap and practice

- Variants are named animation targets selected by labels.
- `initial`, `animate`, and gesture props select different kinds of states.
- Matching labels can coordinate motion descendants.
- React state remains the source of truth for application behavior.
- Use semantic labels and keep state maps small.

Practice by creating a card with `rest` and `selected` variants. Let a real button toggle React state, update `aria-pressed`, and select the corresponding animation label. Confirm that keyboard activation changes both the semantic state and the visual target.

Continue with [variant orchestration and stagger](008-variant-orchestration-and-stagger.md).

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="006-keyframes-and-repeats.md">006 - Keyframes and repeats</a></td><td><a href="../qnas/007-variants-and-reusable-states.md">007 - Variants and reusable states Q&amp;A</a></td><td><a href="../examples/007-variants-and-reusable-states.md">007 - Variants and reusable states example</a></td><td><a href="008-variant-orchestration-and-stagger.md">008 - Variant orchestration and stagger</a></td></tr>
  </tbody>
</table>
# Shared layout and layout groups

A layout animation follows the geometry of one Motion component across React renders. A shared layout animation connects different components that represent the same visual object. The `layoutId` prop gives matching elements a shared identity so Motion can animate between their old and new locations.

## Shared identity with layoutId

When a component with a particular layoutId leaves and another component with the same layoutId enters, Motion can animate between their measured layouts. This is useful for a selected-tab indicator, a card expanding into a detail view, or a shared highlight that moves between choices.

~~~jsx
{selected && <motion.span layoutId="active-indicator" />}
~~~

The old and new elements should represent the same visual thing. Use a stable ID that is unique in the relevant page scope. When both matching elements are present during a transition, Motion can crossfade them. For a modal or detail view that replaces a source card, combine layoutId with AnimatePresence so the source can remain through its exit.

The transition on the destination element controls a shared layout animation. This lets the compact and expanded representations use different transition settings where appropriate.

## Coordinate components with LayoutGroup

`LayoutGroup` is useful when multiple components affect each other's positions but do not all re-render together. Grouping them synchronizes layout measurements when a member changes, so related siblings can animate their new positions.

~~~jsx
import { LayoutGroup, motion } from "motion/react";

<LayoutGroup>
  <motion.section layout />
  <motion.section layout />
</LayoutGroup>
~~~

Use LayoutGroup for coordination. It does not automatically make every member a shared element; elements that should animate their own size or position still need `layout`, and elements that should morph between locations need a matching `layoutId`.

## Namespace shared IDs

A layoutId is global across the application unless namespaced. If a reusable tab row appears in more than one place, the same indicator ID could incorrectly connect those rows. Give each LayoutGroup an `id` to namespace the layout IDs inside it:

~~~jsx
<LayoutGroup id="settings-tabs">
  <Tabs />
</LayoutGroup>
~~~

Choose an ID that is stable and unique for each rendered group. Do not generate a new random ID on every render, because that changes the identity Motion uses to match elements.

## Shared layout versus ordinary layout

| Intent | Prop |
| --- | --- |
| One component changes its measured geometry | `layout` |
| Different components represent one shared visual element | Matching `layoutId` |
| Related siblings need synchronized measurement | `LayoutGroup` |
| Separate reusable groups need isolated IDs | `LayoutGroup id` |

The concepts can be combined. A card may have layout for its own size change and layoutId for a transition into a detail panel. Keep each prop tied to a clear visual relationship.

## Common mistakes and limits

- **No connection:** source and destination need exactly matching layoutId values.
- **Wrong components connect:** IDs collide because they are global. Namespace reusable groups with LayoutGroup id.
- **No geometry animation:** shared elements must render in states that Motion can measure; check conditional rendering and keys.
- **LayoutGroup used as a replacement for layout:** the group synchronizes measurement, but members still opt into layout behavior.
- **Unrelated IDs reused:** assign IDs based on visual identity, not a generic word reused across unrelated page regions.
- **Unexpected crossfade:** two matching elements may coexist during transition. Design the content and opacity accordingly.
- **Trying to share between pages without lifecycle support:** keep the source or outgoing component present as needed, often with AnimatePresence.
- **Duplicate accessible content:** visual shared elements do not automatically merge semantics. Ensure only the appropriate representation is exposed to assistive technology.

## Recap and practice

Use matching layoutId values to connect different representations of one visual object. Use LayoutGroup to coordinate related layout measurements, and namespace repeated components with its id prop.

Practice with three filter buttons and a moving underline. Then render two independent tab rows and give each its own LayoutGroup id. Confirm that selecting a tab moves only the underline in its own row.

Continue with [motion values and direct updates](015-motion-values-and-direct-updates.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="013-layout-animations.md">013 - Layout animations</a></td><td><a href="../qnas/014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups Q&amp;A</a></td><td><a href="../examples/014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups example</a></td><td><a href="015-motion-values-and-direct-updates.md">015 - Motion values and direct updates</a></td></tr></tbody>
</table>
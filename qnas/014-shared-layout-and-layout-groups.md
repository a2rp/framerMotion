# Shared layout and layout groups Q&A

## 1. What does layoutId do?

It identifies different Motion components as representations of the same visual element. When one leaves and another with the same ID enters, Motion can animate between their measured layouts.

## 2. How is layoutId different from layout?

Layout animates the geometry of one component across its own React renders. LayoutId links different components that represent one shared visual element.

## 3. Do the source and destination need the same layoutId string?

Yes. The values must match exactly for Motion to connect their transitions.

## 4. What is LayoutGroup for?

It coordinates layout measurement among related Motion components that can affect one another's positions but may not re-render together.

## 5. Does LayoutGroup make every child animate?

No. Members still need layout for their own geometry animation or a matching layoutId for a shared element transition.

## 6. Are layoutId values global?

They are global across the application by default. Reusable components can accidentally connect unrelated elements if they reuse the same ID.

## 7. How can reusable groups isolate their IDs?

Wrap them in LayoutGroup with a stable id. Motion namespaces layoutId values inside that group.

## 8. Should the LayoutGroup id be random?

No. It should be stable for the rendered group. A random ID on each render changes the matching namespace.

## 9. What happens if both shared elements are present at once?

Motion can crossfade them while animating their shared geometry. Make sure their overlapping content and semantics still make sense.

## 10. Can a modal expand from a card?

Yes. Give the card and modal representation the same layoutId. Use AnimatePresence when the source or destination needs to remain mounted through its exit.

## 11. Which transition controls a shared layout animation?

The destination element's transition is used for a shared layout animation. Set transition on the element that is entering into the shared identity.

## 12. Can I combine layout and layoutId?

Yes. They express different relationships. A component can animate its own geometry with layout and connect to another representation with layoutId.

## 13. Why do unrelated tab rows animate into each other?

Their indicator elements likely reuse the same global layoutId. Namespace each row with a distinct LayoutGroup id.

## 14. Does a shared visual element combine accessibility semantics?

No. Motion animates presentation. Ensure the appropriate element has the accessible name and state, and avoid exposing duplicate interactive representations.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="013-layout-animations.md">013 - Layout animations Q&amp;A</a></td><td><a href="../lessons/014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups lesson</a></td><td><a href="../examples/014-shared-layout-and-layout-groups.md">014 - Shared layout and layout groups example</a></td><td><a href="015-motion-values-and-direct-updates.md">015 - Motion values and direct updates Q&amp;A</a></td></tr></tbody>
</table>
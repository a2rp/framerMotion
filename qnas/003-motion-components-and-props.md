# Motion components and props Q&A

## 1. What does `motion.div` render?

It renders a normal HTML `div` through a React component that accepts Motion props. Use `motion.button` for a button, `motion.section` for a section, and supported SVG tags such as `motion.path` for SVG content.

## 2. Do Motion components replace HTML semantics?

No. They add animation capabilities to an element. A `motion.div` is still a `div`, not a button. Use semantic HTML and ARIA state where appropriate.

## 3. Which props are Motion-specific?

Common Motion props include `initial`, `animate`, `exit`, `transition`, `whileHover`, `whileTap`, `whileFocus`, `whileInView`, and `layout`. Standard React props such as `className`, `id`, `aria-label`, and `onClick` remain available too.

## 4. Why does `animate` not work on a regular `div`?

A regular React `div` does not implement Motion's animation props. Use the corresponding Motion component, for example `motion.div`, and pass the target through `animate`.

## 5. Are Motion props added as attributes to the DOM?

Motion handles its animation props rather than forwarding them as ordinary HTML attributes. Pass actual content, IDs, labels, classes, and ARIA attributes through their standard React props.

## 6. Can I put a `className` on a Motion component?

Yes. CSS classes are useful for layout, static appearance, and focus styles. Motion can animate values on the same element. Avoid letting CSS and Motion both control the same animated property unless their interaction is intentional.

## 7. How should I combine CSS with a Motion transform?

Let Motion own the transform values that change during animation, such as `x`, `y`, and `scale`. Use CSS for unrelated properties. A CSS `transform` declaration can conflict with Motion's generated transform.

## 8. Can I animate several properties together?

Yes. A target object can contain multiple supported property values, for example `animate={{ opacity: 1, x: 20 }}`. Each property can use a shared transition or a property-specific transition.

## 9. Can I animate SVG?

Motion provides components for supported SVG elements and attributes. Use an SVG motion component such as `motion.circle` or `motion.path` with the attributes valid for that element. SVG path drawing receives a separate lesson.

## 10. Can I use Motion components inside a list?

Yes. Render them with a stable, unique React key for each item. Stable identity helps React and Motion handle updates, exits, and layout changes correctly.

## 11. Do I have to use a Motion component for every element?

No. Keep ordinary HTML elements where no Motion-specific behavior is needed. Use Motion components where you need animation, gestures, viewport tracking, or layout behavior.

## 12. Can a custom React component accept Motion props?

Yes, by wrapping it with `motion.create()`. The custom component must forward the ref and style to the actual HTML or SVG element. See [the custom components lesson](../lessons/022-custom-react-components-with-motion-create.md).

## 13. Can one property use a different transition from another?

Yes. Motion supports property-specific transition configuration. For example, a position value can use a spring while opacity uses a short tween. Check the syntax for the installed Motion version.

## 14. Where do I learn the difference between `initial` and `animate`?

Continue with [initial and animate states](../lessons/004-initial-and-animate-states.md). That lesson explains first render behavior, state changes, and how to handle an element whose initial state should not animate.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="002-installation-and-project-setup.md">002 - Installation and project setup Q&amp;A</a></td><td><a href="../lessons/003-motion-components-and-props.md">003 - Motion components and props lesson</a></td><td><a href="../examples/003-motion-components-and-props.md">003 - Motion components and props example</a></td><td><a href="004-initial-and-animate-states.md">004 - Initial and animate states Q&amp;A</a></td></tr>
  </tbody>
</table>

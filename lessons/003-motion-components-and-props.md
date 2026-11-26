# Motion components and props

Motion components are React components that render familiar HTML or SVG elements while accepting animation-specific props. For example, `motion.div` renders a `div`, and `motion.button` renders a `button`. The element still participates in normal React rendering and keeps its native semantics.

Import the component factory from the current React entry point:

~~~jsx
import { motion } from "motion/react";
~~~

## Keep the underlying element meaningful

Choose the element based on the content and interaction, then add Motion where animation is useful:

~~~jsx
<motion.article initial={{ opacity: 0 }} animate={{ opacity: 1 }}>
  <h2>Latest update</h2>
  <p>The article fades into view.</p>
</motion.article>

<motion.button
  type="button"
  whileHover={{ scale: 1.03 }}
  whileTap={{ scale: 0.98 }}
>
  Save changes
</motion.button>
~~~

The first element remains an article and the second remains a button. Motion does not make a generic element accessible by itself. A clickable `motion.div` still lacks button keyboard behavior and semantics. Use native controls for actions and links.

Motion provides components for supported HTML and SVG tags. Start with a built-in tag such as `motion.section` or `motion.path`. Wrapping a custom React component requires a ref and style forwarding; see [custom React components](022-custom-react-components-with-motion-create.md).

## Common props

Motion props describe different parts of an animation:

- `initial` sets the starting target.
- `animate` sets the target to reach.
- `exit` sets a target played during removal when a parent uses `AnimatePresence`.
- `transition` configures timing and interpolation.
- `whileHover`, `whileTap`, and `whileFocus` define temporary interaction targets.
- `whileInView` defines a target active while the element is in view.
- `layout` enables animation after measured layout changes.
- `style` and standard element props provide styles and attributes.

Each target can animate multiple supported values. Motion can animate ordinary CSS properties, independent transforms such as `x` and `scale`, SVG attributes, and supported complex values. The installed version and the pair of start/end values determine what can be interpolated.

## Standard props still work

Standard React props such as `className`, `id`, `aria-label`, `onClick`, and `style` remain available:

~~~jsx
<motion.button
  className="action"
  aria-label="Add item"
  onClick={addItem}
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
>
  Add
</motion.button>
~~~

Motion-specific props are consumed by Motion instead of being written as invalid attributes on the rendered HTML node. Keep values such as labels, IDs, and ARIA attributes in their normal React props.

## Style and transform ownership

Motion can write animated values into the element's style. A motion component can accept a Motion value through `style`, or animate a target using `animate`. For ordinary styles, CSS classes remain useful for layout and static appearance:

~~~jsx
<motion.div
  className="card"
  animate={{ opacity: active ? 1 : 0.7, x: active ? 0 : 8 }}
/>
~~~

Be deliberate when both CSS and Motion control the same property. A CSS transform rule and Motion's transform output may compete. Prefer Motion's independent transform props for the animated transform and keep unrelated static styles in CSS.

## Motion component versus ordinary element

Use a Motion component only where Motion behavior is needed. An ordinary element can remain a normal React element:

~~~jsx
<main>
  <h1>Account</h1>
  <motion.section animate={{ opacity: 1 }}>
    <h2>Activity</h2>
  </motion.section>
</main>
~~~

This keeps the boundary clear. You may also use a motion component without an animation prop when you need a gesture, viewport, or layout feature, but do not convert every element without a reason.

## Targets and transitions

The `animate` prop accepts a target object, a variant label, or a supported animation-control value. A target object maps property names to destination values. Transitions can be applied to the whole target or to individual properties:

~~~jsx
<motion.div
  animate={{ x: 80, opacity: 1 }}
  transition={{
    default: { type: "spring", stiffness: 220, damping: 24 },
    opacity: { duration: 0.2 },
  }}
/>
~~~

The `x` property uses an independent transform, while opacity uses a tween here. The [transition lesson](005-transitions-and-springs.md) explains transition options and property-specific settings.

## SVG elements

Motion has SVG element components, for example `motion.circle` and `motion.path`. Use SVG-specific attributes such as `cx`, `cy`, and `strokeWidth` with the correct element. Do not apply HTML-only assumptions to SVG. Path drawing and SVG-specific behavior are covered in [the SVG lesson](020-svg-and-path-animations.md).

## Common mistakes

- **Using the wrong semantic element.** Animation does not replace element semantics.
- **Passing animation props to a normal element.** Props such as `animate` need a Motion component.
- **Using an unsupported CSS value pair.** Check compatible start and end values and the installed version.
- **Letting CSS and Motion write the same transform.** Decide which system owns the animated transform.
- **Forgetting a list key.** React identity affects mounting, exit, and layout behavior.
- **Wrapping a custom component without ref forwarding.** Motion needs the actual HTML or SVG node.
- **Animating every node.** Keep static semantic structure as normal HTML where no Motion behavior is needed.

## Recap and practice

- A Motion component renders an HTML or SVG element and accepts Motion props.
- Standard React props still apply.
- Use semantic tags and correct ARIA state regardless of animation.
- Keep static styles in CSS and assign animated properties consistently.
- Use target objects for simple states, then add transitions and gesture props as needed.

Practice by making one heading, one section, and one button motion components. Give each a different Motion prop, then identify which native HTML element is rendered.

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="002-installation-and-project-setup.md">002 - Installation and project setup</a></td><td><a href="../qnas/003-motion-components-and-props.md">003 - Motion components and props Q&amp;A</a></td><td><a href="../examples/003-motion-components-and-props.md">003 - Motion components and props example</a></td><td><a href="004-initial-and-animate-states.md">004 - Initial and animate states</a></td></tr>
  </tbody>
</table>

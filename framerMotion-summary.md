# Framer Motion Summary

This is a topic-wide review of Motion for React, the current continuation of Framer Motion. The course uses the `motion` package and imports React APIs from `motion/react`. Use the [keywords index](framerMotion-keywords.md) to look up API names and the [README roadmap](README.md) to open each lesson, Q&A, and example.

## 1. The animation model

React decides what the interface should render from props and state. Motion animates supported visual values between targets. Keep application state in React when it affects content, behavior, or accessibility. Use Motion values for frequently changing visual values such as pointer position or scroll progress.

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

export function Notice() {
  const [visible, setVisible] = useState(true);

  return (
    <section>
      <button type="button" onClick={() => setVisible((value) => !value)}>
        Toggle notice
      </button>
      <motion.p
        animate={{ opacity: visible ? 1 : 0, y: visible ? 0 : 8 }}
        aria-hidden={!visible}
      >
        The state selects the target and Motion interpolates the visual change.
      </motion.p>
    </section>
  );
}
~~~

This example changes opacity and position. Hiding content visually does not remove it from keyboard navigation or the accessibility tree. Use conditional rendering with `AnimatePresence` when content should leave the DOM after its exit animation. Keep the related lesson's behavior in mind: [initial and animate states](lessons/004-initial-and-animate-states.md) and [exit animations](lessons/011-animatepresence-and-exit-animations.md).

## 2. Installation and imports

For new React projects, the course uses:

~~~bash
npm install motion
~~~

~~~jsx
import { motion } from "motion/react";
~~~

Motion for React currently requires React 18.2 or newer. Framework entry points and feature bundles can differ by project. In a React Server Components app, use a client component for hooks and event-driven behavior, or the supported `motion/react-client` entry point for server-compatible motion components. See [project setup](lessons/002-installation-and-project-setup.md) and [Next.js and Server Components](lessons/027-nextjs-server-rendering-and-react-server-components.md).

Existing applications may still use the `framer-motion` package. Do not rename dependencies blindly: verify the installed version, update imports and lockfiles together, and review behavior changes before merging. The [migration chapter](lessons/029-migrating-from-framer-motion-to-motion.md) covers the transition.

## 3. Declarative animation targets

A motion component supports standard element props plus Motion props:

- `initial` defines the first visual target.
- `animate` defines the current target.
- `exit` defines the target played by an `AnimatePresence` boundary.
- `transition` configures timing and interpolation.
- Gesture and viewport props define temporary targets such as `whileHover` or `whileInView`.
- `layout` asks Motion to animate measured layout changes.

~~~jsx
<motion.button
  initial={{ opacity: 0, y: 12 }}
  animate={{ opacity: 1, y: 0 }}
  whileHover={{ scale: 1.03 }}
  whileTap={{ scale: 0.98 }}
  transition={{ duration: 0.25, ease: "easeOut" }}
>
  Save
</motion.button>
~~~

Use semantic elements for their behavior. A motion button remains a button; a clickable `div` does not become a button just because it animates. Review [motion components](lessons/003-motion-components-and-props.md), [initial and animate](lessons/004-initial-and-animate-states.md), and [gestures](lessons/009-hover-tap-and-focus-gestures.md).

## 4. Transitions and keyframes

A tween is useful when a specific duration and easing curve should control the change. A spring is useful when the interaction should feel responsive and settle naturally. Parameters such as stiffness, damping, and mass change a spring's feel. Prefer short, purposeful transitions for small controls and test the result at real interface size.

Keyframes describe a sequence of values for one property. Repeats are useful for deliberate loops, but infinite movement can distract, affect performance, and conflict with reduced-motion preferences. See [transitions and springs](lessons/005-transitions-and-springs.md) and [keyframes and repeats](lessons/006-keyframes-and-repeats.md).

## 5. Variants and coordinated motion

Variants give related components named states such as `closed` and `open`. A parent can pass a variant label to descendants, which makes it possible to coordinate a menu, dialog, or list without putting every target into one large component.

~~~jsx
const listVariants = {
  closed: { transition: { staggerChildren: 0.04, staggerDirection: -1 } },
  open: { transition: { delayChildren: 0.08, staggerChildren: 0.07 } },
};

const itemVariants = {
  closed: { opacity: 0, y: 8 },
  open: { opacity: 1, y: 0 },
};

function MenuItems({ items, open }) {
  return (
    <motion.ul initial="closed" animate={open ? "open" : "closed"} variants={listVariants}>
      {items.map((item) => (
        <motion.li key={item.id} variants={itemVariants}>
          {item.label}
        </motion.li>
      ))}
    </motion.ul>
  );
}
~~~

Give list items stable keys. Keep state names and visual targets understandable. Use dynamic variants with `custom` when a target depends on data such as slide direction. See [variants](lessons/007-variants-and-reusable-states.md) and [orchestration](lessons/008-variant-orchestration-and-stagger.md).

## 6. Gestures and dragging

Motion supports hover, tap, focus, pan, and drag interactions. Use hover as an enhancement, not the only way to reveal an action. Preserve visible keyboard focus. Dragging needs useful constraints and a non-drag alternative when it performs an important action. Choose the drag axis and momentum deliberately, and ensure touch scrolling still works for the rest of the page.

See [hover, tap, and focus](lessons/009-hover-tap-and-focus-gestures.md) and [pan and drag](lessons/010-pan-drag-and-constraints.md).

## 7. Enter, exit, and layout changes

`AnimatePresence` observes its direct React children. Give changing children stable, unique keys so Motion can distinguish the old child from the new one. The boundary can coordinate exit and entry with modes such as `"sync"`, `"wait"`, and `"popLayout"`; choose only modes supported by the installed release and observe their constraints.

For layout changes, add `layout` to elements whose measured position or size should animate after React changes the DOM layout. A layout transition uses transforms to represent the measured difference. Use `layout="position"` when animating size would visibly stretch content. Shared `layoutId` values can link a visual across separate elements. See [presence](lessons/011-animatepresence-and-exit-animations.md), [presence modes](lessons/012-presence-modes-and-dynamic-exit-data.md), [layout](lessons/013-layout-animations.md), and [shared layout](lessons/014-shared-layout-and-layout-groups.md).

## 8. Motion values and derived values

A Motion value stores a changing visual value without requiring a React render for every frame. Attach it through the `style` prop or derive new Motion values from it.

~~~jsx
import { useRef } from "react";
import { motion, useScroll, useSpring, useTransform } from "motion/react";

export function ReadingProgress() {
  const target = useRef(null);
  const { scrollYProgress } = useScroll({ target });
  const smoothProgress = useSpring(scrollYProgress, { stiffness: 120, damping: 24 });
  const scaleX = useTransform(smoothProgress, [0, 1], [0, 1]);

  return (
    <article ref={target}>
      <motion.div
        aria-hidden="true"
        style={{ scaleX, transformOrigin: "0% 50%" }}
      />
      <p>Article content goes here.</p>
    </article>
  );
}
~~~

Use `useTransform` to map a value to another range, `useSpring` to smooth a changing value, and `useVelocity` or `useMotionValueEvent` when logic needs to observe a value. See [Motion values](lessons/015-motion-values-and-direct-updates.md) and [derived values](lessons/016-transforming-and-composing-motion-values.md).

## 9. Scroll-linked and in-view animation

Use `useScroll` when a visual should track scroll progress continuously, such as a reading indicator. Its target, container, and offsets define the measured range. Use `whileInView` or `useInView` when an animation or state change should happen after an element enters a viewport. A trigger is event-like; scroll progress is continuous.

Avoid hiding essential information until a user scrolls. Test long pages, nested scroll containers, and reduced-motion behavior. See [scroll-linked animation](lessons/017-scroll-linked-animations-with-usescroll.md) and [in-view animation](lessons/018-in-view-animations-and-useinview.md).

## 10. Imperative animation, SVG, and custom properties

Use declarative props for state-driven targets. Use `useAnimate` when an event needs a sequence, scoped selectors, or playback control. It returns a scope ref and an animation function; attach the ref to a container and keep selectors inside that scope. Its animations are cleaned up when the component unmounts.

Motion components can animate SVG attributes and path drawing values such as `pathLength`. CSS custom properties are useful for shared design tokens, while supported complex CSS values can animate as complete values. Confirm that the starting and ending values are compatible. See [useAnimate](lessons/019-imperative-sequences-with-useanimate.md), [SVG](lessons/020-svg-and-path-animations.md), and [CSS values](lessons/021-css-variables-and-complex-values.md).

## 11. Custom components and configuration

Use `motion.create(Component)` to add Motion props to a custom React component. Define the motion component outside render so React does not receive a new component type on every render. The wrapped component must pass a ref and the style prop to the HTML or SVG node being animated. In React 18, this commonly means `forwardRef`; React 19 supports ref as a prop.

`MotionConfig` can provide a shared default transition or a reduced-motion policy. Keep global defaults modest and override them locally where an interaction needs a different feel. See [custom components](lessons/022-custom-react-components-with-motion-create.md) and [global configuration](lessons/023-global-configuration-with-motionconfig.md).

## 12. Accessibility, responsive behavior, and bundle size

Respect `prefers-reduced-motion`. A `MotionConfig` policy of `reducedMotion="user"` disables transform and layout animations for users who request reduced motion while leaving properties such as opacity available. Use `useReducedMotion` when the interface needs an alternate design, for example removing parallax or replacing large movement with a simple fade.

Make content visible and usable without waiting for an animation. Use native controls and keyboard access. Avoid rapid flashes, endless decorative movement, and large unexpected motion. Keep interactions understandable without hover.

Use `LazyMotion` and `m` when bundle analysis shows that reducing loaded features matters. A smaller feature bundle can omit capabilities, so confirm all props used by the application are included. Measure the production bundle before and after the change. See [accessibility](lessons/024-accessible-and-reduced-motion.md), [responsive patterns](lessons/025-responsive-motion-patterns.md), and [bundle optimization](lessons/026-bundle-optimization-with-lazymotion.md).

## 13. React architecture, routing, and verification

In a Server Components framework, keep Motion hooks and event handlers in client components. Use the server-compatible entry point only for the patterns it supports. For route transitions, preserve React keys and put the presence boundary where it can observe route element changes. Coordinate focus, scroll restoration, and accessible page announcements separately from visual transitions.

Motion updates visual values outside React's render loop, but application state changes still render React components. Prefer transforms and opacity for frequent animation when they meet the design need. Profile before adding optimizations.

When testing, assert semantic state and visible outcomes rather than exact frame timings. Mock or disable animation only when the test is about application behavior, and keep separate checks for reduced-motion preferences, focus, and keyboard use. See [Next.js](lessons/027-nextjs-server-rendering-and-react-server-components.md), [routing](lessons/028-react-router-transitions-and-navigation.md), [performance](lessons/030-performance-and-debugging.md), and [testing](lessons/031-testing-motion-components.md).

## 14. Quick decision guide

| Need | Start with |
| --- | --- |
| Animate between React states | `initial`, `animate`, and `transition` |
| Reuse coordinated states across children | `variants` |
| Respond to pointer or keyboard interaction | `whileHover`, `whileTap`, or `whileFocus` |
| Animate a removed item | `AnimatePresence` with an `exit` target and stable key |
| Animate after layout reflow | `layout` |
| Link a visual across locations | Matching `layoutId` values |
| Tie a visual directly to scroll position | `useScroll` and a derived Motion value |
| Trigger work once an element appears | `whileInView` or `useInView` |
| Sequence scoped DOM animations imperatively | `useAnimate` |
| Respect system motion preferences | `MotionConfig reducedMotion="user"` and `useReducedMotion` |

The [README roadmap](README.md) links to the complete lesson, Q&A, and example for every topic.

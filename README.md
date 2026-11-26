# Framer Motion Learning Notes

A structured, hands-on course for building accessible interface animations with Motion for React, the current continuation of Framer Motion. The course moves from React animation fundamentals through gestures, layout, scroll, performance, and a complete interface.

The repository is named `framerMotion`, while current examples use the `motion` package and imports from `motion/react`. Older projects may still use `framer-motion`; see the migration chapter before changing an existing application.

## Learning outcomes

By following the chapters in order, you will learn to:

- Install Motion in a React project and choose the correct entry point.
- Animate HTML and SVG elements with declarative props, variants, and transitions.
- Coordinate enter, exit, layout, and shared element transitions.
- Build hover, tap, focus, pan, and drag interactions.
- Compose motion values for scroll effects and other continuous values.
- Use imperative sequences when declarative component props are not a good fit.
- Respect reduced-motion preferences and keep controls accessible.
- Apply Motion with React Server Components, routing, and bundle-size constraints.
- Debug and test motion behavior, then combine the ideas in a small interface.

## Prerequisites and setup

These notes assume familiarity with JavaScript, JSX, React components, props, state, lists and keys, refs, and basic CSS. The current Motion for React installation guide requires React 18.2 or newer. Check the version required by the Motion release installed in your project.

Install Motion in an existing React application:

~~~bash
npm install motion
~~~

Create a motion component and animate it when state changes:

~~~jsx
import { useState } from "react";
import { motion } from "motion/react";

export function ToggleCard() {
  const [open, setOpen] = useState(false);

  return (
    <section>
      <button type="button" onClick={() => setOpen((value) => !value)}>
        Toggle details
      </button>
      <motion.div
        animate={{ opacity: open ? 1 : 0.5, y: open ? 0 : 8 }}
        transition={{ duration: 0.25, ease: "easeOut" }}
      >
        The React state chooses the target; Motion interpolates the visual change.
      </motion.div>
    </section>
  );
}
~~~

The examples use the current `motion/react` entry point unless a chapter explains a specialized entry point. They are learning snippets, not a bundled application. Put each runnable example into a React project and include the imports and surrounding component shown in that chapter.

The MIT license in this repository applies to these notes. It does not replace the licenses or terms for Motion, React, or other dependencies.

## How to use these notes

1. Read the lesson in order and note which values come from React state and which are animation values.
2. Work through the matching example in a React project.
3. Use the matching Q&A to compare options and diagnose common mistakes.
4. Change one target, transition, or state at a time and observe the result.
5. Revisit the summary and keywords index when using the repository as a reference.
6. Check reduced-motion behavior and keyboard interaction before reusing an animation in an interface.

## Quick guides

- [Framer Motion summary](framerMotion-summary.md)
- [Framer Motion keywords and concepts](framerMotion-keywords.md)

## Chapter roadmap

<table width="100%">
  <thead>
    <tr><th width="48">No.</th><th>Lesson</th><th width="72">Q&amp;A</th><th width="88">Example</th></tr>
  </thead>
  <tbody>
    <tr><td>001</td><td><a href="lessons/001-motion-and-react-animation-fundamentals.md">Motion and React animation fundamentals</a></td><td><a href="qnas/001-motion-and-react-animation-fundamentals.md">Open Q&amp;A</a></td><td><a href="examples/001-motion-and-react-animation-fundamentals.md">Open example</a></td></tr>
    <tr><td>002</td><td><a href="lessons/002-installation-and-project-setup.md">Installation and project setup</a></td><td><a href="qnas/002-installation-and-project-setup.md">Open Q&amp;A</a></td><td><a href="examples/002-installation-and-project-setup.md">Open example</a></td></tr>
    <tr><td>003</td><td><a href="lessons/003-motion-components-and-props.md">Motion components and props</a></td><td><a href="qnas/003-motion-components-and-props.md">Open Q&amp;A</a></td><td><a href="examples/003-motion-components-and-props.md">Open example</a></td></tr>
    <tr><td>004</td><td><a href="lessons/004-initial-and-animate-states.md">Initial and animate states</a></td><td><a href="qnas/004-initial-and-animate-states.md">Open Q&amp;A</a></td><td><a href="examples/004-initial-and-animate-states.md">Open example</a></td></tr>
    <tr><td>005</td><td><a href="lessons/005-transitions-and-springs.md">Transitions and springs</a></td><td><a href="qnas/005-transitions-and-springs.md">Open Q&amp;A</a></td><td><a href="examples/005-transitions-and-springs.md">Open example</a></td></tr>
    <tr><td>006</td><td><a href="lessons/006-keyframes-and-repeats.md">Keyframes and repeats</a></td><td><a href="qnas/006-keyframes-and-repeats.md">Open Q&amp;A</a></td><td><a href="examples/006-keyframes-and-repeats.md">Open example</a></td></tr>
    <tr><td>007</td><td><a href="lessons/007-variants-and-reusable-states.md">Variants and reusable states</a></td><td><a href="qnas/007-variants-and-reusable-states.md">Open Q&amp;A</a></td><td><a href="examples/007-variants-and-reusable-states.md">Open example</a></td></tr>
    <tr><td>008</td><td><a href="lessons/008-variant-orchestration-and-stagger.md">Variant orchestration and stagger</a></td><td><a href="qnas/008-variant-orchestration-and-stagger.md">Open Q&amp;A</a></td><td><a href="examples/008-variant-orchestration-and-stagger.md">Open example</a></td></tr>
    <tr><td>009</td><td><a href="lessons/009-hover-tap-and-focus-gestures.md">Hover, tap, and focus gestures</a></td><td><a href="qnas/009-hover-tap-and-focus-gestures.md">Open Q&amp;A</a></td><td><a href="examples/009-hover-tap-and-focus-gestures.md">Open example</a></td></tr>
    <tr><td>010</td><td><a href="lessons/010-pan-drag-and-constraints.md">Pan, drag, and constraints</a></td><td><a href="qnas/010-pan-drag-and-constraints.md">Open Q&amp;A</a></td><td><a href="examples/010-pan-drag-and-constraints.md">Open example</a></td></tr>
    <tr><td>011</td><td><a href="lessons/011-animatepresence-and-exit-animations.md">AnimatePresence and exit animations</a></td><td><a href="qnas/011-animatepresence-and-exit-animations.md">Open Q&amp;A</a></td><td><a href="examples/011-animatepresence-and-exit-animations.md">Open example</a></td></tr>
    <tr><td>012</td><td><a href="lessons/012-presence-modes-and-dynamic-exit-data.md">Presence modes and dynamic exit data</a></td><td><a href="qnas/012-presence-modes-and-dynamic-exit-data.md">Open Q&amp;A</a></td><td><a href="examples/012-presence-modes-and-dynamic-exit-data.md">Open example</a></td></tr>
    <tr><td>013</td><td><a href="lessons/013-layout-animations.md">Layout animations</a></td><td><a href="qnas/013-layout-animations.md">Open Q&amp;A</a></td><td><a href="examples/013-layout-animations.md">Open example</a></td></tr>
    <tr><td>014</td><td><a href="lessons/014-shared-layout-and-layout-groups.md">Shared layout and layout groups</a></td><td><a href="qnas/014-shared-layout-and-layout-groups.md">Open Q&amp;A</a></td><td><a href="examples/014-shared-layout-and-layout-groups.md">Open example</a></td></tr>
    <tr><td>015</td><td><a href="lessons/015-motion-values-and-direct-updates.md">Motion values and direct updates</a></td><td><a href="qnas/015-motion-values-and-direct-updates.md">Open Q&amp;A</a></td><td><a href="examples/015-motion-values-and-direct-updates.md">Open example</a></td></tr>
    <tr><td>016</td><td><a href="lessons/016-transforming-and-composing-motion-values.md">Transforming and composing motion values</a></td><td><a href="qnas/016-transforming-and-composing-motion-values.md">Open Q&amp;A</a></td><td><a href="examples/016-transforming-and-composing-motion-values.md">Open example</a></td></tr>
    <tr><td>017</td><td><a href="lessons/017-scroll-linked-animations-with-usescroll.md">Scroll-linked animations with useScroll</a></td><td><a href="qnas/017-scroll-linked-animations-with-usescroll.md">Open Q&amp;A</a></td><td><a href="examples/017-scroll-linked-animations-with-usescroll.md">Open example</a></td></tr>
    <tr><td>018</td><td><a href="lessons/018-in-view-animations-and-useinview.md">In-view animations and useInView</a></td><td><a href="qnas/018-in-view-animations-and-useinview.md">Open Q&amp;A</a></td><td><a href="examples/018-in-view-animations-and-useinview.md">Open example</a></td></tr>
    <tr><td>019</td><td><a href="lessons/019-imperative-sequences-with-useanimate.md">Imperative sequences with useAnimate</a></td><td><a href="qnas/019-imperative-sequences-with-useanimate.md">Open Q&amp;A</a></td><td><a href="examples/019-imperative-sequences-with-useanimate.md">Open example</a></td></tr>
    <tr><td>020</td><td><a href="lessons/020-svg-and-path-animations.md">SVG and path animations</a></td><td><a href="qnas/020-svg-and-path-animations.md">Open Q&amp;A</a></td><td><a href="examples/020-svg-and-path-animations.md">Open example</a></td></tr>
    <tr><td>021</td><td><a href="lessons/021-css-variables-and-complex-values.md">CSS variables and complex values</a></td><td><a href="qnas/021-css-variables-and-complex-values.md">Open Q&amp;A</a></td><td><a href="examples/021-css-variables-and-complex-values.md">Open example</a></td></tr>
    <tr><td>022</td><td><a href="lessons/022-custom-react-components-with-motion-create.md">Custom React components with motion.create()</a></td><td><a href="qnas/022-custom-react-components-with-motion-create.md">Open Q&amp;A</a></td><td><a href="examples/022-custom-react-components-with-motion-create.md">Open example</a></td></tr>
    <tr><td>023</td><td><a href="lessons/023-global-configuration-with-motionconfig.md">Global configuration with MotionConfig</a></td><td><a href="qnas/023-global-configuration-with-motionconfig.md">Open Q&amp;A</a></td><td><a href="examples/023-global-configuration-with-motionconfig.md">Open example</a></td></tr>
    <tr><td>024</td><td><a href="lessons/024-accessible-and-reduced-motion.md">Accessible and reduced motion</a></td><td><a href="qnas/024-accessible-and-reduced-motion.md">Open Q&amp;A</a></td><td><a href="examples/024-accessible-and-reduced-motion.md">Open example</a></td></tr>
    <tr><td>025</td><td><a href="lessons/025-responsive-motion-patterns.md">Responsive motion patterns</a></td><td><a href="qnas/025-responsive-motion-patterns.md">Open Q&amp;A</a></td><td><a href="examples/025-responsive-motion-patterns.md">Open example</a></td></tr>
    <tr><td>026</td><td><a href="lessons/026-bundle-optimization-with-lazymotion.md">Bundle optimization with LazyMotion</a></td><td><a href="qnas/026-bundle-optimization-with-lazymotion.md">Open Q&amp;A</a></td><td><a href="examples/026-bundle-optimization-with-lazymotion.md">Open example</a></td></tr>
    <tr><td>027</td><td><a href="lessons/027-nextjs-server-rendering-and-react-server-components.md">Next.js, server rendering, and React Server Components</a></td><td><a href="qnas/027-nextjs-server-rendering-and-react-server-components.md">Open Q&amp;A</a></td><td><a href="examples/027-nextjs-server-rendering-and-react-server-components.md">Open example</a></td></tr>
    <tr><td>028</td><td><a href="lessons/028-react-router-transitions-and-navigation.md">React Router transitions and navigation</a></td><td><a href="qnas/028-react-router-transitions-and-navigation.md">Open Q&amp;A</a></td><td><a href="examples/028-react-router-transitions-and-navigation.md">Open example</a></td></tr>
    <tr><td>029</td><td><a href="lessons/029-migrating-from-framer-motion-to-motion.md">Migrating from framer-motion to Motion</a></td><td><a href="qnas/029-migrating-from-framer-motion-to-motion.md">Open Q&amp;A</a></td><td><a href="examples/029-migrating-from-framer-motion-to-motion.md">Open example</a></td></tr>
    <tr><td>030</td><td><a href="lessons/030-performance-and-debugging.md">Performance and debugging</a></td><td><a href="qnas/030-performance-and-debugging.md">Open Q&amp;A</a></td><td><a href="examples/030-performance-and-debugging.md">Open example</a></td></tr>
    <tr><td>031</td><td><a href="lessons/031-testing-motion-components.md">Testing Motion components</a></td><td><a href="qnas/031-testing-motion-components.md">Open Q&amp;A</a></td><td><a href="examples/031-testing-motion-components.md">Open example</a></td></tr>
    <tr><td>032</td><td><a href="lessons/032-capstone-accessible-animated-interface.md">Capstone: accessible animated interface</a></td><td><a href="qnas/032-capstone-accessible-animated-interface.md">Open Q&amp;A</a></td><td><a href="examples/032-capstone-accessible-animated-interface.md">Open example</a></td></tr>
  </tbody>
</table>

## Repository structure

- `README.md` is the roadmap and entry point.
- `framerMotion-summary.md` reviews the course-wide concepts.
- `framerMotion-keywords.md` indexes APIs and terminology.
- `lessons/` explains concepts in sequence.
- `qnas/` answers practical questions for each chapter.
- `examples/` provides focused implementations for each chapter.
- `LICENSE` covers these personal notes only.

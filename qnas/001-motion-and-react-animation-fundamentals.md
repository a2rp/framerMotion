# Motion and React animation fundamentals Q&A

## 1. What is Motion for React?

Motion for React is a library that adds animation and interaction props to React components. It was formerly known as Framer Motion. Current projects use the `motion` package and import React APIs from `motion/react`. Older projects may still have the `framer-motion` package and import path.

## 2. Does Motion replace React state?

No. React state should continue to describe meaningful interface state, such as whether a dialog is open or which item is selected. Motion reads the resulting target and interpolates visual values. A Motion value is more appropriate than React state for rapidly changing visual data such as scroll progress.

## 3. What is declarative animation?

Declarative animation describes the desired visual target in component props. For example, `animate={{ opacity: open ? 1 : 0 }}` says the target depends on the React value `open`. Motion handles the interpolation. This keeps the animation near the UI state that controls it.

## 4. Is `motion.div` a new HTML tag?

No. `motion.div` is a React component that renders a regular `div` and accepts Motion props. Motion has corresponding components for supported HTML and SVG elements. Use the element that matches the content's actual meaning, such as `motion.button` for an action.

## 5. Does animating a Motion value make React render on every frame?

No. Motion updates supported animated values outside React's render loop. React still renders when its own state or props change. This distinction is useful for smooth visual updates without putting every animation frame into React state.

## 6. Should every hover or transition use Motion?

No. A simple CSS transition can be enough for a one-off style change. Motion is useful when targets depend on React state, several components need coordination, an element needs an exit animation, or values are linked to scroll or gestures. Use the smallest solution that fits the behavior.

## 7. Does an opacity animation remove content from the page?

No. Setting opacity to zero changes appearance only. The element can remain in the DOM, keyboard tab order, and accessibility tree. If content is unavailable, update its semantic and interaction state too. When an element should be removed after animating out, use `AnimatePresence` with an `exit` target.

## 8. How do I make an animated toggle accessible?

Use a native button and reflect the toggle state with `aria-pressed` or another appropriate semantic state. Keep keyboard focus visible. The animation should provide visual feedback while the button's text, role, and state communicate the control's purpose.

## 9. Why do stable React keys matter for animation?

Keys tell React which list item remains the same across renders. If keys change or are based on an unstable array position, React can remount the wrong child. That can cause entrance, exit, or layout animations to play unexpectedly. Prefer stable data identifiers.

## 10. What is the difference between a Motion component and a Motion value?

A Motion component is a React component such as `motion.div` that renders an element and accepts animation props. A Motion value is a reactive value that can update a visual property without storing every frame in React state. They are often used together through the `style` prop.

## 11. Which values can Motion animate?

Motion supports common visual values, including transforms, opacity, colors, many CSS properties, and SVG attributes. Interpolation depends on the property and compatible start and end values. Verify complex or browser-specific values in the installed version instead of assuming every CSS value works identically.

## 12. Should I start with `initial` or `animate`?

`initial` describes the starting visual target. `animate` describes the target Motion should reach. For an element that should appear already in its final state during server rendering or first paint, `initial={false}` can be appropriate. The next lesson explains how to select these states.

## 13. Do I need imperative animation code for ordinary interactions?

Usually not. Use declarative props when an animation follows React state or a gesture. Use `useAnimate` when an event needs a sequence, scoped selectors, or explicit playback control. Do not move application state into imperative animation code just to trigger a transition.

## 14. What React version do these notes assume?

The current Motion for React installation guide requires React 18.2 or newer. Check the requirements of the exact Motion release used by your project. The examples use current imports from `motion/react`.

## 15. Does the MIT license in this repository cover the Motion package?

No. The repository license covers these learning notes. Motion, React, and other dependencies keep their own license terms. Review the license of each package used by an application.

## 16. What should I learn next?

Install the package and confirm that a basic React component renders, then study how `initial` and `animate` describe a state change. Continue to [installation and project setup](../lessons/002-installation-and-project-setup.md).

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td>Start of course</td><td><a href="../lessons/001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals lesson</a></td><td><a href="../examples/001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals example</a></td><td><a href="002-installation-and-project-setup.md">002 - Installation and project setup Q&amp;A</a></td></tr>
  </tbody>
</table>

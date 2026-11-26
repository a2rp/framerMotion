# SVG and path animations example

This example draws a simple route inside a responsive SVG. A highlighted point follows the route through a motion path, while a button changes the viewBox to focus the route. The route stays visible and named when reduced motion is preferred.

## Setup

Install Motion with `npm install motion`. Save as `src/AnimatedRoute.jsx` and render `<AnimatedRoute />`.

## `src/AnimatedRoute.jsx`

~~~jsx
import { useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const route = "M 70 220 C 120 205, 135 125, 205 145 S 285 210, 355 150 S 445 65, 530 85";
const sideRoute = "M 205 145 C 225 95, 275 70, 320 62";

export function AnimatedRoute() {
  const [focused, setFocused] = useState(false);
  const shouldReduceMotion = useReducedMotion();
  const duration = shouldReduceMotion ? 0 : 1.6;

  return (
    <main
      style={{
        maxWidth: 760,
        margin: "48px auto",
        padding: 24,
        color: "#172033",
        fontFamily: "system-ui, sans-serif",
        lineHeight: 1.6,
      }}
    >
      <h1>Animated route map</h1>
      <p>
        The route draws from left to right. Use the button to focus the middle
        section of the map.
      </p>

      <motion.svg
        role="img"
        aria-labelledby="route-title route-description"
        viewBox="0 0 600 300"
        animate={{
          viewBox: focused ? "135 35 340 190" : "0 0 600 300",
        }}
        transition={{ duration: shouldReduceMotion ? 0 : 0.5, ease: "easeInOut" }}
        style={{
          display: "block",
          width: "100%",
          height: "auto",
          overflow: "visible",
          border: "1px solid #cbd5e1",
          borderRadius: 16,
          background: "#f8fafc",
        }}
      >
        <title id="route-title">Route with two connected paths</title>
        <desc id="route-description">
          A purple route travels from the lower left to the upper right, with a
          branch near the middle and a moving point along the main route.
        </desc>

        <motion.path
          d={route}
          fill="none"
          stroke="#cbd5e1"
          strokeWidth="12"
          strokeLinecap="round"
          strokeLinejoin="round"
          initial={shouldReduceMotion ? { pathLength: 1 } : { pathLength: 0 }}
          animate={{ pathLength: 1 }}
          transition={{ duration, ease: "easeInOut" }}
        />

        <motion.path
          d={sideRoute}
          fill="none"
          stroke="#0f766e"
          strokeWidth="7"
          strokeLinecap="round"
          initial={shouldReduceMotion ? { pathLength: 1 } : { pathLength: 0 }}
          animate={{ pathLength: 1 }}
          transition={{
            duration: shouldReduceMotion ? 0 : 0.8,
            delay: shouldReduceMotion ? 0 : 0.45,
            ease: "easeOut",
          }}
        />

        <motion.path
          d={route}
          fill="none"
          stroke="#7c3aed"
          strokeWidth="5"
          strokeLinecap="round"
          strokeLinejoin="round"
          initial={shouldReduceMotion ? { pathLength: 1 } : { pathLength: 0 }}
          animate={{ pathLength: 1 }}
          transition={{ duration, ease: "easeInOut" }}
        />

        <motion.circle
          cx="70"
          cy="220"
          r="9"
          fill="#f97316"
          initial={shouldReduceMotion ? { pathLength: 1 } : { pathLength: 0 }}
          animate={{ pathLength: 1 }}
          transition={{ duration, ease: "linear" }}
          style={{
            offsetPath: `path("${route}")`,
            offsetDistance: shouldReduceMotion ? "100%" : "100%",
          }}
        />
      </motion.svg>

      <button
        type="button"
        onClick={() => setFocused((value) => !value)}
        style={{ marginTop: 18, padding: "10px 14px" }}
      >
        {focused ? "Show full route" : "Focus route"}
      </button>

      <p>
        The route and branch are meaningful content. Reduced motion shows the
        completed paths immediately rather than animating the drawing.
      </p>
    </main>
  );
}
~~~

## Important note about the moving point

The `offsetPath`/`offsetDistance` feature animates a shape along a CSS motion path where supported. For a reliable cross-browser learning exercise, first verify it in the target browsers. The route drawing itself uses Motion's SVG `pathLength` feature and does not depend on the moving point.

If the point is not positioned correctly, use an SVG-specific path-following implementation or replace it with a static marker. Do not leave the marker at a misleading location.

## How it works

The SVG uses a `viewBox` of `0 0 600 300` so its internal coordinates stay stable while the rendered width changes. Each route stroke is a `motion.path` with `pathLength` moving from 0 to 1. The thicker gray stroke acts as a background track; the purple path draws on top, and the teal branch has a shorter reveal.

The button toggles the SVG's `viewBox` between the full map and a closer framing. That is a whole-graphic zoom, so the reduced-motion setting removes the transition. The paths render at their final length immediately when reduced motion is active.

The route graphic has a title and description. If it is used only as decoration in another layout, hide it from assistive technology and provide any essential directions as ordinary text.

## Expected behavior

- The main route draws across the map on mount.
- The branch draws shortly after the main route begins.
- The button focuses the center of the route and restores the full view on the next activation.
- The SVG scales to the available width while preserving its viewBox geometry.
- With reduced motion, the complete route appears without a drawing animation.

## Adapt the example

- Remove the gray track if only the animated route should be visible.
- Change stroke width, line caps, and line joins to match the design.
- Add a shape whose `d` value changes between compatible path descriptions.
- Use `pathOffset` with `pathLength` for a partial progress segment.
- Replace the optional moving point with a static marker if path following is not supported by the chosen browser.
- Animate a circle's `r` or a rect's `attrX` to practice SVG attributes.

## Common mistakes

- **Path hidden:** confirm stroke, width, opacity, and pathLength.
- **Path clipped:** compare coordinates against the viewBox.
- **Fill covers the interior:** set fill to none for a stroke-only path.
- **Attribute/transform confusion:** use `attrX` or `attrY` for SVG coordinates.
- **Unexpected pivot:** inspect transformBox and the SVG coordinate system.
- **Unreliable path morph:** use compatible path commands and point structures.
- **Animation carries meaning:** include an accessible title/description or equivalent text.
- **Reduced motion ignored:** show the final graphic without travel.

## Practice

1. Remove the gray track and compare the visual clarity.
2. Change the viewBox dimensions and keep all route points in bounds.
3. Animate a circle radius or rectangle attribute using Motion.
4. Try a partial path with pathOffset and pathLength.
5. Test the route in reduced-motion mode and keep the completed graphic visible.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate example</a></td><td><a href="../lessons/020-svg-and-path-animations.md">020 - SVG and path animations lesson</a></td><td><a href="../qnas/020-svg-and-path-animations.md">020 - SVG and path animations Q&amp;A</a></td><td><a href="021-css-variables-and-complex-values.md">021 - CSS variables and complex values example</a></td></tr></tbody>
</table>

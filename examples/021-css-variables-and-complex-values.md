# CSS variables and complex values example

This card changes theme through two inherited CSS custom properties. A shadow interpolates between compatible strings, and a button controls the discrete theme choice.

## Setup

Install Motion with `npm install motion`. Save as `src/ThemeCard.jsx` and render `<ThemeCard />`.

## `src/ThemeCard.jsx`

~~~jsx
import { useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const themes = {
  violet: {
    name: "Violet",
    accent: "#6d28d9",
    soft: "#f5f3ff",
    shadow: "0 12px 32px rgba(124, 58, 237, 0.18)",
  },
  teal: {
    name: "Teal",
    accent: "#0f766e",
    soft: "#f0fdfa",
    shadow: "0 12px 32px rgba(15, 118, 110, 0.18)",
  },
};

export function ThemeCard() {
  const [active, setActive] = useState("violet");
  const shouldReduceMotion = useReducedMotion();
  const theme = themes[active];

  return (
    <main
      style={{
        maxWidth: 640,
        margin: "64px auto",
        padding: 24,
        fontFamily: "system-ui, sans-serif",
        color: "#172033",
      }}
    >
      <motion.article
        initial={false}
        animate={{
          "--card-accent": theme.accent,
          "--card-surface": theme.soft,
          boxShadow: theme.shadow,
        }}
        transition={{
          duration: shouldReduceMotion ? 0 : 0.4,
          ease: "easeInOut",
        }}
        style={{
          "--card-accent": themes.violet.accent,
          "--card-surface": themes.violet.soft,
          boxShadow: themes.violet.shadow,
          padding: 28,
          border: "2px solid var(--card-accent)",
          borderRadius: 20,
          background: "var(--card-surface)",
        }}
      >
        <p style={{ color: "var(--card-accent)", fontWeight: 700 }}>
          Shared theme values
        </p>
        <h1 style={{ color: "var(--card-accent)" }}>{theme.name} interface</h1>
        <p>
          The accent and surface variables are defined on this card and
          consumed by its descendants. The shadow animates as a compatible
          complex value.
        </p>
        <button
          type="button"
          aria-pressed={active === "teal"}
          onClick={() =>
            setActive((value) => (value === "violet" ? "teal" : "violet"))
          }
          style={{
            padding: "10px 14px",
            border: 0,
            borderRadius: 8,
            background: "var(--card-accent)",
            color: "white",
            font: "inherit",
            cursor: "pointer",
          }}
        >
          Switch to {active === "violet" ? "teal" : "violet"}
        </button>
        <p>Selected theme: <strong>{theme.name}</strong></p>
      </motion.article>
    </main>
  );
}
~~~

## How it works

React state selects a theme. Motion animates `--card-accent` and `--card-surface` on the card. Descendants read them through CSS `var()`, so the border, headings, button, and surface share one source.

Both `boxShadow` values contain matching lengths, a blur radius, and one RGBA color. Motion can interpolate their numeric and color parts. When reduced motion is preferred, the transition is immediate.

Animating custom properties triggers paint. Here they are useful because several descendants share the changing values. For one element moving across the page, a direct Motion transform is generally a better fit.

## Expected behavior

- The card starts with the violet theme.
- The button switches to teal and updates its pressed state.
- Headings, border, button, and surface follow the shared variables.
- The shadow changes between compatible complex strings.
- Reduced motion makes the theme change immediate.

## Adapt the example

- Add another theme with the same semantic values.
- Add an icon whose fill uses `var(--card-accent)`.
- Add a CSS fallback and temporarily remove the variable.
- Change the shadow endpoints while preserving their structure.
- Replace one shared variable with a direct Motion target and compare clarity.

## Common mistakes

- **Variable has no effect:** check its spelling, cascade scope, and the consuming `var()` declaration.
- **No interpolation:** make sure both endpoints are valid and structurally compatible.
- **Shadow jumps:** match its layer count, numbers, units, and colors.
- **Too much painting:** avoid animating shared variables every frame when a direct Motion value works.
- **Theme conveyed only by color:** retain a visible theme name and semantic pressed state.
- **Reduced motion ignored:** make the transition immediate when requested.

## Practice

1. Add another theme and keep its label synchronized.
2. Change one shadow endpoint's structure, observe the result, then restore matching values.
3. Use one custom property across three descendants.
4. Replace one variable with a direct target and compare readability.
5. Enable reduced motion and confirm the final colors remain clear.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="020-svg-and-path-animations.md">020 - SVG and path animations example</a></td><td><a href="../lessons/021-css-variables-and-complex-values.md">021 - CSS variables and complex values lesson</a></td><td><a href="../qnas/021-css-variables-and-complex-values.md">021 - CSS variables and complex values Q&amp;A</a></td><td><a href="022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create() example</a></td></tr></tbody>
</table>

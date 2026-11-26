# Global configuration with MotionConfig

`MotionConfig` provides configuration for Motion components beneath it in the React tree. Use it to set a shared fallback transition or a consistent reduced-motion policy. It can also pass a Content Security Policy nonce to Motion-generated style blocks and adjust pointer coordinates for special drag setups.

Wrap the part of the app that should share the configuration, often near the application root:

~~~jsx
import { MotionConfig } from "motion/react";

export function AppMotionBoundary({ children }) {
  return (
    <MotionConfig
      transition={{ type: "spring", stiffness: 280, damping: 26 }}
      reducedMotion="user"
    >
      {children}
    </MotionConfig>
  );
}
~~~

The wrapper affects Motion components below it. It does not turn ordinary CSS animations or unrelated libraries into Motion animations.

## Set a fallback transition

The `transition` prop defines a fallback transition for child Motion components. Components can still define their own animation targets and more specific transitions.

~~~jsx
<MotionConfig transition={{ duration: 0.35, ease: "easeOut" }}>
  <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} />
</MotionConfig>
~~~

A shared transition makes timing consistent and reduces repeated configuration. It should still fit the interface: a spring may suit a small interactive control but feel wrong for a long opacity fade.

### Local transition specificity

A transition declared closer to a component is more specific. By default, a local transition replaces the inherited fallback rather than automatically merging every value. If a local transition should keep the global defaults while changing one detail, set `inherit: true`:

~~~jsx
<motion.div
  animate={{ x: 0, opacity: 1 }}
  transition={{
    inherit: true,
    opacity: { duration: 0.18 },
  }}
/>
~~~

Here the default transition remains available for other values while opacity gets a local duration. Without `inherit: true`, a local transition can use Motion's ordinary defaults for values it does not specify. Keep local exceptions intentional so a global policy remains understandable.

## Choose a reduced-motion policy

`reducedMotion` controls how Motion responds to the operating-system preference:

| Value | Meaning |
| --- | --- |
| `"user"` | Respect the device's reduced-motion setting |
| `"always"` | Always enable Motion's reduced-motion behavior, useful for checking |
| `"never"` | Do not apply the preference-based behavior; this is the default |

When reduced motion is enabled, Motion disables transform and layout animations while other properties such as opacity or background color can continue. It does not mean every animation becomes static. Plan the final experience deliberately, and use `useReducedMotion` for custom behavior that needs more than Motion's automatic handling.

For example, the global policy can remove a large position transition while a subtle opacity change remains. Avoid relying on a transform to convey essential state.

## Use a Content Security Policy nonce

If a site uses a Content Security Policy nonce for inline styles, pass the same nonce to MotionConfig's `nonce` prop so Motion-generated style blocks can follow that policy:

~~~jsx
<MotionConfig nonce={styleNonce}>
  <App />
</MotionConfig>
~~~

Only add a nonce when the application has a matching CSP setup. The value must match the nonce used by the page's security policy; an arbitrary value does not authorize a style block.

## Adjust pointer coordinates when necessary

`transformPagePoint` can map pointer coordinates before Motion uses them. This can help with a known coordinate scaling mismatch, for example when a drag region is rendered at a different scale from the page coordinate space.

~~~jsx
<MotionConfig
  transformPagePoint={({ x, y }) => ({ x: x / 2, y: y / 2 })}
>
  <DragSurface />
</MotionConfig>
~~~

This is specialized configuration. Most applications should leave pointer coordinates alone. If scaling an SVG viewBox or another coordinate space, use the corresponding element-aware conversion rather than guessing a constant factor.

## Scope and organization

Put a MotionConfig boundary around the components that should share defaults. A local MotionConfig can provide a different policy for a distinct part of the tree, such as an embedded editor or preview. Keep boundaries easy to locate and avoid wrapping every individual element with its own provider.

A useful division is:

- One app-level transition fallback and reduced-motion policy.
- Local transitions where a particular interaction needs different timing.
- A localized config for a subtree with a genuine different requirement.
- Explicit user preference handling for custom canvas, video, or non-Motion effects.

Do not use global configuration to hide inconsistent component behavior. If one component needs a special effect, define that exception near the component and explain why.

## Accessibility and testing

Using `reducedMotion="user"` creates a consistent baseline, but review the interface with the preference enabled. Motion automatically disables transform and layout animations; your own CSS, videos, imperative sequences, and custom logic may need separate handling.

Use `reducedMotion="always"` during development or a story to inspect the reduced-motion behavior. Restore the desired user policy afterward. Do not leave a test-only policy in production unless always-reduced behavior is the product decision.

## Troubleshooting

1. **A child ignores the default transition:** check that it is inside the MotionConfig subtree and has no more-specific local transition.
2. **A local transition loses global values:** set `inherit: true` if it should merge with the fallback.
3. **A transform still moves with reducedMotion user:** check whether reduced motion is enabled and whether the animation uses Motion transforms or custom CSS.
4. **Opacity continues under reduced motion:** that is expected; non-transform values may remain animated.
5. **A custom animation is unaffected:** MotionConfig does not control unrelated animation systems; adapt them with their own logic.
6. **CSP blocks styles:** verify the Motion nonce matches the page policy.
7. **Drag coordinates are wrong:** inspect actual coordinate scaling before adding transformPagePoint.
8. **Configuration is hard to trace:** reduce unnecessary nested boundaries.

## Recap and practice

MotionConfig sets subtree-level defaults for transitions, reduced motion, CSP nonce, and pointer coordinate mapping. Global settings provide consistency; local transitions handle justified exceptions. A local transition replaces the fallback by default, and `inherit: true` opts into inheriting it.

Practice by setting an app-wide transition and `reducedMotion="user"`. Add a local opacity timing override, compare it with and without inheritance, then inspect the interface with reduced motion enabled.

Continue with [accessible and reduced motion](024-accessible-and-reduced-motion.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="022-custom-react-components-with-motion-create.md">022 - Custom React components with motion.create()</a></td><td><a href="../qnas/023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig Q&amp;A</a></td><td><a href="../examples/023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig example</a></td><td><a href="024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion</a></td></tr></tbody>
</table>

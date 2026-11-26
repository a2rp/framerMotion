# Bundle optimization with LazyMotion

Motion for React makes animation convenient, but the standard `motion` component includes a broad set of animation features. When an application needs only a smaller subset, `LazyMotion` and the lightweight `m` components let you choose which features are loaded and when.

Bundle optimization is a measurement-led choice. Start with the normal API, inspect the production build, and use `LazyMotion` when its smaller initial feature set is useful for the application.

## Learning outcomes

By the end of this lesson, you should be able to:

- Explain what `LazyMotion` and `m` do.
- Choose between the `domAnimation` and `domMax` feature bundles.
- Configure synchronous and asynchronous feature loading.
- Avoid accidentally loading the full `motion` component inside a lazy feature boundary.
- Verify the actual production bundle and preserve accessible behavior while optimizing.

## Why use LazyMotion?

The standard `motion` component is designed for convenience and ships with its feature functionality available. That makes it a strong default when a component needs animations, gestures, drag, or layout features without extra setup.

`LazyMotion` separates the component API from the feature bundle. Import `m` from `motion/react-m`, then provide the required bundle to a `LazyMotion` boundary. This allows an application to include only the feature set it needs or defer loading that set until after the initial render.

Reported size figures depend on the package version, bundler, minification, compression, and imported features. Treat documentation figures as context, not as a guaranteed saving for your application. Compare production builds with the same build settings.

## The lightweight m component

The `m` components use the same declarative props style as `motion`, including props such as `animate`, `initial`, `exit`, `variants`, and gesture props when the selected feature bundle supports them.

~~~jsx
import { LazyMotion, domAnimation } from "motion/react";
import * as m from "motion/react-m";

function Notice() {
  return (
    <LazyMotion features={domAnimation}>
      <m.div initial={{ opacity: 0 }} animate={{ opacity: 1 }}>
        Ready
      </m.div>
    </LazyMotion>
  );
}
~~~

The `m` component does not preload the same complete set of features as `motion`. The `LazyMotion` boundary supplies the feature definitions needed to make the animation props work.

Keep the boundary above the components that use `m`. A component rendered outside the boundary might not receive the configured features.

## Choose a feature bundle

Motion currently documents two ready-made DOM feature bundles:

| Feature bundle | What it provides | Use it when |
| --- | --- | --- |
| `domAnimation` | Animations, variants, exit animations, and tap, hover, and focus gestures | The interface needs common animation and interaction features, without drag or layout animation |
| `domMax` | The `domAnimation` features plus pan and drag gestures and layout animations | The interface needs drag or layout animation |

Choose the smallest bundle that supports the props used by the application. If the app requires a feature that is not included, the corresponding behavior will not be available through that bundle. Do not choose a smaller bundle by removing an interaction users rely on.

## Synchronous feature loading

A synchronous bundle is imported into the main JavaScript chunk. This is straightforward and avoids a separate feature-loading delay:

~~~jsx
import { LazyMotion, domAnimation } from "motion/react";
import * as m from "motion/react-m";

export function MotionRoot({ children }) {
  return (
    <LazyMotion features={domAnimation}>
      {children}
    </LazyMotion>
  );
}
~~~

This setup can reduce the feature code included for the motion elements while keeping it available at the initial render. It is a useful first step when the application only needs a smaller feature subset.

## Asynchronous feature loading

For a bundler that supports dynamic imports, provide `LazyMotion` with a function that loads the feature bundle. Motion can then defer that code until after the initial render.

~~~js
// src/motion-features.js
import { domAnimation } from "motion/react";

export default domAnimation;
~~~

~~~jsx
// src/MotionRoot.jsx
import { LazyMotion } from "motion/react";
import * as m from "motion/react-m";

const loadFeatures = () =>
  import("./motion-features.js").then((module) => module.default);

export function MotionRoot({ children }) {
  return (
    <LazyMotion features={loadFeatures}>
      {children}
    </LazyMotion>
  );
}

export function Intro() {
  return (
    <m.div initial={{ opacity: 0 }} animate={{ opacity: 1 }}>
      Motion features load on demand.
    </m.div>
  );
}
~~~

The dynamic import creates a separate chunk when the bundler supports code splitting. The animation waits for the feature function to resolve. Account for that timing: do not make important content inaccessible or permanently invisible while optional animation features load. A visible, usable static state should remain understandable.

For a component that needs drag or layout animations, the feature module can export `domMax` instead. Choose that bundle intentionally because it includes a broader set of features.

## Keep full motion components out of the lazy boundary

Importing and rendering a normal `motion` component within a `LazyMotion` boundary can load the full feature implementation and remove the expected bundle-size benefit.

~~~jsx
import { LazyMotion, domAnimation, motion } from "motion/react";
import * as m from "motion/react-m";

function Incorrect() {
  return (
    <LazyMotion features={domAnimation}>
      <motion.div animate={{ opacity: 1 }} />
    </LazyMotion>
  );
}

function Correct() {
  return (
    <LazyMotion features={domAnimation}>
      <m.div animate={{ opacity: 1 }} />
    </LazyMotion>
  );
}
~~~

During an incremental migration, search for all animated components inside the boundary. Use `m` consistently where the lazy feature set is intended.

## Use strict mode as a guard

Set `strict` on `LazyMotion` while adopting this pattern:

~~~jsx
<LazyMotion features={domAnimation} strict>
  {children}
</LazyMotion>
~~~

Strict mode throws an error if a normal `motion` component renders within that boundary. This catches an accidental import that could undo the optimization. Remove strict mode only if the application has a deliberate reason and the bundle impact is understood.

## A practical optimization workflow

1. Establish a baseline from the production build, not a development server.
2. Use the bundle analyzer or build output to identify what Motion contributes.
3. Replace the relevant `motion` components with `m` components.
4. Wrap them in a shared `LazyMotion` boundary.
5. Select `domAnimation` or `domMax` according to the features the interface actually uses.
6. Prefer synchronous loading if the feature set should be available immediately. Use async loading when deferring that code is useful and the loading behavior is acceptable.
7. Enable `strict` during migration.
8. Rebuild using the same production settings and compare the relevant chunks.
9. Test animations, exit states, gestures, drag, layout changes, keyboard behavior, and reduced-motion behavior that the application uses.
10. Keep the change only if the measured result is useful and the interface still behaves correctly.

## Common mistakes

- Using `motion` instead of `m` inside the lazy boundary.
- Selecting `domAnimation` while relying on drag or layout animation.
- Assuming a package's headline size equals the code shipped by the production app.
- Measuring development output or comparing builds with different settings.
- Hiding content until an asynchronously loaded feature bundle resolves.
- Removing an animation feature without checking the underlying interaction and accessible state.
- Adding multiple unnecessary boundaries without measuring their effect.

## When not to use LazyMotion

Use the regular `motion` component when its simplicity is more valuable than the measured bundle reduction, when the interface depends on several feature groups, or when managing a feature boundary would add avoidable complexity.

Optimization should follow evidence. If the production bundle is already acceptable, keep the simpler implementation and revisit only when profiling identifies a real need.

## Practice

Take one animated component in a small app and make two production builds: one with `motion` and one with `m` plus `LazyMotion`. Record the build command, bundler, chunk sizes, and features used. Confirm the changed component still supports its required animations and interactions.

Continue with [Next.js, server rendering, and React Server Components](027-nextjs-server-rendering-and-react-server-components.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="025-responsive-motion-patterns.md">025 - Responsive motion patterns</a></td><td><a href="../qnas/026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion Q&amp;A</a></td><td><a href="../examples/026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion example</a></td><td><a href="027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components</a></td></tr></tbody>
</table>

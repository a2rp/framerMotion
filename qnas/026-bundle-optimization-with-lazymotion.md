# Bundle optimization with LazyMotion Q&A

## 1. What problem does LazyMotion solve?

It lets an application choose when Motion feature code is loaded and which supported feature bundle is used. This can reduce the initial JavaScript needed by animated components when the application does not need the full feature set.

## 2. What is the difference between motion and m?

The regular `motion` component includes Motion's feature functionality for convenience. The lightweight `m` components use a similar props API but rely on a surrounding `LazyMotion` boundary to provide the feature definitions.

## 3. Do I have to use m with LazyMotion?

Use `m` when the goal is to get the lazy feature-loading benefit. A normal `motion` component rendered inside the boundary can load the full feature implementation and defeat that benefit.

## 4. Where do I import m from?

For the current Motion for React package, import it as a namespace from `motion/react-m`:

~~~jsx
import * as m from "motion/react-m";
~~~

## 5. Where do I import LazyMotion and feature bundles from?

Import `LazyMotion` and the selected bundle, such as `domAnimation`, from `motion/react`:

~~~jsx
import { LazyMotion, domAnimation } from "motion/react";
~~~

## 6. What does domAnimation include?

It provides animations, variants, exit animations, and tap, hover, and focus gestures. It does not include pan and drag gestures or layout animations.

## 7. What does domMax include?

It includes the `domAnimation` features plus pan and drag gestures and layout animations. Select it when the interface actually uses those capabilities.

## 8. Which bundle should I choose?

Choose the smallest feature bundle that supports every animation and interaction used by the application. Do not select `domAnimation` if users rely on drag or layout animation; use a bundle that includes those features.

## 9. What is synchronous feature loading?

Pass the feature bundle directly to `LazyMotion`, for example `features={domAnimation}`. The feature code is available with the main bundle and does not wait for a separate dynamic import.

## 10. What is asynchronous feature loading?

Pass a function that dynamically imports a module and resolves to the feature bundle. A bundler that supports code splitting can place that module in a separate chunk and load it after the initial render.

## 11. What should the async feature function resolve to?

It should resolve to the actual feature bundle, not just the module namespace:

~~~js
const loadFeatures = () =>
  import("./motion-features.js").then((module) => module.default);
~~~

The imported file can export `domAnimation` or `domMax` as its default export.

## 12. Will an animation start before asynchronous features arrive?

The feature function must resolve before the feature-dependent animation can run. Plan for that delay. Keep important content visible and usable without waiting for an animation.

## 13. Does LazyMotion automatically make the total app bundle smaller?

No fixed saving is guaranteed. The outcome depends on what the app imports, the bundler, production build settings, compression, and whether the feature bundle is synchronous or asynchronous. Measure the built application.

## 14. How should I compare bundle size?

Build both versions with the same production settings, then compare the relevant initial and deferred chunks using the project's bundle output or analyzer. Record the bundler and settings so the comparison is meaningful.

## 15. What does strict mode do?

`strict` makes `LazyMotion` throw if a regular `motion` component renders inside its boundary. It helps catch an import that would undermine the intended feature loading.

## 16. Can I use AnimatePresence with LazyMotion?

Yes, when the selected feature set supports the exit animation behavior and the rendered animated elements use `m`. Verify that the exit transition works in the actual application.

## 17. Can I use layout or drag props with domAnimation?

No. The documented `domAnimation` bundle does not include layout animations or pan and drag gestures. Use `domMax` when those features are required.

## 18. Can I put every animated page under one LazyMotion boundary?

A shared boundary can make feature configuration consistent, but choose its location according to the app's component structure and loading needs. Avoid adding boundaries without a clear purpose, and inspect the production chunks to confirm the result.

## 19. Should I hide content until an async feature bundle loads?

No. Important content should remain understandable and usable while the bundle loads. Treat animation as enhancement and provide a sensible visible state.

## 20. Does LazyMotion replace reduced-motion support?

No. Bundle selection controls available Motion features and loading. Respecting a user's reduced-motion preference is a separate accessibility requirement.

## 21. What is a good migration checklist?

- Measure a production baseline.
- Replace the relevant `motion` components with `m`.
- Add a `LazyMotion` boundary.
- Choose a bundle that includes every needed feature.
- Enable `strict` during migration.
- Compare production output using identical settings.
- Test entrance, exit, gesture, drag, and layout behavior that the app uses.
- Keep the optimization only if the measured result is worthwhile and behavior remains correct.

## 22. When should I keep the regular motion component?

Keep it when the simpler setup is preferable, the measured bundle is acceptable, or the interface needs a feature combination that makes a smaller bundle an unhelpful trade-off.

## 23. What should I practice?

Convert one animated component from `motion` to `m`, first with synchronous `domAnimation`. Compare production output, then try asynchronous loading if deferring features is useful. Check the content and interactions before and after the feature chunk loads.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="025-responsive-motion-patterns.md">025 - Responsive motion patterns Q&amp;A</a></td><td><a href="../lessons/026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion lesson</a></td><td><a href="../examples/026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion example</a></td><td><a href="027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components Q&amp;A</a></td></tr></tbody>
</table>

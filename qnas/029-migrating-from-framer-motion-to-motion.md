# Migrating from framer-motion to Motion Q&A

## 1. What changed when Framer Motion became Motion?

For regular React applications, the package is now `motion` and its React APIs are imported from `motion/react`. The migration is often a package and import-path update, followed by any required major-version changes.

## 2. Is the package change itself a complete migration?

Not always. If the app is on an older major version, follow the upgrade notes for each major version from the installed version to the target. The package rename does not automatically update removed APIs or behavior changes.

## 3. How do I change the package with npm?

Remove the old package and add the current one:

~~~bash
npm uninstall framer-motion
npm install motion
~~~

Commit the package manifest and the resulting lockfile together.

## 4. How do I change the package with Yarn or pnpm?

Use the matching package manager's remove and add commands, then commit its lockfile. Avoid using multiple package managers to edit one project.

## 5. How do I change React imports?

Change React API imports from `framer-motion` to `motion/react`:

~~~jsx
import { motion, AnimatePresence } from "motion/react";
~~~

Keep the imported API names unless the relevant version guide calls out an API change.

## 6. Do I change every framer-motion import?

No. Search the repository and review each match. Code Components and Overrides authored for Framer's platform continue to use the Framer integration import. Regular React application code should use the current Motion package.

## 7. What import should LazyMotion components use?

Import `LazyMotion` and its feature bundle from `motion/react`, then import the lightweight `m` components from `motion/react-m`.

## 8. What import should a React Server Component use?

Use the documented `motion/react-client` entry point for Motion components imported from an RSC module. Stateful logic, hooks, and event handlers still belong in an appropriate Client Component.

## 9. Do I need to rewrite every animation?

Usually not for the package rename alone. Keep existing props and animation definitions, then change only the dependency and import path before making any version-specific fixes.

## 10. How should I handle a deprecated or removed API?

Identify the source version and read the guide for the major that removed or changed it. Update the affected call site to the documented replacement, then verify the intended behavior.

## 11. What replaces exitBeforeEnter?

For versions where `exitBeforeEnter` has been removed, use `mode="wait"` on `AnimatePresence` when the new child should wait for the old child to exit. The wait mode supports only one child at a time.

## 12. What should I do with positionTransition or layoutTransition?

These older layout props were replaced by the `layout` prop and the normal `transition` configuration. Check the source version's upgrade notes and verify size changes, shared layout, and drag behavior after updating.

## 13. What should I use instead of motion.custom?

Use the current documented custom component creation API, such as `motion.create()`, where the installed source version requires that migration. Confirm that the wrapped component forwards the ref and styling needed by Motion.

## 14. Can a major-version upgrade change tests?

Yes. Rendering and scheduling behavior can change between major versions. If a test assumed an animation update was synchronous, use the testing guidance for the target version and await the appropriate render or animation frame.

## 15. Can a major-version upgrade affect styled components?

It can. If a project relied on Motion automatically filtering styling-library props, check the version-specific prop-forwarding notes. Configure prop validity or the styling library's prop filter as documented for the actual versions.

## 16. Should I update across several major versions at once?

Read each intervening major-version guide and make the changes in a deliberate order. Keeping package rename and API changes reviewable makes regressions easier to locate.

## 17. How do I find missed imports?

Search the whole repository, including tests, story files, examples, workspace packages, and build configuration. Classify each remaining `framer-motion` import as either a Framer-specific integration or an app import that still needs migration.

## 18. Should both packages stay installed?

Usually no for a regular application. Remove the old dependency unless a specific workspace or Framer integration still requires it. Check the dependency tree for duplicate copies after installation.

## 19. How do I verify the lockfile?

Use the project's existing package manager to perform the install, inspect the lockfile diff, and confirm that CI and deployment use the same lockfile. Do not edit lockfile records by hand.

## 20. Can the import rename affect the bundle?

Yes, the package resolution and import paths can change the dependency graph. Compare production builds with identical settings and inspect the actual application chunks instead of relying on the package's headline size.

## 21. What should I check after migration?

Check initial and animate states, variants, exit animations, gestures, layout animation, reduced motion, SSR and hydration if used, styled prop forwarding, tests, and the production build.

## 22. What if the old app still works before migration?

Record its version and behavior first. A controlled baseline makes it easier to identify whether an issue came from the package change, an intervening major-version change, or another dependency update.

## 23. How do I roll back safely?

Restore the prior package manifest, lockfile, and imports together. Restoring only one of these can leave the dependency tree inconsistent.

## 24. What should I practice?

In a small React app, record the installed package version and production build. Replace the dependency and React import path, follow any intervening major-version notes, then compare the interaction and build output with the baseline.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation Q&amp;A</a></td><td><a href="../lessons/029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion lesson</a></td><td><a href="../examples/029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion example</a></td><td><a href="030-performance-and-debugging.md">030 - Performance and debugging Q&amp;A</a></td></tr></tbody>
</table>

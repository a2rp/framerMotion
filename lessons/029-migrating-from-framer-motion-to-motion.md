# Migrating from framer-motion to Motion

Motion for React was previously distributed as `framer-motion`. For a regular React application, the current package is `motion` and React APIs are imported from `motion/react`.

For many applications, the migration is a package and import-path change. It is not a promise that every old version can be upgraded by renaming the dependency alone. Check the current major version and follow the upgrade notes for each major release between it and the target version.

## Learning outcomes

By the end of this lesson, you should be able to:

- Identify whether a project should migrate to the `motion` package.
- Replace the dependency and React import path safely.
- Separate the package rename from breaking changes between major versions.
- Find older APIs and update them deliberately.
- Validate build output, runtime behavior, and tests after migration.
- Keep the right import for code that runs inside the Framer platform.

## Identify the project type

For a regular React application, the current package is `motion`:

~~~bash
npm uninstall framer-motion
npm install motion
~~~

For Yarn or pnpm, use the equivalent remove and add commands:

~~~bash
yarn remove framer-motion
yarn add motion
~~~

~~~bash
pnpm remove framer-motion
pnpm add motion
~~~

Code Components and Overrides that run inside the Framer website builder have a different integration. Those Framer-specific files continue to import from `framer-motion` according to the Framer integration guidance. Do not apply the standard application migration mechanically to code that is authored for the Framer platform.

## Update React imports

Change React API imports from `framer-motion` to `motion/react`:

~~~jsx
// Before
import { AnimatePresence, motion } from "framer-motion";

// After
import { AnimatePresence, motion } from "motion/react";
~~~

Keep the imported APIs the same unless the upgrade guide for an intervening major version calls out a change. The same general import path applies to hooks and components such as `MotionConfig`, `LayoutGroup`, `useReducedMotion`, and `useMotionValue`.

If the project uses LazyMotion, update both relevant entry points:

~~~jsx
import { LazyMotion, domAnimation } from "motion/react";
import * as m from "motion/react-m";
~~~

For a React Server Component module, use the RSC entry point where appropriate:

~~~jsx
import * as motion from "motion/react-client";
~~~

Do not change unrelated JavaScript-only imports without checking their documented entry point. React APIs and vanilla JavaScript APIs can use different package subpaths.

## Migrate in controlled steps

### 1. Record the starting point

Before editing, record:

- The installed `framer-motion` version.
- The React version and rendering mode.
- The package manager and lockfile.
- The bundler, framework, and production build command.
- Any special imports, custom motion components, and styling-library wrappers.

Inspect the dependency tree and repository for `framer-motion` imports. Also inspect generated code, tests, examples, and configuration, not only files under `src`.

### 2. Read the required major-version upgrade notes

Start with the project's current major version and follow each major upgrade step in sequence. Do not skip intermediate major-version notes just because the package rename looks small.

Look for APIs the project actually uses. Upgrade notes describe possible breaking changes; they do not mean every app needs edits for every listed change.

### 3. Replace the dependency

Remove `framer-motion` and install `motion` using the project's package manager. Do not maintain both packages in a regular app unless there is an explicit compatibility reason. Duplicate copies can make dependency resolution and bundle analysis harder.

### 4. Change React import paths

Update React API imports to `motion/react`. Update LazyMotion imports to `motion/react-m` where used. Keep the Framer-specific import path in code that runs inside Framer's own Code Components or Overrides environment.

### 5. Update APIs only where the version guide requires it

Examples of older changes to check when migrating from earlier versions include:

- Replace removed `exitBeforeEnter` with `mode="wait"` on `AnimatePresence` when sequential entry is intended.
- Review old layout-transition props such as `positionTransition` and `layoutTransition`; modern layout animations use the `layout` prop with a transition.
- Replace old custom-component creation APIs with the documented `motion.create()` API where required by the source version.
- Review styled component prop forwarding if the project relies on an optional prop-validity integration that is no longer supplied automatically in a newer major version.
- Update tests that assumed animation updates happened synchronously if the source version's render scheduling changed.

These are version-dependent examples, not a universal list of edits. Check the APIs and versions present in the actual application.

### 6. Build and inspect

Run the project's existing type check, lint, test, and production build commands if they are part of its normal workflow. Resolve errors in the code rather than suppressing them broadly.

Inspect the production output for duplicate animation packages and confirm that the bundle contains the intended Motion entry points. An import rename can still produce an unexpected dependency graph if the lockfile or workspace configuration is stale.

### 7. Verify behavior

Compare the migrated app against the existing behavior:

- Initial and animate states.
- Variants and child orchestration.
- Exit animations and presence modes.
- Hover, focus, tap, drag, and in-view behavior used by the app.
- Layout and shared-layout transitions.
- Server rendering and hydration if applicable.
- Reduced-motion behavior.
- Styled component props and forwarded DOM attributes.
- Test timing and animation-frame assertions.

Use representative routes and interactions. A successful build does not by itself confirm animation behavior.

## Import and package checklist

| Project usage | Package or import path |
| --- | --- |
| Regular React application dependency | `motion` |
| Motion React components and hooks | `motion/react` |
| Lightweight `m` components | `motion/react-m` |
| Motion components in React Server Component modules | `motion/react-client` |
| Framer Code Components and Overrides | `framer-motion` according to the Framer integration |

Check the framework and installed package version before changing subpath imports. Do not copy an import path into an environment that does not support it.

## Lockfiles and workspaces

After changing the dependency:

- Commit the package manifest and the lockfile generated by the same package manager.
- Check every workspace package that imports Motion.
- Search for remaining `framer-motion` references and decide whether each belongs to a Framer platform integration or is an unconverted app import.
- Avoid manually editing lockfile records.
- Confirm CI and deployment use the same lockfile and install command.

If the project is a monorepo, the root dependency and workspace-level dependency policy should agree. Avoid accidentally installing both package names in different workspaces.

## Common migration mistakes

- Replacing the package but leaving imports from `framer-motion` in application files.
- Rewriting imports in Framer platform Code Components that still require `framer-motion`.
- Updating to the newest major without reading version notes for the source and intermediate majors.
- Changing deprecated APIs that the app does not use, creating unnecessary review work.
- Editing the manifest but forgetting the package lock.
- Running only a development server and skipping the production build.
- Assuming successful compilation means interactions, exits, layout, and reduced motion still behave correctly.
- Keeping both packages installed without a clear compatibility reason.
- Replacing failed imports with broad aliases that hide which package version the code uses.

## Rollback plan

Keep the migration in a small, reviewable commit. If the production build or important interaction checks fail, restore the prior manifest, lockfile, and imports together. Do not roll back only one of those pieces, because that can leave the dependency graph inconsistent.

## Practice

Choose a small React app using `framer-motion`. Record the installed version and production bundle baseline. Update the package and React import paths, then follow any required major-version notes. Build the app and compare its animations, layout behavior, reduced-motion support, and bundle output with the baseline.

Continue with [Performance and debugging](030-performance-and-debugging.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation</a></td><td><a href="../qnas/029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion Q&amp;A</a></td><td><a href="../examples/029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion example</a></td><td><a href="030-performance-and-debugging.md">030 - Performance and debugging</a></td></tr></tbody>
</table>

# Installation and project setup Q&A

## 1. Which package do these notes use?

New examples use the `motion` npm package. For React components and hooks, they import from `motion/react`. Older projects may use `framer-motion`, which is handled separately in the migration chapter.

## 2. What is the React import path?

Use:

~~~jsx
import { motion } from "motion/react";
~~~

The `motion/react` entry point exposes React-specific components and hooks. Do not confuse it with imports from the base `motion` package used for non-React APIs.

## 3. What React version should I use?

The current Motion for React installation guide requires React 18.2 or newer. Check the peer requirements for the specific Motion version selected by your project before upgrading.

## 4. Should I install Motion in a separate project?

Usually no. Add it to the React application where you want to use it. A separate sandbox is useful for learning examples, but production code should use the app's existing dependency tree and package manager.

## 5. What if I use pnpm or Yarn instead of npm?

Use the package manager that owns the repository's lockfile. For example, add Motion with that package manager rather than creating an npm lockfile beside a pnpm or Yarn lockfile.

## 6. Do I need to configure Vite?

A standard Vite React project normally needs no special Motion configuration. Install the package, import the React entry point, and render a motion component from the existing app.

## 7. Why does `npm ls motion react` help?

It shows whether Motion and React are installed in the dependency tree visible to the current project. Run it from the folder containing the intended `package.json`. It does not prove the application builds or that every runtime behavior is correct.

## 8. Do I need a separate type package for TypeScript?

The Motion package includes TypeScript declarations. Start with the installed package's types. Do not add an older third-party type package unless the official instructions for your chosen version require one.

## 9. Why does the import fail with a module-not-found error?

Check that the command ran in the correct project directory, that the package installation succeeded, and that the code imports from `motion/react`. If the package exists in a different workspace or package, install it where the React app can resolve it.

## 10. What should I do if the project has two lockfiles?

Do not delete one without checking the project configuration. Determine which package manager the project uses, keep its lockfile, and install Motion with that package manager. Then review the diff before committing.

## 11. Can I use a CDN in these examples?

The course is organized around React projects and package-manager installs. A CDN is not necessary for the React setup shown here. If you use a CDN for another environment, follow the instructions for that environment and pin a deliberate version rather than relying on an unreviewed floating version.

## 12. Can I import from `framer-motion` after installing `motion`?

No. Package names and import paths must match the dependency installed. New projects in these notes install `motion` and import from `motion/react`. Migrating an existing project also requires updating its dependency and lockfile consistently.

## 13. Does the repository's MIT license grant rights to Motion?

No. The license applies to the notes in this repository. Motion, React, and every other dependency have their own licenses and terms.

## 14. What is the safest installation workflow?

1. Confirm the React project already starts.
2. Use its existing package manager.
3. Install `motion`.
4. Import from `motion/react`.
5. Check the dependency tree.
6. Start the same development command and verify a simple component renders.

Continue with [motion components and props](../lessons/003-motion-components-and-props.md).

## Chapter navigation

<table width="100%">
  <thead>
    <tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr>
  </thead>
  <tbody>
    <tr><td><a href="001-motion-and-react-animation-fundamentals.md">001 - Motion and React animation fundamentals Q&amp;A</a></td><td><a href="../lessons/002-installation-and-project-setup.md">002 - Installation and project setup lesson</a></td><td><a href="../examples/002-installation-and-project-setup.md">002 - Installation and project setup example</a></td><td><a href="003-motion-components-and-props.md">003 - Motion components and props Q&amp;A</a></td></tr>
  </tbody>
</table>

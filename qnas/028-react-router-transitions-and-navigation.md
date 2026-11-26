# React Router transitions and navigation Q&A

## 1. What is a route transition?

It is a visual effect that accompanies a route content change. React Router still handles the URL, matching, history, and data; the transition layer animates the rendered change.

## 2. What is the current React Router approach for view transitions?

Current React Router guidance uses React's `ViewTransition` component around a persistent `Outlet`. React and React DOM 19.3 or later are required for this component.

## 3. Should I key the shared ViewTransition boundary by the current location?

No. Keep the boundary mounted across navigations. React Router updates the outlet inside it, and React coordinates the transition.

## 4. Should the navigation bar be inside the animated region?

Only if the navigation itself should transition. Usually keep it outside the boundary and wrap the main outlet content.

## 5. What is the React Router Outlet?

`Outlet` renders the matching child route of its parent route, or nothing when no child route matches. It is a natural content region to wrap in a persistent view transition.

## 6. Do route links still work when transitions are enabled?

Yes. Use router links and forms as usual. The transition enhances the visual update; it should not replace routing or change the destination.

## 7. Which React Router navigations can activate the transition?

The current integration applies to navigation from `Link`, `NavLink`, `Form`, `useNavigate`, and `useSubmit` when the router's React Transition integration remains enabled.

## 8. Does React Router require an extra startTransition call for navigation?

No, for the current supported router navigation APIs in this integration. React Router coordinates its navigation updates. Custom state updates outside that flow have their own transition requirements.

## 9. What happens if I disable router transitions?

React Router's View Transition boundary may not activate for navigation updates. Keep the router's transition integration enabled when relying on this behavior.

## 10. What happens in a browser without View Transitions support?

Navigation still works, but the browser transition animation is not shown. Treat the effect as progressive enhancement.

## 11. Will every Back or Forward navigation animate?

No. React's View Transition integration can skip transitions for synchronous updates caused by browser `popstate`. Back and Forward must still work correctly when no animation runs.

## 12. How do I respect reduced motion?

Use `@media (prefers-reduced-motion: reduce)` to disable or substantially shorten the view-transition pseudo-element animations. React does not automatically disable these transitions for the user.

## 13. Are React Router's older viewTransition props still recommended?

Current React Router docs deprecate the old link/form `viewTransition` prop, the navigation option, and related state APIs in favor of React's `ViewTransition` component. Check the installed router version when maintaining an older application.

## 14. Can I use Motion AnimatePresence for route transitions?

Yes. It can retain a removed route child while its Motion `exit` animation runs. Use it when Motion-specific exit choreography is required, and keep the presence boundary mounted above the child that changes.

## 15. What does AnimatePresence need to detect a route change?

Its direct child needs a stable key that changes when the route identity should change. The key lets the presence boundary distinguish the exiting child from the entering child.

## 16. Should I key the outlet by pathname or location.key?

Choose based on the intended trigger. `location.pathname` changes for path changes but not for query-only or hash-only changes. `location.key` identifies a history location and can animate more navigation entries.

## 17. Does mode="wait" work for multiple route children?

No. The `wait` mode supports one child at a time. It lets the entering child wait for the exiting child, which can make a route feel slower if the exit is long.

## 18. Why might a Motion route exit not play?

Check that `AnimatePresence` remains mounted, the direct child has a unique stable key, and the exiting child defines an `exit` target. Also check that a parent boundary is not removed at the same time.

## 19. Can an animated Outlet pattern fail with nested routing?

It can behave differently depending on how the router creates and captures the outlet element. Test the actual nested routes, route context, loaders, error boundaries, and pending states in the installed router setup.

## 20. Can I combine React ViewTransition and Motion AnimatePresence?

They can both animate a route update, but applying both to the same route region can create overlapping or competing effects. Choose one route-level transition owner unless the separate roles are intentional and tested.

## 21. Is a route fade a shared-element transition?

No. A fade animates the route snapshot or route child. A shared-element transition deliberately matches an element on both routes, such as a card image and its detail image, using a unique shared identity.

## 22. Should route content wait for the exit animation before navigation?

Usually no. Avoid long decorative exits that delay access to the destination. Choose a short transition that keeps navigation responsive and preserves loading or error feedback.

## 23. What should I use for in-page animations during route transitions?

Keep them separate from the route transition. Motion can animate controls or content within a page, while React Router continues to own the navigation update. Avoid animating the same content with two transition systems.

## 24. What should I test before shipping?

- Router links, forms, and programmatic navigation.
- Direct loads and refreshes on nested URLs.
- Browser Back and Forward when a transition does and does not run.
- Route loading, error, and not-found states.
- Focus, keyboard navigation, and scroll restoration.
- Reduced-motion preference and unsupported-browser behavior.
- Unique keys, exit timing, and nested route context if using `AnimatePresence`.

## 25. What should I practice?

Build two routes under a shared layout. Try React's persistent `ViewTransition` around `Outlet`, then build a separate Motion `AnimatePresence` version. Compare behavior and keep only the route-level transition approach that fits the application.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components Q&amp;A</a></td><td><a href="../lessons/028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation lesson</a></td><td><a href="../examples/028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation example</a></td><td><a href="029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion Q&amp;A</a></td></tr></tbody>
</table>

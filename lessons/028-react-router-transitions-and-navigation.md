# React Router transitions and navigation

A route transition animates the visible change between two matched pages. The router still owns the URL, history, route matching, data loading, and navigation state. Animation should follow that navigation instead of replacing it with a separate routing system.

There are two useful approaches:

- React Router's current integration uses React's `<ViewTransition>` around a persistent `<Outlet>`. It animates navigation updates with the browser View Transitions API.
- Motion's `AnimatePresence` can keep an exiting route element mounted while its Motion `exit` animation runs. This gives direct control over Motion animation values, but route keys and nested routing need care.

Choose one owner for the route-level transition. Using two systems to animate the same route change can cause duplicate or competing effects.

## Learning outcomes

By the end of this lesson, you should be able to:

- Keep navigation and route matching under React Router's control.
- Use a persistent React `ViewTransition` boundary around a router `Outlet`.
- Style route transitions and respect reduced-motion preferences.
- Explain when Motion `AnimatePresence` is useful for a custom route animation.
- Choose a stable route key and keep an exit boundary mounted.
- Preserve browser history, focus, scroll, loading, and direct-link behavior.

## Navigation and animation are separate jobs

React Router changes the matched route in response to a link, form, or programmatic navigation. The route tree provides the new page, often through `Outlet`.

A transition system animates what the user sees as that route content changes. It should not prevent links from working, interfere with the browser URL, or hide route loading and error states.

Use router navigation primitives such as `Link` and `NavLink` instead of plain anchors for in-app navigation. A link still represents a real destination, supports opening in a new tab, and preserves expected keyboard behavior.

## Current React Router view transitions

Current React Router guidance uses React's `ViewTransition` component. The feature requires React and React DOM 19.3 or later. Keep the boundary mounted in a shared layout and wrap the outlet:

~~~jsx
import { ViewTransition } from "react";
import { Link, Outlet } from "react-router";

export default function AppLayout() {
  return (
    <>
      <nav aria-label="Main navigation">
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>

      <ViewTransition default="route-fade">
        <main>
          <Outlet />
        </main>
      </ViewTransition>
    </>
  );
}
~~~

Keep the navigation outside the animated region if it should remain stationary. Do not add a location key to the `ViewTransition` boundary or remount the shared layout on every route. The boundary should remain in the tree while the outlet content changes.

The router coordinates navigations from `Link`, `NavLink`, `Form`, `useNavigate`, and `useSubmit` when its React Transition integration is enabled. Do not disable router transitions if using this integration.

## Customize the cross-fade

A view transition class selects CSS for the route snapshot:

~~~css
::view-transition-old(.route-fade) {
  animation: route-out 140ms ease-in both;
}

::view-transition-new(.route-fade) {
  animation: route-in 200ms ease-out both;
}

@keyframes route-out {
  to {
    opacity: 0;
  }
}

@keyframes route-in {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(.route-fade),
  ::view-transition-new(.route-fade) {
    animation: none !important;
  }
}
~~~

The default browser transition is a cross-fade. A CSS view-transition class can change its timing and appearance. The router can still navigate normally in browsers that do not support the View Transitions API; the animation is an enhancement.

React View Transitions are activated by updates marked as React Transitions. React Router's default transition integration enables this for navigation. Synchronous updates such as a direct browser `popstate` update may not animate. Back and Forward navigation must still work even when no transition plays.

## Respect reduced motion

React does not automatically disable View Transition animations when the user prefers reduced motion. Use the CSS media query to disable or substantially reduce the route snapshot animation.

Also keep route content understandable without motion. A route transition should not delay access to the destination, make the page flash, or communicate information only through movement.

## Existing React Router viewTransition options

Older React Router integrations provided `viewTransition` props on links and forms, a `viewTransition: true` navigation option, and related state hooks. Current React Router docs deprecate these APIs in favor of React's `ViewTransition` component. Existing applications can continue to use the legacy APIs where their installed versions require them, but new code should follow the current router and React version guidance.

Do not enable both the router's legacy View Transition wrapping and React's built-in View Transition for the same navigation. Both can call the browser transition mechanism and interrupt one another.

## Motion AnimatePresence for custom route choreography

Use Motion `AnimatePresence` when the route change needs Motion-specific enter and exit values rather than the browser snapshot transition. The direct child of `AnimatePresence` needs a unique, stable key. The presence boundary must stay mounted while the child is removed so Motion can finish its `exit` animation.

A common outlet pattern looks like this:

~~~jsx
import { AnimatePresence, motion } from "motion/react";
import { useLocation, useOutlet } from "react-router";

function AnimatedOutlet() {
  const location = useLocation();
  const outlet = useOutlet();

  return (
    <AnimatePresence mode="wait" initial={false}>
      <motion.div
        key={location.pathname}
        className="route-content"
        initial={{ opacity: 0, x: 16 }}
        animate={{ opacity: 1, x: 0 }}
        exit={{ opacity: 0, x: -16 }}
        transition={{ duration: 0.2, ease: "easeInOut" }}
      >
        {outlet}
      </motion.div>
    </AnimatePresence>
  );
}
~~~

Use this pattern only when a custom Motion exit is needed. Confirm that the captured outlet continues to render under its route context in the router setup being used. Test nested routes and loader transitions, not just two simple pages.

`mode="wait"` sequences the new child after the exiting child finishes and supports one child at a time. It can make navigation feel slower if exits are long or route data is still loading. The default `sync` mode enters and exits immediately, so overlapping content may need layout or positioning work.

Choose a key based on what should count as a new page:

- `location.pathname` animates when the path changes, but not when only the query or hash changes.
- `location.key` treats each history location as a separate entry and may animate more navigation updates.

Avoid using an array index or a random value as a route key. A changing random key causes unnecessary remounts, while an index can identify the wrong page.

## Keep Motion and router responsibilities clear

- Let React Router choose the route and update the URL.
- Let `AnimatePresence` control the lifetime of a Motion child only when its exit needs to finish.
- Keep shared navigation outside the route content boundary unless it should transition too.
- Preserve route error, pending, and not-found states.
- Avoid changing focus or scroll position solely to make an animation look better.
- Do not start a route transition from a hover event. Navigation should remain tied to a real link, form, or explicit action.
- Do not combine a full-page `ViewTransition` and full-page `AnimatePresence` transition without a specific design and tests.

## Motion values for nested and shared content

A route-level fade is not automatically a shared-element transition. If the same image or card should visually connect a list page to a detail page, use a deliberate shared-element design, give matched elements stable unique identities, and test the layout and fallback behavior. Avoid assigning one shared name to multiple simultaneous elements.

For most navigation, a simple short cross-fade is easier to follow than a large directional slide. Use direction-aware transitions only when the relationship between destinations is clear and the direction is available from reliable navigation state.

## Common mistakes

- Keying or unmounting the router layout itself, which prevents a stable exit boundary.
- Putting `AnimatePresence` inside the condition that removes it.
- Omitting a key or using an unstable key for the direct presence child.
- Assuming `mode="wait"` supports multiple simultaneous children.
- Animating only a pathname while expecting query-only navigation to trigger the same keyed remount.
- Using router APIs that are deprecated in the current React Router version without checking the installed version.
- Disabling React Router's Transition integration while expecting its View Transition boundary to run.
- Forgetting the reduced-motion media query.
- Making navigation wait for a long decorative exit.
- Treating the transition as a substitute for route loading UI, focus management, or scroll restoration.

## Practice

Create Home and About routes with a shared layout. First wrap the persistent outlet with React's `ViewTransition` and style a brief cross-fade. Check link clicks, browser Back and Forward, direct URL loading, reduced motion, and an unsupported-browser fallback.

Then create a separate Motion-based version with `AnimatePresence` and a keyed outlet child. Compare the two approaches, including route loading and exit timing. Keep one route-level transition owner in the final version.

Continue with [Migrating from framer-motion to Motion](029-migrating-from-framer-motion-to-motion.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components</a></td><td><a href="../qnas/028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation Q&amp;A</a></td><td><a href="../examples/028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation example</a></td><td><a href="029-migrating-from-framer-motion-to-motion.md">029 - Migrating from framer-motion to Motion</a></td></tr></tbody>
</table>

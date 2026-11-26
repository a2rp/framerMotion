# Framer Motion Keywords and Concepts

A searchable index of Motion for React APIs and animation terms used in this course. These are library APIs and concepts, not JavaScript reserved words. Current examples use the `motion` package and import React APIs from `motion/react`. Follow the linked lesson for full explanations and runnable patterns.

## Package and component basics

| Term | Meaning | Lesson |
| --- | --- | --- |
| Motion for React | The current name for the React animation library formerly known as Framer Motion. | [Chapter 001](lessons/001-motion-and-react-animation-fundamentals.md) |
| `motion` package | Current npm package used by new examples in these notes. Install it with `npm install motion`. | [Chapter 002](lessons/002-installation-and-project-setup.md) |
| `motion/react` | React entry point for Motion components, hooks, and related APIs. | [Chapter 002](lessons/002-installation-and-project-setup.md) |
| `framer-motion` | Package and import path used by older projects. See the migration lesson before changing an existing dependency. | [Chapter 029](lessons/029-migrating-from-framer-motion-to-motion.md) |
| `motion.div` | A motion-enabled HTML element. Motion also provides components for other HTML and SVG tags. | [Chapter 003](lessons/003-motion-components-and-props.md) |
| Motion component | A React component that accepts animation, gesture, viewport, and layout props while rendering an HTML or SVG element. | [Chapter 003](lessons/003-motion-components-and-props.md) |
| Motion prop | A prop such as `animate`, `whileHover`, or `layout` that adds Motion behavior to a motion component. | [Chapter 003](lessons/003-motion-components-and-props.md) |
| `initial` | The visual target used before an element begins its animation. Set it to `false` to render directly at the animate state. | [Chapter 004](lessons/004-initial-and-animate-states.md) |
| `animate` | A target object, variant label, or supported animation control that describes the active visual state. | [Chapter 004](lessons/004-initial-and-animate-states.md) |
| `exit` | A target or variant label played when a child leaves an `AnimatePresence` boundary. | [Chapter 011](lessons/011-animatepresence-and-exit-animations.md) |

## Transitions and target values

| Term | Meaning | Lesson |
| --- | --- | --- |
| Transition | Configuration that controls how an animated value changes, including type, duration, easing, delay, and spring parameters. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| Tween | A time-based animation controlled by duration and easing. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| Spring | A physics-inspired transition controlled by parameters such as stiffness, damping, and mass, or by a visual duration and bounce configuration supported by the installed version. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| `duration` | Time-based transition length in seconds. Spring behavior depends on its spring configuration. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| `ease` | A curve that changes the rate of progress over a tween. It can be a named easing or a supported easing definition. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| `stiffness` | Spring parameter that influences how strongly the spring moves toward its target. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| `damping` | Spring parameter that reduces oscillation. More damping generally settles motion sooner. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| `mass` | Spring parameter that changes how heavy the animated value feels. | [Chapter 005](lessons/005-transitions-and-springs.md) |
| Keyframes | An ordered sequence of target values for one animated property. | [Chapter 006](lessons/006-keyframes-and-repeats.md) |
| `repeat` | Number of additional animation iterations. Use a deliberate finite count or an intentional infinite loop. | [Chapter 006](lessons/006-keyframes-and-repeats.md) |
| `repeatType` | Controls whether repeated playback restarts, reverses, or mirrors according to the current API. | [Chapter 006](lessons/006-keyframes-and-repeats.md) |
| `repeatDelay` | Delay between iterations of a repeating animation. | [Chapter 006](lessons/006-keyframes-and-repeats.md) |
| Independent transforms | Motion values such as `x`, `y`, `scale`, and `rotate` that can be animated independently and composed into a transform. | [Chapter 003](lessons/003-motion-components-and-props.md) |
| Complex value | A supported CSS value containing multiple numeric parts, such as a shadow or transform-like string. Interpolation depends on compatible value structure. | [Chapter 021](lessons/021-css-variables-and-complex-values.md) |
| CSS custom property | A variable written with a `--name` property and read with `var(--name)`. Motion can animate supported custom properties. | [Chapter 021](lessons/021-css-variables-and-complex-values.md) |

## Variants and orchestration

| Term | Meaning | Lesson |
| --- | --- | --- |
| Variant | A named animation target in a `variants` object, for example `hidden` or `visible`. | [Chapter 007](lessons/007-variants-and-reusable-states.md) |
| `variants` | Object mapping labels to animation targets. Labels can be used by `initial`, `animate`, `exit`, and gesture or viewport props. | [Chapter 007](lessons/007-variants-and-reusable-states.md) |
| Variant propagation | A parent variant label can coordinate descendant motion components that define matching variant labels. | [Chapter 007](lessons/007-variants-and-reusable-states.md) |
| `custom` | Data passed to dynamic variants or presence-aware descendants when a static target is not enough. | [Chapter 012](lessons/012-presence-modes-and-dynamic-exit-data.md) |
| `delayChildren` | Variant transition option that delays child animations. It can be combined with a stagger function. | [Chapter 008](lessons/008-variant-orchestration-and-stagger.md) |
| `stagger` | Helper that generates offsets so children begin at different times. | [Chapter 008](lessons/008-variant-orchestration-and-stagger.md) |
| `when` | Variant transition option for ordering parent and child animation phases. | [Chapter 008](lessons/008-variant-orchestration-and-stagger.md) |

## Gestures and viewport

| Term | Meaning | Lesson |
| --- | --- | --- |
| `whileHover` | Target or variant label active while a pointer is hovering over the element. | [Chapter 009](lessons/009-hover-tap-and-focus-gestures.md) |
| `whileTap` | Target or variant label active during a press interaction. | [Chapter 009](lessons/009-hover-tap-and-focus-gestures.md) |
| `whileFocus` | Target or variant label active while the element has keyboard or programmatic focus. | [Chapter 009](lessons/009-hover-tap-and-focus-gestures.md) |
| Hover callback | Callbacks such as `onHoverStart` and `onHoverEnd` for responding to hover state changes. | [Chapter 009](lessons/009-hover-tap-and-focus-gestures.md) |
| Pan gesture | Pointer movement tracking that reports an origin and movement information without necessarily moving the element. | [Chapter 010](lessons/010-pan-drag-and-constraints.md) |
| `drag` | Enables dragging on a motion component. It can be restricted to `"x"`, `"y"`, or both axes. | [Chapter 010](lessons/010-pan-drag-and-constraints.md) |
| `dragConstraints` | Bounds dragging with explicit pixel limits or the measured bounds of a referenced container. | [Chapter 010](lessons/010-pan-drag-and-constraints.md) |
| `dragElastic` | Controls how far a dragged element can move beyond its constraints. | [Chapter 010](lessons/010-pan-drag-and-constraints.md) |
| `dragMomentum` | Controls whether a drag continues with momentum after release. | [Chapter 010](lessons/010-pan-drag-and-constraints.md) |
| `whileInView` | Target or variant label active while the component is in the observed viewport. | [Chapter 018](lessons/018-in-view-animations-and-useinview.md) |
| `viewport` | Options for viewport tracking, including root, margin, once, and amount where supported by the installed version. | [Chapter 018](lessons/018-in-view-animations-and-useinview.md) |
| `useInView` | Hook that reports whether an element attached to a ref is currently in view. | [Chapter 018](lessons/018-in-view-animations-and-useinview.md) |

## Presence and layout

| Term | Meaning | Lesson |
| --- | --- | --- |
| `AnimatePresence` | Keeps removed React children available long enough for their `exit` animations to finish. | [Chapter 011](lessons/011-animatepresence-and-exit-animations.md) |
| Presence mode | Policy for coordinating entering and exiting children, such as the documented `"sync"`, `"wait"`, or `"popLayout"` modes. Check mode constraints in the installed version. | [Chapter 012](lessons/012-presence-modes-and-dynamic-exit-data.md) |
| `useIsPresent` | Hook that reports whether a component is still present within an `AnimatePresence` boundary. | [Chapter 012](lessons/012-presence-modes-and-dynamic-exit-data.md) |
| `usePresenceData` | Reads custom data supplied by the nearest presence boundary, useful when an exiting child needs updated direction or state. | [Chapter 012](lessons/012-presence-modes-and-dynamic-exit-data.md) |
| `layout` | Enables layout animation when React changes an element's measured size or position. | [Chapter 013](lessons/013-layout-animations.md) |
| `layout="position"` | Limits layout animation to position changes when scaling content would be undesirable. | [Chapter 013](lessons/013-layout-animations.md) |
| `layoutId` | Matches elements across different parts of a React tree so Motion can animate a shared visual between them. | [Chapter 014](lessons/014-shared-layout-and-layout-groups.md) |
| `LayoutGroup` | Coordinates layout measurements and animations among related motion components. | [Chapter 014](lessons/014-shared-layout-and-layout-groups.md) |

## Motion values and imperative animation

| Term | Meaning | Lesson |
| --- | --- | --- |
| Motion value | A Motion-managed value that updates an animated property without making React render on every frame. | [Chapter 015](lessons/015-motion-values-and-direct-updates.md) |
| `useMotionValue` | Creates a Motion value in a React component. | [Chapter 015](lessons/015-motion-values-and-direct-updates.md) |
| `useTransform` | Derives a value by mapping one or more source values to a new range or by running a transformation function. | [Chapter 016](lessons/016-transforming-and-composing-motion-values.md) |
| `useSpring` | Creates or applies a spring-following Motion value. | [Chapter 016](lessons/016-transforming-and-composing-motion-values.md) |
| `useVelocity` | Derives the velocity of a Motion value for effects that depend on direction or speed. | [Chapter 016](lessons/016-transforming-and-composing-motion-values.md) |
| `useMotionValueEvent` | Subscribes to Motion value events from a React component with lifecycle-managed cleanup. | [Chapter 016](lessons/016-transforming-and-composing-motion-values.md) |
| `useScroll` | Returns scroll-position and progress Motion values for the viewport or a configured container and target. | [Chapter 017](lessons/017-scroll-linked-animations-with-usescroll.md) |
| `scrollYProgress` | Normalized vertical scroll progress, generally represented from 0 to 1 for its configured offsets. | [Chapter 017](lessons/017-scroll-linked-animations-with-usescroll.md) |
| `offset` | Defines the intersection points used to measure progress between a scroll target and its container. | [Chapter 017](lessons/017-scroll-linked-animations-with-usescroll.md) |
| `useAnimate` | Provides a scoped ref and imperative animate function for sequences and event-driven animation. It cleans up animations when the calling component unmounts. | [Chapter 019](lessons/019-imperative-sequences-with-useanimate.md) |
| Animation controls | Controls returned by imperative animation APIs for operations such as stopping or seeking supported animations. | [Chapter 019](lessons/019-imperative-sequences-with-useanimate.md) |

## SVG, composition, and configuration

| Term | Meaning | Lesson |
| --- | --- | --- |
| SVG motion component | Motion-enabled SVG element, such as `motion.path` or `motion.circle`. | [Chapter 020](lessons/020-svg-and-path-animations.md) |
| `pathLength` | Motion value that can reveal an SVG path progressively when used with a dashed stroke. | [Chapter 020](lessons/020-svg-and-path-animations.md) |
| `motion.create()` | Creates a motion-enabled custom component. Define it outside render and ensure the ref and style reach the underlying HTML or SVG element. | [Chapter 022](lessons/022-custom-react-components-with-motion-create.md) |
| `forwardRef` | React 18 pattern for forwarding a ref from a custom component to the DOM element Motion needs to measure and animate. | [Chapter 022](lessons/022-custom-react-components-with-motion-create.md) |
| `MotionConfig` | Provides shared Motion configuration such as a default transition or reduced-motion policy to descendant motion components. | [Chapter 023](lessons/023-global-configuration-with-motionconfig.md) |
| `reducedMotion` | MotionConfig policy for respecting the user's reduced-motion preference, enforcing reduction, or leaving it disabled. | [Chapter 024](lessons/024-accessible-and-reduced-motion.md) |
| `useReducedMotion` | Hook that reports the user's reduced-motion preference so an interface can adapt motion intentionally. | [Chapter 024](lessons/024-accessible-and-reduced-motion.md) |
| `LazyMotion` | Loads Motion features in a controlled way to reduce the initial bundle used by motion components. | [Chapter 026](lessons/026-bundle-optimization-with-lazymotion.md) |
| `m` | Lightweight component factory used with `LazyMotion`. Load only feature bundles that the interface needs. | [Chapter 026](lessons/026-bundle-optimization-with-lazymotion.md) |
| `motion/react-client` | React Server Components entry point for using supported motion components in frameworks such as Next.js. | [Chapter 027](lessons/027-nextjs-server-rendering-and-react-server-components.md) |
| Client component boundary | In a React Server Components app, interactive hooks and client-only behavior belong in a client component. | [Chapter 027](lessons/027-nextjs-server-rendering-and-react-server-components.md) |
| Stable key | A React list key that preserves component identity, required for predictable list and presence animations. | [Chapter 028](lessons/028-react-router-transitions-and-navigation.md) |
| Reduced motion | A user preference exposed by the operating system and browser that asks interfaces to limit nonessential movement. | [Chapter 024](lessons/024-accessible-and-reduced-motion.md) |

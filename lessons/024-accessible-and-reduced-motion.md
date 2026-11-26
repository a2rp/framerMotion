# Accessible and reduced motion

Motion should support the task and respect the visitor's preferences. Large or repeated movement can distract, make content harder to follow, or cause discomfort for people sensitive to motion. A useful reduced-motion experience keeps the interface understandable while removing or changing unnecessary movement.

Motion offers a global policy through `MotionConfig` and a component-level hook through `useReducedMotion`. These help with Motion animations; CSS animation, video, canvas, and other libraries need their own behavior.

## Set a global policy

Use `reducedMotion="user"` to respect the operating-system preference:

~~~jsx
import { MotionConfig } from "motion/react";

<MotionConfig reducedMotion="user">
  <App />
</MotionConfig>
~~~

When reduced motion is enabled, Motion disables transform and layout animations. Other values such as opacity and background color can still animate. This provides a baseline, but it is not a guarantee that every visual effect becomes static.

MotionConfig also accepts `"always"` to force its reduced-motion behavior and `"never"` to ignore the preference. The default is `"never"`. Use always for inspection or when it is the intended product policy; avoid overriding a visitor's setting with never without a clear reason.

## Adapt a specific interaction

Use `useReducedMotion` when an interaction needs a custom alternative:

~~~jsx
import { motion, useReducedMotion } from "motion/react";

function Panel({ open }) {
  const shouldReduceMotion = useReducedMotion();

  return (
    <motion.aside
      animate={
        open
          ? { opacity: 1, x: 0 }
          : shouldReduceMotion
            ? { opacity: 0 }
            : { opacity: 0, x: "-100%" }
      }
    />
  );
}
~~~

The hook returns whether the device preference requests reduced motion and updates the component when that preference changes. Use it to replace a spatial transition with a fade, remove parallax, stop decorative auto-play, or shorten a custom sequence.

Avoid an API that has too many nested conditional targets. A small helper or two named target objects can make full-motion and reduced-motion states easier to compare.

## Reduce motion while preserving feedback

Reduced motion does not mean every state change must be silent or instantaneous. Choose an alternative that still explains what changed:

| Full-motion pattern | Reduced-motion alternative |
| --- | --- |
| Large slide from offscreen | Fade in place or update instantly |
| Parallax tied to scrolling | Static positioning |
| Zoom or large scale pulse | No scale change, or a subtle color/opacity cue |
| Long decorative path draw | Show the complete graphic |
| Auto-playing background video | Pause or disable auto-play |
| Repeated looping movement | Stop the loop |

Keep the same final state and meaning. If a menu opens, users should still know it opened; do not make important content depend on a large spatial animation. A modest opacity change may remain under Motion's automatic policy, so use hook logic if it should also become instant.

## Content, semantics, and focus

- Keep information available in the document regardless of animation progress.
- Use semantic buttons and links for controls; animation does not add keyboard behavior.
- Preserve visible focus indicators and do not move focus as a visual side effect.
- Use `aria-expanded`, `aria-pressed`, or status semantics for the underlying state.
- Avoid announcing continuous frame updates through a live region.
- Do not rely on color or motion alone to communicate a state.
- Keep hit targets and layout stable when a transform is reduced.

When opening a dialog or menu, manage focus according to the interaction pattern. Motion can animate the panel, but it does not perform focus management or create dialog semantics for you.

## Motion outside Motion components

`MotionConfig` affects Motion components in its subtree. For CSS effects, respond to the media query separately:

~~~css
@media (prefers-reduced-motion: reduce) {
  .decorative-parallax {
    transform: none;
  }

  .decorative-loop {
    animation: none;
  }
}
~~~

Also review auto-playing video, canvas animation, animated images, and third-party components. Use a single preference source where practical so the site's behavior remains consistent.

## Test both modes

A useful review checks:

1. The normal experience with motion enabled.
2. The operating-system reduced-motion preference with `reducedMotion="user"`.
3. Any custom `useReducedMotion` paths.
4. CSS media-query behavior and non-Motion animations.
5. Keyboard access, visible focus, and unchanged content meaning.
6. State changes after the preference is changed while the page is open.

During development, `reducedMotion="always"` can help inspect the reduced behavior. Restore the user policy before shipping unless always-reduced behavior is intentional.

## Troubleshooting

1. **Transforms continue to animate:** confirm the MotionConfig boundary and user policy; inspect CSS or imperative animations outside Motion.
2. **Opacity still changes:** this is expected under Motion's automatic reduced-motion behavior; make it conditional if needed.
3. **Parallax remains active:** use `useReducedMotion` to remove its derived transform or render a static value.
4. **A control loses feedback:** preserve a state cue such as text, color, or a concise fade.
5. **Content is absent until an animation ends:** make it part of normal document flow and do not gate meaning on motion.
6. **Focus moves unexpectedly:** remove animation-driven focus changes and handle focus from the interaction itself.
7. **The preference seems stale:** test the hook's update behavior and inspect the OS preference.
8. **A CSS animation ignores MotionConfig:** add a `prefers-reduced-motion` CSS rule.

## Recap and practice

Set a global user policy with MotionConfig, then use `useReducedMotion` where an interaction needs a tailored alternative. Motion automatically suppresses transform and layout animation, but opacity and effects outside Motion may continue. Preserve meaning, keyboard operation, and focus in both modes.

Practice by replacing a large offscreen slide with a fade or instant state for reduced motion. Then inspect CSS effects, looping media, focus, and state cues under the system preference.

Continue with [responsive motion patterns](025-responsive-motion-patterns.md).

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig</a></td><td><a href="../qnas/024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion Q&amp;A</a></td><td><a href="../examples/024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion example</a></td><td><a href="025-responsive-motion-patterns.md">025 - Responsive motion patterns</a></td></tr></tbody>
</table>

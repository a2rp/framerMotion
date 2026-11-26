# Accessible and reduced motion Q&A

## 1. What is reduced motion?

It is a device preference indicating that an interface should reduce certain kinds of motion. It can improve comfort and usability for people sensitive to movement.

## 2. How do I respect the preference across Motion components?

Wrap the relevant application subtree in `<MotionConfig reducedMotion="user">`.

## 3. What does Motion disable under reducedMotion user?

It disables transform and layout animations. Other values, such as opacity and background color, can continue.

## 4. Does reduced motion make every animation static?

No. Motion's automatic behavior does not disable every property or external animation system. Use custom logic for effects that need a different alternative.

## 5. What does useReducedMotion return?

It returns whether the device preference requests reduced motion and updates the component when the preference changes.

## 6. When should I use useReducedMotion?

Use it when a particular interaction needs a tailored state, such as replacing a slide with a fade or disabling parallax or video auto-play.

## 7. What is a good alternative to a large slide?

Fade in place, provide an immediate state change, or use another small cue that keeps the final state clear.

## 8. Should opacity always remain animated?

Not necessarily. Motion can preserve opacity under its automatic policy. Make it instant or static with `useReducedMotion` if that better suits the interaction.

## 9. Should reduced motion remove all feedback?

No. Preserve useful feedback using a concise text, color, or static state change. The goal is to reduce unnecessary movement without losing meaning.

## 10. Does MotionConfig affect CSS animations?

No. Add a `prefers-reduced-motion` media query for CSS effects and review other animation systems separately.

## 11. What should happen to parallax?

Usually remove or greatly reduce it when reduced motion is requested. Use a static position instead of a scroll-linked transform.

## 12. What about auto-playing video?

Use the preference to disable or pause nonessential auto-playing background video. Provide user controls when playback is a meaningful feature.

## 13. Should the focus move after an animated panel opens?

Only if the interaction semantics require it. Motion itself should not move focus; implement focus management as part of the dialog or menu behavior.

## 14. Does an animated control become keyboard accessible automatically?

No. Use semantic buttons and links, keyboard behavior, visible focus, and appropriate state attributes.

## 15. Should a live region announce animation progress?

Usually not. Avoid announcing every frame. Announce only meaningful discrete state changes when assistive technology users need them.

## 16. What does reducedMotion="always" do?

It forces Motion's reduced-motion behavior, useful for inspecting that mode. It is not necessarily the right production policy for every app.

## 17. What does reducedMotion="never" do?

It ignores the device preference. It is the default, but overriding a visitor's preference should require a clear product reason.

## 18. How do I test reduced motion?

Enable the operating-system preference with `reducedMotion="user"`, inspect custom hook behavior, CSS media queries, video, focus, and content meaning.

## 19. What should happen if the preference changes while the page is open?

Custom logic using `useReducedMotion` should respond to the updated preference and rerender with the adapted animation state.

## 20. Does reduced motion change content order or availability?

It should not. Keep content and controls available independently of animation and preserve the same final interface state.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="023-global-configuration-with-motionconfig.md">023 - Global configuration with MotionConfig Q&amp;A</a></td><td><a href="../lessons/024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion lesson</a></td><td><a href="../examples/024-accessible-and-reduced-motion.md">024 - Accessible and reduced motion example</a></td><td><a href="025-responsive-motion-patterns.md">025 - Responsive motion patterns Q&amp;A</a></td></tr></tbody>
</table>

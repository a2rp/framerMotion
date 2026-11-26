# Capstone: accessible animated interface

This capstone combines Motion, React state, semantic HTML, keyboard behavior, and a reduced-motion strategy in one small interface. The goal is a useful animated disclosure that remains understandable when motion is removed.

The interface contains a page heading, a button that reveals a short guide, and three static steps. The guide uses a brief entrance and exit transition for people who have not requested reduced motion. It does not move the page, trap focus, autoplay, or make essential information available only through animation.

## Learning outcomes

By completing the capstone, you should be able to:

- Turn an interaction requirement into a semantic React component.
- Keep a disclosure's accessible state synchronized with its visible state.
- Use AnimatePresence for a conditional exit without making exiting content available to assistive technology.
- Respect the system reduced-motion preference and remove non-essential movement.
- Preserve keyboard access, visible focus, readable content, and responsive layout.
- Review an animated feature against observable acceptance criteria.

## Capstone brief

Build a responsive resource page with a "Quick guide" disclosure. The user can open or close the guide with a button. The guide gives three concise steps that help someone use the page. A restrained opacity and vertical transition can communicate that the guide has appeared, but the content and control must work without that transition.

Use these requirements as the product contract:

1. The page has one descriptive level-one heading.
2. A native button opens and closes the guide.
3. The button's accessible name describes the next action.
4. The button exposes the correct aria-expanded state.
5. The button's aria-controls value points to a stable guide container.
6. The guide has a heading and readable content.
7. Escape closes the guide when the disclosure button has focus.
8. Keyboard focus stays on the disclosure button after it toggles.
9. The exiting guide is hidden from the accessibility tree while it leaves visually.
10. The system reduced-motion preference removes position travel and makes the transition immediate.
11. The page remains usable at narrow widths and browser zoom.
12. Motion is decorative. The control and content do not depend on animation to work.

## Plan the motion before coding

Describe each state in words first:

| State | User-visible content | Motion policy |
| --- | --- | --- |
| Closed | The page heading, disclosure button, and resource steps are available | The guide is not visible |
| Opening with normal motion | The guide becomes visible below its button | A short fade and small vertical offset |
| Open | The guide remains visible and readable | No looping or repeated movement |
| Closing with normal motion | The guide fades and moves a small distance | It is removed after its exit transition |
| Opening or closing with reduced motion | The same content appears or disappears | No position change and no meaningful wait |

A transition should clarify a state change. It should not hide content, delay the button, or keep the interface moving after the user has finished the action.

## Use native interaction semantics

A disclosure is not a menu or dialog. Use a native button and expose its open state with aria-expanded. Keep the controlled container in the DOM so aria-controls has a stable target. Enter and Space already activate a native button. Do not add custom keyboard handling that duplicates native behavior.

A non-modal disclosure does not trap focus. The guide in this capstone contains only reading content, so it adds no new tab stops. Escape is an optional convenience that closes the guide while focus is on the disclosure button. If you later add links or controls inside the guide, revisit focus behavior, Escape handling, and the exit lifecycle.

## Keep the exiting content out of the accessibility tree

AnimatePresence keeps a removed child mounted long enough to run its exit animation. The wrapper can become aria-hidden as soon as the user closes the guide. The exiting child can also read Motion's presence state and set aria-hidden while it is leaving.

Do not place focusable controls inside a subtree that becomes aria-hidden while focus remains inside it. This capstone avoids that case by keeping the guide non-interactive. If a future version adds focusable content, move focus to a meaningful surviving element before hiding the subtree, and make the exiting subtree unavailable for keyboard interaction.

## Respect reduced motion at two levels

Use MotionConfig with reducedMotion="user" around the application so transform and layout movement follow the user preference. Use useReducedMotion for a custom version of this particular transition: do not apply the vertical offset and set its duration to zero when reduction is requested.

A reduced-motion alternative should preserve the information and action. Do not make the guide inaccessible, remove its content, or disable the disclosure control. Check both normal and reduced-motion states in a browser.

## Component boundaries

A small component structure can keep each concern clear:

- The page owns the disclosure's open state.
- The button reports and changes that state.
- The guide wrapper keeps its id and accessible relationship stable.
- The animated child owns its entrance and exit variants.
- MotionConfig sets the application-wide preference policy.
- CSS owns layout, color, type, visible focus, and responsive behavior.

Avoid introducing a general animation framework for one short disclosure. Keep variants close to the component that uses them. If the pattern is repeated across the product, extract a shared component after its behavior is clear.

## Acceptance review

### Semantics and content

- There is a single level-one heading describing the page.
- The disclosure is a real button with a useful name.
- aria-expanded matches the open state.
- aria-controls refers to the persistent guide container.
- The guide has a heading that names its region.
- The guide's information is available as text and is not encoded only by color or motion.

### Keyboard and focus

- Tab reaches the button in a logical order.
- Enter and Space toggle the guide.
- Focus stays visible on the button after the action.
- Escape closes the guide when focus is on the button.
- Closing never strands focus in hidden content.

### Motion and preference

- Normal motion is short, purposeful, and plays only on the state change.
- Reduced motion removes the vertical movement and transition delay.
- The guide still opens, closes, and remains readable with reduced motion.
- No loop, auto-play, parallax, or flashing effect is added.

### Layout and resilience

- The content does not overflow at a narrow viewport.
- Text remains readable at browser zoom.
- The interface still makes sense if animation is unavailable.
- No focus outline is removed without a visible replacement.
- The guide can be opened and closed repeatedly without stale state.

## Suggested implementation order

1. Build the page with static HTML and CSS. Confirm the hierarchy and reading order.
2. Add the button and React state. Verify the accessible name, aria-expanded, and aria-controls without animation.
3. Add the reduced-motion policy at the application root.
4. Add the entrance and exit transition to the guide child.
5. Hide exiting content from the accessibility tree and confirm focus remains on the button.
6. Check keyboard operation, screen-reader output, and the reduced-motion preference.
7. Review narrow layouts and zoom.
8. Add tests for semantic state and keyboard operation. Use a browser for the actual visual transition.
9. Remove any motion that does not improve understanding.

## Common capstone failures

- Making the disclosure a generic div instead of a button.
- Leaving aria-expanded false while the guide is open.
- Pointing aria-controls at an element that only exists after opening.
- Using a large spring or long delay for a simple disclosure.
- Assuming MotionConfig handles every custom animation value.
- Hiding the whole guide from assistive technology before its open state changes.
- Keeping focusable content under aria-hidden during an exit.
- Treating a reduced-motion preference as a reason to disable functionality.
- Adding a modal focus trap to a non-modal disclosure.
- Claiming accessibility from a screenshot or an automated checker alone.

## Completion criteria

The capstone is complete when a person can understand the page, open and close its guide with a pointer or keyboard, perceive the current state, keep track of focus, and use the same information with reduced motion enabled. The animation is an enhancement to the state change, not a prerequisite for using the interface.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="031-testing-motion-components.md">031 - Testing Motion components</a></td><td><a href="../qnas/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface Q&amp;A</a></td><td><a href="../examples/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface example</a></td><td>End of roadmap</td></tr></tbody>
</table>
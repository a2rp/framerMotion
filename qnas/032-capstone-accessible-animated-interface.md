# Capstone: accessible animated interface Q&A

## 1. What does the capstone build?

A responsive resource page with a button-controlled quick guide. The guide opens and closes with a short transition when motion is allowed, and remains fully usable when reduced motion is requested.

## 2. Why is the control a native button?

A button already supports keyboard activation, focus, and an interactive role. A generic element would require rebuilding those behaviors and could still expose the wrong semantics.

## 3. What does aria-expanded communicate?

It communicates whether the content controlled by the disclosure is currently expanded. It must change to true when the guide is open and false when it is closed.

## 4. Why keep the aria-controls target stable?

A stable target lets the button's aria-controls relationship refer to the same guide container in both states. The container can stay mounted while AnimatePresence animates its child.

## 5. Is this guide a dialog?

No. It is a non-modal disclosure in the normal page flow. It does not trap focus or block interaction with the rest of the page.

## 6. Which keyboard keys should operate the disclosure?

Enter and Space activate the native button. The browser provides these behaviors. Do not add custom handlers that duplicate them.

## 7. Why support Escape?

Escape is an optional convenience for closing the guide. In this capstone, the guide has no focusable descendants, so the disclosure button remains focused while it is open. Escape can close it directly from that button.

## 8. Where should focus go when the guide closes?

Focus should remain on the disclosure button. If a future version adds focusable content and can close while focus is inside that content, move focus to the button or another meaningful surviving element before the content becomes unavailable.

## 9. Why can exiting content be an accessibility problem?

AnimatePresence keeps a removed child mounted during its exit. If that child contains interactive controls, they could remain available after the parent state says the disclosure is closed. Hide exiting content from the accessibility tree, prevent keyboard access during exit, and manage focus.

## 10. Why is the guide wrapper separate from its animated child?

The persistent wrapper gives aria-controls a stable target and can reflect the disclosure state immediately. AnimatePresence can animate the inner child out without removing that relationship.

## 11. What does useIsPresent tell the animated child?

It tells a descendant that AnimatePresence is keeping it mounted for exit even though it has been removed from the React tree. The child can use this state to change its accessibility or visual behavior while it exits.

## 12. Does aria-hidden alone disable keyboard interaction?

No. aria-hidden removes content from the accessibility tree but does not itself prevent keyboard focus. Do not put focusable descendants inside an aria-hidden subtree. If interactive content exits, use a proper inert or focus-management strategy as well.

## 13. What does MotionConfig reducedMotion="user" do?

It asks Motion to respect the user's system preference. Motion disables transform and layout animations under its reduced-motion policy, while other values can still animate. The design should explicitly decide what happens to each important movement.

## 14. Why also use useReducedMotion?

A hook lets a component define a specific alternative. The capstone uses it to remove the vertical offset and set the custom transition duration to zero, rather than relying only on a global transform policy.

## 15. Should reduced motion remove the guide?

No. The preference changes unnecessary movement, not the functionality or information. The button, guide, and readable content remain available.

## 16. Is a fade always appropriate for reduced motion?

No single effect is right for every interface. A brief opacity change may be acceptable for one product, while another should make the change immediate. Avoid treating opacity as automatically harmless; consider context and user needs.

## 17. Why avoid a large spring or long delay?

This is a simple disclosure. Excessive bounce, travel, or delay makes the interface harder to follow and can add discomfort. The transition should be brief and should not delay the control or content.

## 18. Should the guide animate every time it scrolls into view?

No. The guide responds to an explicit button action. Repeated automatic or scroll-triggered movement can distract from the page and should not be added without a clear need and a reduced-motion alternative.

## 19. How can the capstone be tested?

Use component tests for button name, aria-expanded, aria-controls, and keyboard activation. Use browser testing for the visual transition, focus behavior, responsive layout, and the actual system reduced-motion preference.

## 20. Can an automated accessibility checker prove the capstone is accessible?

No. Automated tools can catch some structural issues, but they cannot establish that the interaction makes sense, focus remains logical, movement is comfortable, or screen-reader output is useful. Combine automated checks with keyboard, screen-reader, and browser review.

## 21. How should a test verify the animation?

Test user-visible state in component tests. Do not assert an arbitrary intermediate transform. Use a real browser when visual timing or actual motion is part of the acceptance criteria.

## 22. What changes if the guide gets links or buttons?

Revisit the focus and exit design. The example's simple Escape behavior depends on the guide having no tab stops. Interactive content needs a way to reach its controls, a predictable close action, and a plan to prevent focus from remaining in exiting or hidden content.

## 23. What if the user has reduced motion enabled after the page loads?

Motion's reduced-motion hook can respond to preference changes. Verify that the component's custom alternative remains correct if the preference changes while the interface is open or while a transition is underway.

## 24. What is the most important review question?

Can someone understand and use the same control and information with motion disabled? If not, the animation has become a dependency instead of an enhancement.

## 25. When is the capstone complete?

When the button works with pointer and keyboard, its state is announced correctly, the guide is readable, focus remains logical, exiting content is handled safely, the layout works at zoom and narrow widths, and reduced motion removes non-essential travel without removing functionality.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="031-testing-motion-components.md">031 - Testing Motion components Q&amp;A</a></td><td><a href="../lessons/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface lesson</a></td><td><a href="../examples/032-capstone-accessible-animated-interface.md">032 - Capstone: accessible animated interface example</a></td><td>End of roadmap</td></tr></tbody>
</table>
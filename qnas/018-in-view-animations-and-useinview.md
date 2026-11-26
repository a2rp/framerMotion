# In-view animations and useInView Q&A

## 1. What is an in-view animation?

It responds when an element meets a visibility threshold in a viewport. It is useful for a reveal or discrete state change, not a continuous scroll progress effect.

## 2. What is the difference between whileInView and useInView?

`whileInView` animates a Motion component directly. `useInView` returns a boolean React state that can drive a Motion component or other discrete interface logic.

## 3. How do I make a simple reveal?

Give a Motion component an `initial` state and a `whileInView` target, such as opacity 0 to 1 and a small y offset to 0.

## 4. What does viewport.once do?

When true, Motion stops observing after the first entry. The reveal will not reverse when the element leaves and re-enters.

## 5. Does useInView return a Motion value?

No. It returns a boolean React state. Use `useScroll` for normalized continuous progress Motion values.

## 6. What ref does useInView need?

The ref for the element whose visibility should be measured. Attach it to the rendered DOM element and pass the same ref to `useInView`.

## 7. What does amount control?

It specifies how much of the target must be visible: `"some"`, `"all"`, or a number from 0 to 1. The default is `"some"`.

## 8. Why might amount "all" never trigger?

The target may be larger than the visible viewport, so the whole element cannot enter at once. Use `"some"` or a realistic numeric proportion.

## 9. What does margin change?

It expands or contracts the detection boundary around the viewport. It changes when entry/exit is detected; it does not specify the fraction of the target that must be visible.

## 10. What is the margin value order?

It uses CSS order: top, right, bottom, left. For example, `"0px 0px -80px 0px"` contracts the lower boundary.

## 11. How do I observe an item in a scrolling panel?

Pass the ref of the scrollable ancestor as `root` to `useInView`, or as `viewport.root` to `whileInView`. The target must be inside that scrolling root.

## 12. Why is my root not working?

The root ref should point to the actual scrollable element and the target should be its descendant. The root needs a real viewport, usually a constrained size and scrollable overflow.

## 13. What does useInView's initial option do?

It sets the value returned before the target has been measured. The default is false. Set it to true only if the interface should start in the visible state.

## 14. How does once differ from initial?

`initial` controls the pre-measurement value. `once` controls whether observation stops after the first entry. They solve different timing and repetition needs.

## 15. Will useInView become false when the element leaves?

By default, yes, so it can switch on re-entry. With `once: true`, it remains true after the first intersection.

## 16. Should I use useEffect with isInView?

Only when a side effect is needed. For animation alone, render from the boolean or use `whileInView`. Avoid copying the boolean into another state variable.

## 17. Can I trigger a network request when an item enters view?

Technically visibility can drive effects, but scrolling should not trigger irreversible or surprising actions. If data loading is tied to visibility, make it safe to repeat or guard it explicitly.

## 18. Can I use an in-view hook for parallax?

A boolean observer does not provide continuous progress. Use `useScroll` and derived Motion values when the visual must track movement through a range.

## 19. Why does a reveal stay hidden?

Check the initial state, ref placement, viewport options, target size, and scroll root. If content should be visible before JavaScript measurement, choose a visible fallback rather than an initially hidden state.

## 20. Is a hidden initial state good for all content?

No. Essential instructions must remain useful if animation has not started or motion is reduced. Keep the content in the document and use reveal effects as decoration.

## 21. How should reduced motion change a reveal?

Remove or reduce large translations, scaling, or repeated movement. A static visible state or subtle opacity transition may be more comfortable.

## 22. When should a reveal repeat?

Use one-time behavior for common entrance reveals. Repeat only when the changing visibility state is meaningful and the motion remains predictable during repeated scrolling.

## 23. Can I stagger a list of in-view items?

Yes, but keep the sequence short and avoid delaying essential content. A long list should not take a long time to become readable.

## 24. Does entering view move keyboard focus?

No. An in-view animation should not move focus. Keep focus order and visible focus styling under normal keyboard control.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll Q&amp;A</a></td><td><a href="../lessons/018-in-view-animations-and-useinview.md">018 - In-view animations and useInView lesson</a></td><td><a href="../examples/018-in-view-animations-and-useinview.md">018 - In-view animations and useInView example</a></td><td><a href="019-imperative-sequences-with-useanimate.md">019 - Imperative sequences with useAnimate Q&amp;A</a></td></tr></tbody>
</table>

# Scroll-linked animations with useScroll example

This component adds a page-level reading progress line and a subtle fade/vertical shift while one article section passes through the viewport. The two values come from separate `useScroll` calls because they measure different ranges.

## Setup

Install Motion with `npm install motion`. Save as `src/ScrollLinkedArticle.jsx` and render `<ScrollLinkedArticle />` in a page with enough content to scroll.

## `src/ScrollLinkedArticle.jsx`

~~~jsx
import { useRef } from "react";
import {
  motion,
  useReducedMotion,
  useScroll,
  useTransform,
} from "motion/react";

const sections = [
  {
    title: "Plan the measurement",
    body: "A scroll-linked value helps when the visual should correspond to a position in the reading flow. The article stays available before, during, and after the effect.",
  },
  {
    title: "Map progress to a small change",
    body: "A target ref measures one section. Its progress drives a subtle fade and vertical shift without copying each scroll frame into React state.",
  },
  {
    title: "Keep content independent",
    body: "The explanation remains in document order regardless of the animation. Motion decorates the reading experience but does not gate information or controls.",
  },
];

export function ScrollLinkedArticle() {
  const articleRef = useRef(null);
  const shouldReduceMotion = useReducedMotion();

  // Default container: the page viewport.
  const { scrollYProgress: pageProgress } = useScroll();

  // Measure this section's travel through the viewport.
  const { scrollYProgress: articleProgress } = useScroll({
    target: articleRef,
    offset: ["start end", "end start"],
  });

  const opacity = useTransform(
    articleProgress,
    [0, 0.2, 0.8, 1],
    [0.55, 1, 1, 0.55]
  );
  const y = useTransform(articleProgress, [0, 1], [28, -28]);

  return (
    <main
      style={{
        maxWidth: 760,
        margin: "0 auto",
        padding: "72px 24px 160px",
        color: "#172033",
        fontFamily: "system-ui, sans-serif",
        lineHeight: 1.65,
      }}
    >
      <motion.div
        aria-hidden="true"
        style={{
          position: "fixed",
          zIndex: 10,
          inset: "0 auto auto 0",
          width: "100%",
          height: 5,
          background: "#6750a4",
          scaleX: pageProgress,
          transformOrigin: "0% 50%",
        }}
      />

      <header style={{ marginBottom: 72 }}>
        <p style={{ color: "#6750a4", fontWeight: 700 }}>Motion learning note</p>
        <h1 style={{ lineHeight: 1.15 }}>A scroll-linked reading section</h1>
        <p>
          Scroll through the page. The line follows total page progress, while
          the card responds only as its target section passes through view.
        </p>
      </header>

      <section
        ref={articleRef}
        aria-labelledby="article-title"
        style={{
          minHeight: "130vh",
          display: "grid",
          alignItems: "center",
          padding: "48px 0",
        }}
      >
        <motion.article
          style={{
            opacity: shouldReduceMotion ? 1 : opacity,
            y: shouldReduceMotion ? 0 : y,
            padding: 28,
            border: "1px solid #d8dce6",
            borderRadius: 20,
            background: "#f7f5ff",
            boxShadow: "0 16px 48px rgb(28 25 45 / 10%)",
          }}
        >
          <h2 id="article-title">A small effect tied to a real section</h2>
          {sections.map((section) => (
            <div key={section.title}>
              <h3>{section.title}</h3>
              <p>{section.body}</p>
            </div>
          ))}
          <p>
            The text remains in the page at every scroll position. Animation
            changes presentation only.
          </p>
        </motion.article>
      </section>

      <section aria-labelledby="after-title" style={{ paddingTop: 56 }}>
        <h2 id="after-title">After the target</h2>
        <p>
          This content adds scroll distance so the article reaches the end of
          its measured interval while the page progress line continues.
        </p>
        <p>
          With reduced motion enabled, the card remains static and the reading
          indicator still follows the page.
        </p>
      </section>
    </main>
  );
}
~~~

## How it works

The first `useScroll()` tracks the page viewport. `pageProgress` drives the fixed bar's `scaleX` style, which grows from the left because of `transformOrigin`.

The second call receives `articleRef` as its `target`. Progress starts when the section's start meets the viewport's end and completes when its end meets the viewport's start. It measures section travel rather than the entire page.

`useTransform` derives opacity and vertical position. They are attached to the Motion article, so scroll frames do not need to update React state. `useReducedMotion` removes the decorative fade and travel when reduced motion is requested.

## Expected behavior

- The progress line grows continuously as the page scrolls.
- The card fades and shifts slightly while its section enters, crosses, and leaves the viewport.
- The article text remains available throughout the scroll.
- With reduced motion enabled, the card stays still and the progress line remains.

## Adapt the example

- Change offsets to `["start end", "end end"]` to finish when the section's end meets the viewport's end.
- Map progress to a subtle scale range such as `[0.98, 1]`.
- Track a nested panel with a ref on the actual scroll box and pass it as `container`.
- Use `scrollXProgress` and `axis: "x"` for a horizontal scroll region.
- Add spring smoothing only to decorative output; the spring will lag the direct progress.

## Common mistakes

- **No scroll range:** provide enough content or a scrollable container.
- **Wrong ref role:** the section is the `target`; the scrolling element is the `container`.
- **Global and target progress confused:** each hook measures a different scope here.
- **React state updated per frame:** bind Motion values to visual styles.
- **Too much movement:** keep travel small and check reading comfort.
- **Reduced motion ignored:** preserve content and remove unnecessary movement.
- **Decorative bar announced needlessly:** it uses `aria-hidden`; expose progress semantics only when users need that information.

## Practice

1. Replace the offsets with `["start start", "end end"]` and describe how the interval changes.
2. Remove the target option and compare page progress with section progress.
3. Create a constrained nested scroll box and track it with `container`.
4. Remove opacity while keeping position, then decide whether both effects help.
5. Enable reduced motion and confirm the content remains complete.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="016-transforming-and-composing-motion-values.md">016 - Transforming and composing motion values example</a></td><td><a href="../lessons/017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll lesson</a></td><td><a href="../qnas/017-scroll-linked-animations-with-usescroll.md">017 - Scroll-linked animations with useScroll Q&amp;A</a></td><td><a href="018-in-view-animations-and-useinview.md">018 - In-view animations and useInView example</a></td></tr></tbody>
</table>

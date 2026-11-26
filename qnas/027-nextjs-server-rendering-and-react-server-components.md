# Next.js, server rendering, and React Server Components Q&A

## 1. What is a Server Component?

A Server Component is evaluated as part of server rendering. In the Next.js App Router, pages and layouts are Server Components by default. They can fetch data near its source and avoid shipping their implementation as client JavaScript.

## 2. What is a Client Component?

A Client Component belongs to the client module graph and can use state, event handlers, effects, browser APIs, and client-side hooks. It can still be pre-rendered to HTML on the initial route load and later hydrated in the browser.

## 3. How do I mark a Client Component in Next.js?

Put `"use client"` at the very beginning of the file, before imports or other executable code. It marks the boundary of the client module graph.

## 4. Do I need to add use client to every component below that boundary?

No. The directive marks a client entry point. Its imported modules are included in the client module graph, so nested components do not each need a duplicate directive.

## 5. Which Motion import should I use in a Server Component?

Use Motion's React Server Components entry point:

~~~tsx
import * as motion from "motion/react-client";
~~~

This is the documented entry point for Motion components imported from RSC modules, including Next.js App Router files.

## 6. Which Motion import should I use in a stateful Client Component?

Use the normal React entry point:

~~~tsx
import { motion } from "motion/react";
~~~

The component must be inside a client module boundary if it uses React state, event handlers, or client-only hooks.

## 7. Does motion/react-client replace use client?

No. It provides Motion components that can be imported from an RSC module. If your own component uses state, effects, event handlers, or browser APIs, it still needs a Client Component boundary.

## 8. Can a Motion component render on the server?

Yes. Motion components support server rendering, and their initial state is reflected in generated output. The server can render useful HTML before client JavaScript finishes loading.

## 9. Does server rendering mean the animation has completed in the browser?

No. Server rendering describes the HTML generated for the initial response. Browser-side animation and event handling still depend on the appropriate client code and hydration.

## 10. Why use initial={false} for a server-rendered visual?

It makes the animation target the initial rendered state instead of emitting a hidden or offset initial state. This can keep important content visible when the client animation is delayed or unavailable.

## 11. Do Client Components render on the server?

They can be pre-rendered to HTML for the initial page load. The browser then hydrates them to attach event handlers and make them interactive. Client Component describes its capabilities and module boundary, not a promise that it never participates in server rendering.

## 12. What is hydration?

Hydration is the browser process that connects client JavaScript and event handlers to the server-generated HTML. The first client render should agree with the server output to avoid hydration mismatches.

## 13. What kind of props can a Server Component pass to a Client Component?

Pass values React can serialize across the component boundary, such as strings, numbers, booleans, arrays, and plain data records. A server-created function is not an ordinary serializable callback prop.

## 14. Can a Server Component pass a click handler to a Client Component?

Not as an ordinary function prop created during server rendering. Define the event handler inside the Client Component, or use a supported Server Function pattern for server-side mutations.

## 15. Can a Client Component render server content?

Yes. A Server Component parent can render a Server Component and pass its rendered result through a Client Component slot such as `children`. The client module should not import the server-only module directly.

## 16. Should I mark an entire layout or page as a Client Component?

Usually mark only the parts that need client behavior. A narrow client boundary keeps static structure and server data work out of the client module graph.

## 17. Can I read window or localStorage during Server Component rendering?

No. These are browser APIs and are unavailable on the server. Read them from a Client Component in an appropriate client-side lifecycle or hook, and keep the initial output deterministic.

## 18. How do I avoid a hydration mismatch in an animated component?

Use stable initial values that produce the same output on the server and first client render. Avoid render-time browser reads, random values, and server/client-specific time values. Apply browser-dependent changes after the client boundary is active.

## 19. Does motion/react-client support every Motion feature in a Server Component?

Do not assume that every hook or callback can be used directly from a Server Component. Use the RSC entry point for Motion components, and move stateful behavior or browser-only logic into a Client Component. Verify less common component props against the installed Motion version.

## 20. How can Motion keep client JavaScript limited?

Keep server data fetching and static content in Server Components. Use Motion components from the RSC entry point for server module usage, and make only the interactive animation controls Client Components.

## 21. What is the React Server Component payload?

It is the serialized representation used by the framework to reconcile server-rendered component output with client components. It includes rendered Server Component results, client component references, and serializable props crossing the boundary.

## 22. Can I use a Motion hook in a Server Component?

A hook that requires client state, effects, or browser behavior belongs in a Client Component. Import the hook from the React entry point inside a module marked with `"use client"`.

## 23. What is a good Next.js Motion checklist?

- Keep App Router pages and layouts server-side by default.
- Use `motion/react-client` for Motion components imported from a Server Component.
- Use `"use client"` and `motion/react` for stateful or event-driven animation.
- Pass only serializable ordinary props across the server/client boundary.
- Keep server-only data and secrets in server modules.
- Keep important content visible in its server-rendered state.
- Test the production route for hydration warnings and verify the client behavior.

## 24. What should I practice?

Render a static page section with `motion/react-client`. Add a small expandable Motion control as a Client Component that receives only string props. Inspect the server-rendered page and verify the button works after hydration.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="026-bundle-optimization-with-lazymotion.md">026 - Bundle optimization with LazyMotion Q&amp;A</a></td><td><a href="../lessons/027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components lesson</a></td><td><a href="../examples/027-nextjs-server-rendering-and-react-server-components.md">027 - Next.js, server rendering, and React Server Components example</a></td><td><a href="028-react-router-transitions-and-navigation.md">028 - React Router transitions and navigation Q&amp;A</a></td></tr></tbody>
</table>

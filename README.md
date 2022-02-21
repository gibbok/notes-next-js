# notes-next-js

Next.js is an open-source development framework build on top of Node.js, it provide server-side rendering and generating of static websites.
The Next.js framework utilizes Jamstack architecture.

Next.js use Babel for transpilation and Webpack for bundling.
It use page-based routing and dynamic routing.

It support incremental static regeneration ISR and static site generation SSG.


## Core features

- Page based routing system with support of dynamic orutes
- Pre rendering like static generation SSG and server side rendering SSR both supported for per-page basis
- Code splitting for last load
- Client side routingn with optmimized fetching
- Fash refresh


## Core concepts

- A page i a React component exported from a file in the `pages` directory
- Pages are assocaited with the route base of their file name, for instance `pages/index.js` is associated with `/` or `pages/posts/first.js` is assocaited with `/posts/first`
- A component/page *must be sported as `default`*
- Link between pages must be done using the `Link` Component from next/link, this performes client-side navigation using JavaScript
- Next.js does code splitting automatically, so each page only loads when necessary (fast loading), so each page become isolated so one error in one page should not affect other pates. In production code when the `Link` componet appear in the view port, Next.js autoamtically prefetch the code for the linked page in the background, for almost instand load.

## Others
- Next.js serves static assets under the top-level public directory, files inside public can be referenced from the root of the application similar to pages.

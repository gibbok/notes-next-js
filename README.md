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
- Link between pages must be done using the `Link` Component from next/link

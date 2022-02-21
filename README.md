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
- 
- `Image` component from Next.js performes, optimize image, image is responsive, only load iamge when in the viewport.
- `Image` are served in WebP format when the browser support it
- Automatic Image Optimization works with any image source. Even if the image is hosted by an external data source
- It optmizes images on demand instead o build time, iamges are lazy loaded by default, so images outside the viewport are not loaded, images are awlays renderedin in a way to avoid cumulative layout shift

- It is possible to decide the HTML meta data we want by using the `Head` component
- It is possible to lead exernal JS into the page by using the `Script` component, which can also perform the loading using different strategy, `lazyOnload` loads s c ript on browser idle time, `onLoad` run after script has finished loading

- It support styled jsx, sas and CSS modules (the CSS file name must end with .module.css.). Nextjs Automatically Generates Unique Class Names
- Nexths splitting feature works on CSS modules, to enure the niminal amount of CSS is loaded for each page and result in smaller bunderle sizes
- Global styles should be added in `pages/_app.js` file
- Next.js compiles CSS using PostCSS. To customize PostCSS config, you can create a top-level file called postcss.config.js

## Others
- Next.js serves static assets under the top-level public directory, files inside public can be referenced from the root of the application similar to pages.

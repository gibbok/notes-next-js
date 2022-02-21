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
- Client side routing with optmimized fetching (pre-fetching)
- Fast refresh


## Core concepts

### Pages
- A page i a React component exported from a file in the `pages` directory
- Pages are assocaited with the route base of their file name, for instance `pages/index.js` is associated with `/` or `pages/posts/first.js` is assocaited with `/posts/first`
- A component/page must be sported as `default`
- Link between pages must be done using the `Link` Component from next/link, this performes client-side navigation using JavaScript
- Next.js does code splitting automatically, so each page only loads when necessary (fast loading), so each page become isolated so one error in one page should not affect other pates
- In production code when the `Link` componet appear in the view port, Next.js autoamtically prefetch the code for the linked page in the background, for almost instand load.

### Images
- `Image` component from Next.js performes, optimize image, image is responsive, only load images when in the viewport
- `Image` are served in WebP format when the browser support it, even if the source is in a different format
- Automatic Image Optimization works with any image source. Even if the image is hosted by an external data source
- It optmizes images on demand instead o build time, iamges are lazy loaded by default, so images outside the viewport are not loaded, images are always renderedin in a way to avoid cumulative layout shift
- Next.js serves static assets under the top-level public directory, files inside public can be referenced from the root of the application similar to pages.


### Meta
- It is possible to decide the HTML meta data we want by using the `Head` component
- It is possible to load exernal JS into the page by using the `Script` component, which can also perform the loading using different strategy, `lazyOnload` loads s c ript on browser idle time, `onLoad` run after script has finished loading

### Styling
- It support styled jsx, sass and CSS modules (the CSS file name must end with .module.css.)
- It automatically generates Unique Class Names
- It splitting feature works on CSS modules, to ensure the minimal amount of CSS is loaded for each page and result in smaller bunderle sizes
- Global styles should be added in `pages/_app.js` file
- Next.js compiles CSS using PostCSS. To customize PostCSS config, you can create a top-level file called postcss.config.js

### Pre-rendering
- It pre-render every page by default, so Nextjs generate HTML for each page in advanced instead of having it all done on the client side using Js
- Pre-rendering can result in better performance and SEO
- Each generated HTML is assocaited with a minimal JS code for what page, when the page load the browser laods it JS and make the page interactive (process is called hydfrataion).
- Initial load: pre-rendered HTML created by next js is displayed
- Hydratioan: React componmtes are initialized and the app become interactice

Ther are two forms of pre-rendering:
- Static Generation: pre-rendering method were HTML is generated at build time (with data or no data)
- Server-side Rendering: pre-rendering method were HTML is generated on each request (default mode in dev mode)

Next.js can support a hybrid method, let the developer choosewhich pre-rendering form to use for each page.

Reccomandation: use Static Generation with or without data when posible so it can be served from CDN (instance marketing pages, blog posts, doc, e-commerce product listing)
Use Server-side Rendering for page where data is very frequently updated or change on every request. it is possible to skip pre-rendering and use client-side JS for frequently updated data

#### Static generation without data fetching
For pages which requires no ethcing of external data, those are created at build time.

#### Static generation with data fetching
For pages pages which can be generated after fetching exernal data at build time. It can be used for pre-render page ahead of a user's request.

#### Static generation with data using `getStaticProps`
The `getStaticProps` is an synch function which runs at build time, in that function we can fetch external data and pass to the props of the component for that page.
Notes:
- Next.js polyfills `fetch()` by default on both client and server.
- `getStaticProps` only runs on the server-side. It will never run on the client-side. It won’t even be included in the JS bundle for the browser. That means you can write code such as direct database queries without them being sent to browsers.
- in dev mode `getStaticProps` run on each request
- in prod mode `getStaticProps` run at build time
- `getStaticProps` can be only exported from a page

### Server side rendering
Fetch data at request time instead of build time, by  exporting  `getServerSideProps` instead of `getStaticProps` from a page.
`getServerSideProps` parameter (context) contains request specific parameters.

Time to first byte (TTFB) will be slower than `getStaticProps` because the server must compute the result on every request, and the result cannot be cached by a CDN without extra configuration.

### Client side rendering
USe if you do not need pre-render the data, which consist of pre-render some static part of the page, when the page load usign JS to populate the remaining parts. Useful for project where the data is updated frequently, or private projets where SEO is not relevant.

##  Dynamic URLs using dynamic routes
Statically generate pages with paths that depend on external data, for instance defautls for a blog post `/posts/<id>,` where `<id>` is the name of markdown file present in a folder at build time.

- In Nextjs pages that begin with [ and end with ] are dynamic routes in Next.js.
- More on [feedback](https://nextjs.org/learn/basics/dynamic-routes/dynamic-routes-details) behaviour here
feedback = false: 404 is returned it no result found
feedback = true: pathe will be rendered, or nextjs will server fallback cersion of the path, in backround next will statically generate the request page on next request the pre-renedered content will be show
feedback = blocking, it will apply server side rendering and cached for further requess
 
 
## API Routes
API routes provide a solution to build your API endpoint with Next.js for instance in a new project you could build your entire API with API Routes.
 
Do Not Fetch an API Route from getStaticProps or getStaticPaths instead use helper function, becasue both funcions are running at the server-side and never at the client side.

Notes: The API Route code will not be part of your client bundle, so you can safely write server-side code which cannot be seen in the client.
 
## Others

### Front Matter
Is a [metadata](https://jekyllrb.com/docs/front-matter) section used in MarkDown file

